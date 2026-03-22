# Image Contest System

A fully automated image contest system for YAGPDB. Community members suggest prompts, vote on them, and the top prompt becomes a daily image contest run in a Discord forum channel. Submissions are collected, voting opens, and the winner gets a temporary role and a spotlight in the winners channel.

## Contest Cycle

```
Day 1 noon: Crontab fires -> picks top-voted prompt -> creates forum thread
            |-- 24h submission window (images only, no reactions allowed)
Day 2 noon: Bot transitions to voting -> pre-reacts vote emojis -> voting opens
            |-- New contest can start simultaneously (overlapping supported)
            |-- 24h voting window (react to vote, no new posts allowed)
Day 3 noon: Bot counts votes -> announces winner -> gives role -> locks thread
            |-- N days later: winner role auto-removed
```

Contests can overlap: when a submission phase ends and voting begins, the next day's contest can start its own submission phase. Each contest thread tracks its own state independently.

## Custom Commands Overview

| # | File | Trigger | Purpose |
|---|------|---------|---------|
| 1 | `01-prompt-suggest.go.tmpl` | Command: `suggest` | Users submit prompt ideas with `-suggest Title \| Description` |
| 2 | `02-contest-cycle.go.tmpl` | Crontab (e.g. `0 12 * * *`) | Picks top prompt, creates forum thread, starts submission phase |
| 3 | `03-contest-phase-handler.go.tmpl` | Command: `ic_internal` | Handles voting transition, winner selection, and role removal |
| 4 | `04-submission-tracker.go.tmpl` | Regex: `\A` | Tracks submissions, enforces image-only posts, cleans channels |
| 5 | `05-reaction-guard.go.tmpl` | Reaction: Added Only | Blocks reactions during submissions, blocks self-votes during voting |
| 6 | `06-remove-suggestion.go.tmpl` | Command: `removesuggestion` | Mod command to delete a prompt suggestion |

## Setup

### 1. Create Discord Infrastructure

- **Forum Channel** -- Where contest threads are created. The bot needs Send Messages, Manage Threads, Add Reactions, and Manage Messages.
- **Suggestions Channel** -- Where users run `-suggest`. The bot needs Send Messages, Add Reactions, and Manage Messages. All non-bot messages are auto-deleted to keep it clean.
- **Winners Channel** -- Where winner announcements are posted.
- **General Discussion Channel** -- Where text-only posters are redirected during contests. Can be a channel or thread.
- **Contest Winner Role** -- A role for winners. Must be below YAGPDB's role in the hierarchy.

### 2. Create a CC Group

Create a group called "Image Contest" in the YAGPDB control panel under Custom Commands.

### 3. Add the Custom Commands

Create each CC in order. **Write down each CC's ID as you go** -- CC2 needs CC3's ID for scheduling.

### 4. Configure Each CC

Every CC has a `CONFIGURATION` section at the top. Fill in the channel/role IDs:

| Variable | Used In | Description |
|----------|---------|-------------|
| `$SUGGESTIONS_CHANNEL` | CC 1, 2, 3, 4 | Suggestions channel ID |
| `$FORUM_CHANNEL` | CC 2, 4, 5 | Contest forum channel ID |
| `$GENERAL_DISCUSSION_CHANNEL` | CC 4 | Channel/thread ID for general discussion redirect |
| `$PHASE_HANDLER_CC_ID` | CC 2 | The CC ID of CC 3 (the phase handler) |
| `$WINNERS_CHANNEL` | CC 3 | Winners announcement channel ID |
| `$WINNER_ROLE_ID` | CC 3 | Contest winner role ID |
| `$ROLE_DURATION_DAYS` | CC 3 | Days the winner role lasts (default: 3) |
| `$VOTE_EMOJIS` | CC 3 | Emojis for voting (default: heart) |
| `$SUGGESTION_EMOJI` | CC 1, 2 | Emoji for voting on suggestions (default: heart) |
| `$SUBMISSION_HOURS` | CC 2 | Submission window in hours (default: 24) |
| `$VOTING_HOURS` | CC 2, 3 | Voting window in hours (default: 24) |
| `$MOD_ROLES` | CC 4, 6 | cslice of mod/admin role IDs |

