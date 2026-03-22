# Community CC Catalog (yagpdb-cc.github.io)

Reference of all available community custom commands with trigger types and key configuration.

## Fun Commands

### Counting System
**Basic** — Regex trigger `\A`, run only in counting channel.
- Config: `$channel` (channel ID)
- Validates sequential numbers, tracks per-user counts in DB

**Advanced V2** — Enhanced counting with more features, wrong-count handling, milestones.

### Starboard
**V1** — Single CC, reaction trigger (both). Config: `$starEmoji`, `$starLimit`, `$starboardChannel`
**V2** — Two CCs (main-cc + reaction-handler). More robust, supports embed updates, star count tracking.

### Question of the Day (QOTD)
**Basic** — Single interval CC. Config: `$channel`, `$questions` (cslice of strings)
**Advanced** — 4 CCs: main-cc, component-handler, interval, modal-handler. Staff can submit questions via modal, automated posting.

### Connect Four
2 CCs: start-game (command trigger) + reaction-handler. Full game with board rendering.

### Anonymous Channel
3 CCs: main-cc (regex), component-handler, modal-handler. Users submit messages anonymously via button+modal.

### Cards Against Humanity Groups
5 CCs: newgame, endgame, setgroup, delgroup, listgroups. Manage pack groups for YAGPDB's built-in CAH.

### Simple Fun Commands
| Command | Trigger | Description |
|---------|---------|-------------|
| `animal` | Command | Random animal images (duck, fox, cat, goat, shiba, httpcat) |
| `choose` | Command | Random selection from provided items |
| `coinflip` | Command | Coin flip with currency integration |
| `deathmatch` | Command | Turn-based combat between users |
| `duck` | Command | Random duck images |
| `funinfo` | Command | Stats for counting/qotd/story systems |
| `guess-the-number` | Regex | Number guessing game (1-100) with currency |
| `maze` | Command | Random maze generator |
| `meme` | Command | Meme generator via memegen.link |
| `mock` | Command | sPoNgEbOb text |
| `repeat` | Command | Repeat a phrase N times |
| `slot-machine` | Command | Slot machine with currency |
| `tte` | Regex | Text to emoji conversion |
| `uwuify` | Command | UwU text transformation |
| `wheel-of-fortune` | Regex | Wheel of fortune with currency |
| `x-word-story` | Regex | Collaborative story (X words per turn) |

## Moderation Commands

### Standalone
| Command | Trigger | Description |
|---------|---------|-------------|
| `emote-filter` | Regex | Filter excessive emotes |
| `lockdown` | Command | Lock/unlock channels |
| `nickname-moderation` | Regex | Filter inappropriate nicknames |
| `notes` | Command | Staff notes on users (DB-backed) |
| `slowmode` | Command | Custom slowmode settings |
| `staff-on-duty` | Command | Track which staff are on duty |

### Raid Guard (2 CCs)
Join trigger + admin command. Detects raid patterns, auto-bans.

### Custom Report System (3 CCs)
Custom report + cancel-report + reaction-handler. Enhanced reporting with reaction-based actions.

## Giveaway Systems

### Basic (2 CCs)
Main CC (command) + reaction handler. Simple giveaway with reactions.

### Basic V2 (2 CCs)
Improved version with more features.

### Compressed (2 CCs)
All code compressed into minimal CCs. Supports: start, end, cancel, reroll, list.
Config: flags `-p` (max participants), `-w` (max winners), `-c` (channel)

## Leveling System (7 CCs)
Full XP/leveling with role rewards.
- `configure-role-rewards` — `-rr add <level> <role>`, `-rr set-type stack|highest`
- `configure-settings` — `-leveling set cooldown/min/max`
- `message-handler` — Regex trigger, awards XP on messages
- `reaction-handler` — Awards XP on reactions
- `set-xp` — Admin XP/level manipulation
- `view-leaderboard` — `-lb [page]`
- `view-rank` — `-rank [user]`
Default formula: `f(x) = 100x²`

## Suggestion System (1 CC)
Main CC for submitting/managing suggestions with reactions.

## Tag System (2 CCs)
Main CC + reaction handler. Create/retrieve/manage text tags.

## Birthday System (1 CC)
Track and announce member birthdays.

## AFK System (2 CCs)
Main CC + leave feed. Set AFK status, notify when mentioned.

## Info Commands
| Command | Trigger | Description |
|---------|---------|-------------|
| `avatar` | Command | User avatar display |
| `channel` | Command | Channel information |
| `server` | Command | Server information |
| `user` | Command | User information |

## Utility Commands
| Command | Trigger | Description |
|---------|---------|-------------|
| `big-emoji-v1/v2` | Regex | Enlarge emojis |
| `bookmark` | Command | Bookmark messages to DM |
| `cc2file` | Command | Export CC code as file |
| `color-preview` | Command | Preview hex colors |
| `display-struct` | Command | Inspect data structures |
| `edit` | Command | Edit bot messages |
| `flag-message` | Reaction | Flag messages for review |
| `ghost-ping-v1/v2` | Regex | Detect deleted mentions |
| `json-converter` | Command | Convert data to JSON |
| `random-color` | Command | Generate random color |
| `reaction-logs` | Reaction | Log reactions |
| `reactionbookmark` | Reaction | Bookmark via reaction |
| `send-message` | Command | Send message as bot |
| `snipe-message` | Command | View last deleted message |
| `view-time` | Command | View time in timezone |
| `world-clock` | Command | Multiple timezone display |

## Code Snippets (Reusable Utilities)
| Snippet | Description |
|---------|-------------|
| `binary-search` | Binary search implementation |
| `get-username-color` | Get user's role color |
| `ordinal` | Number to ordinal (1st, 2nd, 3rd) |
| `parse-flags` | Parse command flags (-flag value) |
| `parse-text` | Advanced text parsing |
| `string2time` | Convert string to time object |
