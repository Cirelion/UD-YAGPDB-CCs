# Advanced YAGPDB Reference

## Context Fields Often Missed

### Message
| Field | Type | Notes |
|-------|------|-------|
| `.Message.ReferencedMessage` | *Message | Full message object of the replied-to message |
| `.Message.Type` | int | Discord message type (0=default, 19=reply, etc.) |
| `.Message.Components` | []ActionsRow | Buttons/menus on the message |
| `.Message.Link` | string | Direct Discord link to the message |
| `.Message.ContentWithMentionsReplaced` | string | Content with `<@ID>` replaced by usernames |
| `.Message.Flags` | int | Message flags bitfield |
| `.Message.StickerItems` | []StickerItem | Stickers on the message |

### Channel (Thread-specific)
| Field | Type | Notes |
|-------|------|-------|
| `.Channel.ThreadMetadata.Archived` | bool | Whether archived |
| `.Channel.ThreadMetadata.Locked` | bool | Whether locked |
| `.Channel.ThreadMetadata.AutoArchiveDuration` | int | Minutes until auto-archive |
| `.Channel.ThreadMetadata.Invitable` | bool | Non-mods can add members |
| `.Channel.OwnerID` | int64 | Thread creator (0 for non-threads) |
| `.Channel.AvailableTags` | []ForumTag | Tags available in forum |
| `.Channel.AppliedTags` | []int64 | Tags applied to this thread |

### Member
| Field | Type | Notes |
|-------|------|-------|
| `.Member.CommunicationDisabledUntil` | *time.Time | Timeout expiry (nil if not timed out) |
| `.Member.Pending` | bool | Behind membership screening |
| `.Member.Flags` | int | Guild member flags |

### User
| Field | Type | Notes |
|-------|------|-------|
| `.User.Globalname` | string | New display name from Discord's naming system |

### Guild
| Field | Type | Notes |
|-------|------|-------|
| `.Guild.Threads` | []ChannelState | All active threads |
| `.Guild.Features` | []string | Enabled guild features |
| `.Guild.GetMemberPermissions(chID, memberID, roles)` | int64 | Calculate permissions |

### Interaction
| Field | Type | Notes |
|-------|------|-------|
| `.Interaction.RespondedTo` | bool | Whether already responded |
| `.InteractionData.IsButton` | bool | |
| `.InteractionData.IsMenu` | bool | |
| `.InteractionData.MenuType` | string | "string", "user", "role", "mentionable", "channel" |

### Reaction Trigger
| Field | Type | Notes |
|-------|------|-------|
| `.Reaction.Emoji.APIName` | string | Correctly formatted for API |
| `.Reaction.Emoji.MessageFormat` | string | Correctly formatted for message content |
| `.ReactionMessage` | Message | Alias for `.Message` |

## Exact Function Limits

| Function | Free | Premium | Notes |
|----------|------|---------|-------|
| `execCC` | 1/CC | 10/CC | |
| `scheduleUniqueCC` | 1/CC | 10/CC | |
| execCC per channel/min | 10 | 10 | Excess silently dropped |
| `exec` / `execAdmin` | 5/CC | 5/CC | |
| `sendDM` | 1/CC | 1/CC | |
| `sendTemplate` | 3/CC | 3/CC | |
| `addReactions` | 20/CC | 20/CC | Per emoji, not per call |
| `addMessageReactions` | 20/CC | 20/CC | Per emoji, not per call |
| `deleteMessageReaction` | 10/CC | 10/CC | |
| `editChannelName/Topic` | 10/CC | 10/CC | Discord: 2 per 10 min |
| `editNickname` | 2/CC | 2/CC | |
| `getChannelPins` | 2/CC | 4/CC | |
| `sort` | 1/CC | 3/CC | |
| `sleep` | 60s total | 60s total | Combined across templates |
| Regex patterns | 10/CC | 10/CC | Unique patterns cached |
| `deleteResponse` max delay | 86400s | 86400s | |
| `Append/AppendSlice` max | 10000 | 10000 | |
| `seq` max | 10000 | 10000 | |
| `joinStr` max | 1000kB | 1000kB | |
| complexMessage file | 100kB | 100kB | |
| Generic API calls | 100/CC | 100/CC | |
| State lock actions | 500/CC | 500/CC | |

### What Counts as "Generic API Action Calls"
Not explicitly enumerated in docs. getMessage, sendMessage, editMessage, deleteMessage,
addMessageReactions, giveRoleID, createForumPost, editThread, closeThread, etc. all
likely count. The exact list is undocumented.

## Database Edge Cases

### Type Behavior
- ALL numerical values returned as float64 regardless of stored type
- Large numbers lose precision -- always convert to string for Discord IDs
- `dbIncr` always returns float
- Numerical dict keys retrieved as int64
- `sdict`, `dict`, and `cslice` retain type through serialization

### Pattern Matching
Uses PostgreSQL LIKE syntax (NOT regex):
- `%` matches any sequence of characters
- `_` matches any single character
- Escape with `\%` and `\_`

### Ordering
- `dbTopEntries`: descending by value, then by ID
- `dbBottomEntries`: ascending by value, then by ID
- `dbGetPattern`: ascending
- `dbGetPatternReverse`: descending
- All capped at 100 entries per call

## complexMessage Full Key List

### For sending (complexMessage)
`content`, `embed` (single or slice of up to 10), `file`, `filename`, `reply` (message ID),
`silent` (bool), `menus`, `buttons` (slice), `sticker` (ID or cslice), `forward` (sdict
with `channel` and `message`), `suppress_embeds`, `is_components_v2`, `allowed_mentions`

### For editing (complexMessageEdit)
`content`, `embed`, `silent`, `suppress_embeds`, `is_components_v2`
-- NO `file`, `filename`, `reply`, `buttons`, `menus`, `sticker`, `forward`

### allowed_mentions sub-keys
`parse` (slice: "users", "roles", "everyone"), `users` (slice of IDs),
`roles` (slice of IDs), `replied_user` (bool)

## sort Function
```
{{ sort $list }}                           {{/* ascending */}}
{{ sort $list (sdict "reverse" true) }}    {{/* descending */}}
{{ sort $list (sdict "key" "name") }}      {{/* by map key */}}
```
Limit: 1 call free, 3 premium.

## Miscellaneous
- `toInt` supports base 2-36: `{{ toInt "ff" 16 }}` = 255
- `json` supports pretty-print: `{{ json $data true }}`
- `kindOf` has indirect mode: `{{ kindOf $val true }}` reads through interfaces
- `inFold` = case-insensitive `in`
- `getChannelOrThread` works for both (unlike `getChannel` which fails on threads)