### 5. Channel Restrictions

- **CC 4 (Submission Tracker):** Must be allowed in **both** your contest forum channel **and** your suggestions channel. It auto-cleans the suggestions channel and tracks submissions in contest threads. Threads created on suggestion embeds (for discussing prompts) are left alone.
- **CC 5 (Reaction Guard):** Restrict to your contest forum channel, or rely on the in-code `$FORUM_CHANNEL` check.
- **CC 3 (Phase Handler):** No restrictions needed -- the ExecData guard prevents manual triggering.

## How Each CC Works

### CC1: Prompt Suggest

Users run `-suggest Post Title | Description` in the suggestions channel. The pipe `|` is required and separates the short title (max 200 chars) from the creative description (max 1500 chars). Both sides are required.

The bot:
1. Deletes the user's command message
2. Posts an embed with the suggestion
3. Adds a vote reaction
4. Stores the suggestion in the database

Validation errors (missing pipe, too long, wrong channel, etc.) are shown as self-deleting messages.

### CC2: Contest Cycle

Runs on a crontab schedule. Skips if a submission phase is already in progress (`ic_active_sub` exists) or if there are no prompt suggestions in the database.

When it fires:
1. Reads all `ic_prompt_%` entries from the DB
2. Fetches each suggestion's Discord message to count vote reactions
3. Picks the prompt with the most votes
4. Creates a forum thread titled `YYYY-MM-DD Theme Title`
5. Posts the opening and description as the forum post body
6. Sends the rules and timing info as a follow-up message in the thread
7. Stores per-thread state (`ic_thread_<id>`, `ic_subs_<id>`, `ic_active_sub`)
8. Schedules CC3's `open_voting` action after the submission window
9. Deletes the winning prompt's suggestion (embed + DB entry)
10. Posts a 24h self-deleting announcement in the suggestions channel

The forum post body and rules are split into two messages to stay within Discord's 2000 character limit, allowing longer prompt descriptions.

### CC3: Phase Handler

Called only via `scheduleUniqueCC`. The ExecData guard rejects manual execution. Handles three actions:

**`open_voting`** -- Transitions from submission to voting:
1. Deletes `ic_active_sub` (frees the slot so the next contest cycle can start)
2. Updates the thread's state to "voting"
3. Pre-reacts with vote emojis on every tracked submission
4. Posts a "Voting is Now Open!" embed in the thread
5. Schedules `pick_winner` after the voting window

