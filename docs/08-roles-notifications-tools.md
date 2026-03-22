# Roles, Notifications & Tools Reference

## Roles

### Autorole
Auto-assign a role to new members.
- Optional delay (X minutes after join)
- Prerequisite roles / Exclusion roles
- One-time assignment option
- Screening requirement option
- Retroactive scan (premium)

### Self-Assignable Roles
Members toggle roles via `-role <rolename>` command.
- Organized into groups with modes: None, Single (1 per group), Multiple (min/max)
- **Role Menu:** Reaction-based role assignment
  - `-rolemenu create <group>` — Initialize
  - `-rolemenu update <msgID>` — Add new roles
  - `-rolemenu edit <msgID>` — Change emojis
  - `-rolemenu remove <msgID>` — Delete menu
  - Max 20 roles per menu (Discord reaction limit)
  - Flags: `-nodm` (no DM), `-rr` (remove on reaction remove)

### Bulk Role (Premium)
Mass assign/remove roles with filters:
- All members, bots only, humans only
- Has/missing specific roles
- Joined before/after date

### Voice Roles
Auto-assign role when joining voice/stage channel, auto-remove on leave.
- 1 free, 10 premium

## Notifications

### General
- **Join Message** — Channel + template when user joins
- **Leave Message** — Channel + template when user leaves
- **Join DM** — DM to new members
- **Topic Change** — When channel topic modified
- All support custom command template syntax

### YouTube
- Max 10 feeds (250 premium)
- Template: `{{.ChannelName}} published a new video! {{.URL}}`
- Context: `.ChannelID`, `.ChannelName`, `.VideoID`, `.VideoTitle`, `.VideoThumbnail`, `.VideoDescription`, `.VideoDurationSeconds`, `.URL`, `.IsLiveStream`, `.IsUpcoming`

### Twitch
- Max 3 feeds (15 premium)
- Template: `{{ .User }} is now live on Twitch! {{ .URL }}`
- Context: `.User`, `.URL`, `.Title`, `.Game`, `.IsLive`, `.VODUrl`

### Reddit
- Enter subreddit name (no `r/` prefix)
- Embed or plain text display

### RSS
- Max 2 feeds (10 premium)
- Posts may take up to 5 minutes to appear

### Streaming
- Triggers on Discord streaming status
- Template vars: `{{ .URL }}`, `{{ .Game }}`, `{{ .StreamTitle }}`, `{{ .User.Username }}`
- Optional: Game regex filter, title regex, streaming role
- Known issue: Announcements may not always post

## Tickets
Private channels between member and staff.

### Commands
- `-tickets open <subject>` — Create ticket
- `-tickets close [reason]` — Close ticket
- `-tickets adduser <member>` — Add member
- `-tickets removeuser <member>` — Remove member
- `-tickets rename <name>` — Rename
- `-tickets adminonly` — Toggle admin-only
- `-tickets menucreate` — Create ticket menu (up to 9 reasons)

### Settings
- Admin/Mod roles
- Ticket category (max 50 channels)
- Threaded tickets (premium, uses private threads)
- Transcript channel (optional attachment ZIP)
- Status update channel
- Customizable opening message (supports CC templates, `.Reason` context)

## Reputation System
Track user reputation scores.

### Commands
- `-giverep <user> [num]` / `-takerep <user> [num]`
- `-rep [user]` — Show rep
- `-toprep [page]` — Leaderboard
- `-setrep <user> <num>` — Admin set
- `-delrep <user>` — Remove from system
- `-replog [user] [page]` — Transaction history

### Settings
- Auto "thanks @user" recognition
- Custom point name
- Cooldown (seconds)
- Max per transaction
- Required/restricted giver/receiver roles
- Role rewards at thresholds (5 free, 25 premium)

## Soundboard
Play sounds in voice channels via `-sb <name>`.
- Upload files or URLs
- Per-sound role restrictions
- Sounds: 50 free, 250 premium

## Other Useful Commands
- `-calc <expression>` — Calculator
- `-currenttime [zone]` — Time in timezone
- `-listroles` — All roles with IDs
- `-poll <topic> <opt1> <opt2> ...` — Reaction poll (up to 10)
- `-undelete` — View deleted messages
- `-whois [user]` — User info
- `-remindme <duration> <message>` — Schedule reminder
- `-settimezone <tz>` — Set personal timezone
- `-stats` — Server statistics
- `-evalcc <code>` — Test CC code
- `-diagnosecctriggers [input]` — Debug CC triggers
