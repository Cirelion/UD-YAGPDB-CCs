# Moderation Reference

## Built-in Moderation Commands

### Timeout
`-timeout <user> [duration] [reason]` — Discord native timeout (60s to 4 weeks). Default: 10 min.
`-removetimeout <user> [reason]` — Cancel timeout.

### Mute
`-mute <user> [duration] [reason]` — Assigns mute role. Optional temporary duration.
`-unmute <user> [reason]` — Remove mute.

### Kick
`-kick <user> [reason] [-cl]` — Remove from server. `-cl` deletes last 100 messages.

### Ban
`-ban <user> [duration] [reason] [-ddays N]` — Ban. Optional duration and message deletion (0-7 days).
`-unban <user> [reason]` — Unban.

### Warnings
- `-warn <user> <reason>` — Create warning
- `-warnings <user> [page]` — List warnings
- `-editwarning <id> <new message>` — Edit warning
- `-delwarning <id> [reason]` — Delete warning
- `-clearwarnings <user> [reason]` — Clear all
- `-topwarnings [page]` — Warning leaderboard

### Clean
`-clean <num> [user] [-r regex] [-im] [-ma maxage] [-minage] [-nopin] [-a] [-bots]`
Bulk delete messages. Searches last 1,000 messages, max 2 weeks old.

### Role Management
- `-giverole <user> <role> [duration]` — Assign role (optional expiry)
- `-removerole <user> <role>` — Remove role

## Moderation Settings

### Channels
- **Mod Log** — Records all moderation actions
- **Report Channel** — User reports via `-report`
- **DM Error Channel** — Template errors from moderation DMs

### Moderation DM Templates
Context data: `{{.Reason}}`, `{{.Author}}`, `{{.Duration}}`, `{{.HumanDuration}}`, `{{.WarningID}}`

### Permitted Roles
Allow non-admin users with specific roles to run mod commands despite lacking Discord permissions.

## Basic Automoderator

Enabled per-rule. Each rule has: violation threshold, expiry time, ignored role, ignored channels, auto-warning toggle, message deletion toggle.

### Rules
1. **Slowmode** — X messages in Y seconds
2. **Mass Mention** — Exceed mention limit per message
3. **Server Invites** — Invite links to other servers
4. **Links** — Any URL including Discord GIFs
5. **Banned Words** — Case-insensitive word filter
6. **Banned Websites** — Domain blocking (optional Google Safe Browsing)

### Punishments
Configurable per rule: Mute, Kick, or Ban after N violations.

## Advanced Automoderator

Complex rule system with: Triggers (OR logic) → Conditions (AND logic) → Effects (AND logic)

### Structure
- **Lists** — Word/domain lists (5 free, 25 premium, 5000 chars each)
- **Rulesets** — Rule containers (10 free, 25 premium)
- **Rules** — 25 total free, 150 premium, max 25 parts per rule

### Message Triggers
- All caps (min chars, percentage)
- Message mentions (min unique)
- Any link / Server invites
- Word denylist/allowlist (with visual similarity option)
- Website denylist/allowlist
- Google flagged links / Scam links
- Message matches/not-matching regex
- Message length (more/less than X chars)
- Message with/without attachments

### Rate-Based Triggers
- X user/channel messages in Y seconds
- X user/channel mentions in Y seconds
- X user/channel attachments in Y seconds
- X user/channel links in Y seconds
- X consecutive identical messages

### Violation Triggers
- X violations in Y minutes

### User Profile Triggers
- Nickname matches/not-matching regex
- Nickname word allowlist/denylist

### Join Triggers
- Username word allowlist/denylist
- Username matches/not-matching regex
- Username contains invite link
- New member joined

### Conditions
- Ignored/required roles
- Only bots / Ignore bots
- Ignored/active channels
- Ignored/active categories
- Account age above/below
- Server member duration above/below
- New message / Edited message
- Active in threads / Ignore threads
- Forward message conditions

### Effects
- Delete message
- +Violation (named)
- Kick/Ban/Mute/Warn/Timeout user
- Set nickname
- Reset violations
- Delete multiple messages
- Give/Remove role (with optional duration)
- Enable channel slowmode
- Send message (max 280 chars)
- Send alert (to specified channel)

## Logging
- NOT a full logging bot — stores last X messages per channel
- Deleted messages cached: 1hr standard, 12hr premium
- Log retention: 30 days auto-delete
- Log URL: `/public/<server ID>/log/<log count>`
- Created by: `-logs` command or auto on mod actions (100 messages)

## Verification
Google reCAPTCHA v2 verification system.
- Customizable verify page
- Auto-kick after X minutes if unverified
- Verified role assignment
- Alt account detection (self-hosted only)