**`pick_winner`** -- Selects the winner:
1. Reads submissions from `ic_subs_<threadID>`
2. Fetches each submission message, counts vote reactions (minus the bot's own)
3. Skips mod/admin entries (they can submit but can't win)
4. All entries tied for the most votes win together
5. Gives winner role, posts announcement with images to the winners channel (includes a link back to the contest thread)
6. Posts a summary in the contest thread
7. Locks and archives the thread
8. Cleans up per-thread DB keys
9. Schedules `remove_role` after the role duration

If no one receives any votes, a "No Winner" announcement is posted instead.

**`remove_role`** -- Removes the winner role from all winners after the configured duration.

### CC4: Submission Tracker

Fires on every message in the suggestions channel and contest forum threads (regex `\A` matches everything).

**Suggestions channel:** Deletes all non-bot messages to keep it clean. Threads attached to suggestion embeds (where ParentID = suggestions channel) are allowed through.

**Contest threads (submission phase):**
- **Text-only messages** (no image attachment or image URL) are deleted. A self-deleting message redirects the user to the general discussion channel.
- **Image posts** (file upload or URL ending in .png/.jpg/.jpeg/.gif/.webp) are tracked as submissions.
- **One entry per user:** If a user already has a tracked submission and their original message still exists, the new post is deleted with a notice: "Multiple entries are not allowed. Please delete your current entry if you wish to post something else."
- **Re-entry after deletion:** If a user deleted their original submission, their new image is accepted and tracked as a replacement.
- **Mod/admin detection:** Submissions from users with mod roles are flagged (`isMod: true`) so CC3 can exclude them from winning.

**Contest threads (voting phase):** All messages are deleted to keep the thread clean for voting.

**Non-contest threads / idle phase:** Ignored entirely.

### CC5: Reaction Guard

Fires on every reaction added in the contest forum channel.

**Submission phase:** Removes ALL non-bot reactions and sends a self-deleting notice explaining that voting hasn't opened yet.

**Voting phase:** Checks if the user is reacting to their own submission. If so, removes the reaction and notifies them: "You can't vote for your own submission." All other reactions are allowed through.

### CC6: Remove Suggestion

Mod-only command to remove a prompt suggestion. Usage:
- `-removesuggestion <message ID>` -- pass the suggestion embed's message ID
- Reply to a suggestion embed and run `-removesuggestion`

Deletes both the database entry and the Discord embed message.

## Vote Emoji Configuration

Set `$VOTE_EMOJIS` in CC3 to control which emojis the bot uses for voting.

**Default (single heart):**
```
{{ $VOTE_EMOJIS := cslice "heart_emoji" }}
```

**Multiple emojis:**
```
{{ $VOTE_EMOJIS := cslice "heart_emoji" "fire_emoji" "star_emoji" }}
```

**Custom server emoji:**
```
{{ $VOTE_EMOJIS := cslice "customheart:123456789012345678" }}
```

> **Limit:** Each submission x each emoji = 1 `addMessageReactions` call. YAGPDB allows 20 per CC execution. With 1 emoji, you're limited to 20 submissions per contest getting pre-reacted. Submissions beyond 20 will still be tracked and counted -- they just won't have the bot's pre-react on them.

## Database Keys

| Key | Purpose |
|-----|---------|
| `ic_active_sub` | Thread ID of the contest currently in submission phase. Deleted when voting opens. |
| `ic_thread_<threadID>` | Per-thread contest state: phase, round, prompt, promptAuthor, startedAt |
| `ic_subs_<threadID>` | Per-thread submissions: userID -> {msgID, isMod} |
| `ic_round` | Global round counter (incremented each contest) |
| `ic_prompt_<msgID>` | Individual prompt suggestions: title, desc, authorID, msgID, channelID |

### Resetting Contest State

To fully reset the contest system, delete these DB keys (all with user ID 0):
- `ic_active_sub`
- Any `ic_thread_*` entries
- Any `ic_subs_*` entries
- `ic_round` (optional -- resets the challenge number)
- Any `ic_prompt_*` entries (optional -- clears all suggestions)

## Limits and Constraints

**Free tier:**
- 10 DB calls per CC execution
- 1 `scheduleUniqueCC` per CC execution
- 20 `addMessageReactions` per CC execution (caps pre-reacting at 20 submissions)
- 100 generic API calls per CC execution
- 100 prompt suggestions max per `dbGetPattern` call

**Premium tier raises:**
- 50 DB calls per CC execution
- 10 `scheduleUniqueCC` per CC execution

**Discord limits:**
- Forum thread titles: 100 characters (auto-truncated)
- Message content: 2000 characters (forum post body has a safety truncation)
- Embeds per message: 10 (winner announcement uses 1 text + 1 per winner image)

## Known Behaviors

- **Overlapping contests:** A new submission phase can start while a previous contest is still in voting. Each contest thread has its own independent state and submissions.
- **Mod submissions:** Mods/admins can submit images (they're tracked and pre-reacted) but are excluded from winning.
- **Self-voting:** Users cannot vote for their own submission during the voting phase -- the reaction is removed.
- **Suggestion persistence:** Only the winning prompt is deleted each cycle. All other suggestions persist and compete again in the next cycle.
- **Image detection:** Both file uploads and URLs with image extensions (.png, .jpg, .jpeg, .gif, .webp) count as valid submissions. Plain URLs without image extensions (e.g. imgur gallery links) are treated as text.
- **Tie handling:** All entries tied for the most votes win together. Each winner's image is shown in the announcement.
- **No votes:** If no submissions receive any votes, a "No Winner" round is announced.
