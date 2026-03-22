# Discord Fundamentals for YAGPDB Development

## Channel Types

| Type | ID | Description |
|------|-----|-------------|
| GUILD_TEXT | 0 | Standard text channel |
| DM | 1 | Direct message |
| GUILD_VOICE | 2 | Voice channel |
| GUILD_CATEGORY | 4 | Category (holds up to 50 channels) |
| GUILD_ANNOUNCEMENT | 5 | Announcement/news channel |
| ANNOUNCEMENT_THREAD | 10 | Thread in announcement channel |
| PUBLIC_THREAD | 11 | Thread in text or forum channel |
| PRIVATE_THREAD | 12 | Thread visible only to invitees + MANAGE_THREADS |
| GUILD_STAGE_VOICE | 13 | Stage channel |
| GUILD_FORUM | 15 | Forum channel (can only contain threads) |
| GUILD_MEDIA | 16 | Media channel (like forum but media-focused) |

Threads do NOT count against the per-guild channel limit.

## Thread Behavior

### Archived vs Locked
- **Archived**: Thread is immutable. Users CANNOT send messages OR add reactions.
  The API auto-unarchives when a message is sent (if user has permission).
- **Locked**: Only MANAGE_THREADS holders can unarchive. Stays immutable regardless.
- **Key rule**: "To send a message or add a reaction, a thread must first be unarchived."
  Neither reactions nor messages work in archived threads.

### Auto-archive Durations (minutes)
`60` (1hr), `1440` (1 day), `4320` (3 days), `10080` (7 days)

### Thread Permissions
- Threads inherit permissions from the parent channel
- You CANNOT set permission overwrites directly on threads
- SEND_MESSAGES does NOT carry to threads -- SEND_MESSAGES_IN_THREADS (bit 38) is required instead
- Threads in private channels are fully private to members of that channel

## Forum Channels
- Forum posts are PUBLIC_THREAD channels created within the forum
- Creating a forum post auto-creates a thread with an initial message
- Up to 20 tags per forum channel, up to 5 tags per thread
- Topic length: 0-4096 chars (vs 0-1024 for regular text channels)
- Forum tags can be moderated (only MANAGE_THREADS holders can add/remove)

## Permission Bit Flags (Key Ones)

| Permission | Bit | Value | Notes |
|------------|-----|-------|-------|
| CREATE_INSTANT_INVITE | 0 | 0x1 | |
| KICK_MEMBERS | 1 | 0x2 | |
| BAN_MEMBERS | 2 | 0x4 | |
| ADMINISTRATOR | 3 | 0x8 | Bypasses ALL checks |
| MANAGE_CHANNELS | 4 | 0x10 | |
| MANAGE_GUILD | 5 | 0x20 | |
| ADD_REACTIONS | 6 | 0x40 | |
| VIEW_CHANNEL | 10 | 0x400 | |
| SEND_MESSAGES | 11 | 0x800 | Does NOT apply to threads |
| MANAGE_MESSAGES | 13 | 0x2000 | Delete others' messages |
| READ_MESSAGE_HISTORY | 16 | 0x10000 | |
| MENTION_EVERYONE | 17 | 0x20000 | |
| USE_EXTERNAL_EMOJIS | 18 | 0x40000 | |
| MANAGE_ROLES | 28 | 0x10000000 | |
| MANAGE_THREADS | 34 | 0x400000000 | Delete/archive threads |
| CREATE_PUBLIC_THREADS | 35 | 0x800000000 | |
| CREATE_PRIVATE_THREADS | 36 | 0x1000000000 | |
| SEND_MESSAGES_IN_THREADS | 38 | 0x4000000000 | Required for thread posting |
| MODERATE_MEMBERS | 40 | 0x10000000000 | Timeout users |

### Permission Cascade
1. Start with @everyone role perms, OR all member's role perms
2. ADMINISTRATOR = all permissions
3. Apply channel overwrites: @everyone deny -> @everyone allow -> role denies -> role allows -> member deny -> member allow
4. Threads inherit from parent (no overwrites on threads)

### Implicit Denials
- Denying VIEW_CHANNEL denies all other perms
- Denying SEND_MESSAGES denies MENTION_EVERYONE, SEND_TTS, ATTACH_FILES, EMBED_LINKS
- Timed-out members keep only VIEW_CHANNEL and READ_MESSAGE_HISTORY

## Message Limits

| Constraint | Limit |
|------------|-------|
| Content length | 2,000 chars |
| Embeds per message | 10 |
| Combined embed text | 6,000 chars total |
| Embed title | 256 chars |
| Embed description | 4,096 chars |
| Embed fields | 25 per embed |
| Field name | 256 chars |
| Field value | 1,024 chars |
| Footer text | 2,048 chars |
| Author name | 256 chars |
| Stickers per message | 3 |
| Bulk delete | 2-100 messages, max 2 weeks old |

## Component Limits

| Constraint | Limit |
|------------|-------|
| Total components per message | 40 |
| Buttons per action row | 5 |
| Select menus per action row | 1 |
| Select menu options | 25 |
| Button label | 80 chars |
| Button custom_id | 1-100 chars |
| Select menu placeholder | 150 chars |
| Option label/value/description | 100 chars each |
| Modal text input min_length | 0-4000 |
| Modal text input max_length | 1-4000 |
| Modal text input placeholder | 100 chars |

## Emoji Formats

| Context | Unicode | Custom |
|---------|---------|--------|
| In message content | The character itself | `<:name:id>` or `<a:name:id>` (animated) |
| In API calls (reactions) | URL-encoded character | `name:id` |
| In YAGPDB addReactions | The character (e.g. "❤️") | `name:id` (e.g. "custom:123456") |
| In component emoji field | `(sdict "name" "❤️")` | `(sdict "id" "123456")` |

## Slowmode
Range: 0-21600 seconds (0 to 6 hours)
