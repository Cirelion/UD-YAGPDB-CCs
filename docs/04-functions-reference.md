# YAGPDB Functions Reference

## Channel Functions
| Function | Description |
|----------|-------------|
| `addThreadMember threadID userID` | Add user to thread |
| `closeThread [channel]` | Archive a thread |
| `createForumPost channel title body` | Create forum post, returns channel |
| `createThread channel name [msgID]` | Create thread, returns channel |
| `deleteForumPost channel` | Delete forum post |
| `deleteThread channel` | Delete thread |
| `editChannelName channel name` | Edit channel name (10 calls/CC) |
| `editChannelTopic channel topic` | Edit channel topic |
| `editThread channel (sdict)` | Edit thread properties |
| `getChannel channelID` | Get channel object |
| `getChannelOrThread id` | Get channel or thread |
| `getChannelPins channelID` | Get pinned messages |
| `getPinCount channelID` | Count of pins |
| `getThread threadID` | Get thread object |
| `openThread [channel]` | Unarchive a thread |
| `pinForumPost channel` | Pin forum post |
| `removeThreadMember threadID userID` | Remove user from thread |
| `unpinForumPost channel` | Unpin forum post |

## Database Functions
| Function | Description |
|----------|-------------|
| `dbSet userID key value` | Create/overwrite entry |
| `dbGet userID key` | Get entry (returns DBEntry, use .Value) |
| `dbDel userID key` | Delete entry |
| `dbDelByID entryID` | Delete by internal ID |
| `dbIncr userID key increment` | Increment value (returns float) |
| `dbSetExpire userID key value seconds` | Set with TTL |
| `dbCount query` | Count entries |
| `dbTopEntries pattern amount skip` | Top entries by value |
| `dbBottomEntries pattern amount skip` | Bottom entries by value |
| `dbGetPattern userID pattern amount skip` | Get by pattern |
| `dbGetPatternReverse userID pattern amount skip` | Reverse pattern |
| `dbDelMultiple query amount skip` | Bulk delete |
| `dbRank query userID key` | Get rank position |

## Encoding Functions
| Function | Description |
|----------|-------------|
| `json value` | Marshal to JSON string |
| `jsonToSdict json` | Parse JSON to sdict |
| `encodeBase64 string` | Base64 encode |
| `decodeBase64 string` | Base64 decode |
| `hash algorithm string` | Hash (md5, sha1, sha256, sha512) |
| `urlescape string` | URL encode |
| `urlunescape string` | URL decode |
| `urlquery args...` | Build URL query string |

## Executing CCs
| Function | Description |
|----------|-------------|
| `execCC ccID channel delay data` | Execute another CC. Channel=nil for current. Delay in seconds (0=immediate). Data accessible via `.ExecData` |
| `scheduleUniqueCC ccID channel delay key data` | Schedule with unique key (1 per server per key) |
| `cancelScheduledUniqueCC ccID key` | Cancel scheduled CC |

## Interaction Functions
| Function | Description |
|----------|-------------|
| `sendModal modal` | Send a modal popup |
| `sendResponse nil message` | Send interaction response (first=initial, after=followup) |
| `sendResponseRetID nil message` | Send response, return ID |
| `sendResponseNoEscape nil message` | Response without mention escaping |
| `editResponse nil messageID newContent` | Edit response (needs token) |
| `updateMessage nil newContent` | Update the triggering message |
| `updateMessageNoEscape nil newContent` | Update without escaping |
| `deleteInteractionResponse nil [messageID]` | Delete interaction response |
| `ephemeralResponse` | Mark next response as ephemeral (user-only) |
| `getResponse nil messageID` | Get response message object |

## Component Constructors
| Function | Description |
|----------|-------------|
| `cbutton "label" "text" "custom_id" "id" "style" n` | Create button (styles: 1=primary, 2=secondary, 3=success, 4=danger, 5=link) |
| `cmenu "type" "text/user/role/channel/mentionable" "custom_id" "id" ...` | Create select menu |
| `cmodal "title" "text" "custom_id" "id" "fields" (cslice ...)` | Create modal |

### Components V2
| Function | Description |
|----------|-------------|
| `componentBuilder` | Create new ComponentBuilder |
| `.Add key value` | Add component |
| `.AddSlice key values` | Add multiple components |
| `.Merge other` | Merge builders |
| `.Get key` | Retrieve stored components |

## Math Functions
| Function | Description |
|----------|-------------|
| `add x y` | Addition |
| `sub x y` | Subtraction |
| `mult x y` | Multiplication |
| `div x y` | Integer division |
| `fdiv x y` | Float division |
| `mod x y` | Modulo |
| `pow x y` | Power |
| `sqrt x` | Square root |
| `cbrt x` | Cube root |
| `log x` | Natural log |
| `abs x` | Absolute value |
| `min x y` | Minimum |
| `max x y` | Maximum |
| `round x` | Round |
| `roundCeil x` | Ceiling |
| `roundFloor x` | Floor |
| `roundEven x` | Banker's rounding |
| `randInt [min] max` | Random integer [min, max) |
| `mathConst name` | Math constant (pi, e, etc.) |

## Member Functions
| Function | Description |
|----------|-------------|
| `getMember userID` | Get member object |
| `editNickname newNick` | Edit triggering user's nick |
| `onlineCount` | Online member count |
| `targetHasPermissions userID perms...` | Check target permissions |
| `hasPermissions perms...` | Check triggering user permissions |
| `getTargetPermissionsIn userID channelID` | Get permission int |
| `memberAbove member1 member2` | Role hierarchy check |
| `getMemberVoiceState userID` | Get voice state |

## Message Functions
| Function | Description |
|----------|-------------|
| `sendMessage channelID content` | Send message (nil=current channel) |
| `sendMessageRetID channelID content` | Send and return message ID |
| `sendMessageNoEscape channelID content` | Send without escaping mentions |
| `sendMessageNoEscapeRetID channelID content` | Combined |
| `sendDM content` | DM triggering user (1 call limit) |
| `editMessage channelID messageID content` | Edit message |
| `editMessageNoEscape channelID messageID content` | Edit without escaping |
| `getMessage channelID messageID` | Get message object |
| `deleteMessage channelID messageID [delay]` | Delete message |
| `deleteResponse [delay]` | Delete bot's response |
| `deleteTrigger [delay]` | Delete triggering message |
| `addReactions "emoji1" "emoji2"` | Add to trigger message |
| `addResponseReactions "emoji1" "emoji2"` | Add to response |
| `addMessageReactions channelID messageID "emoji"` | Add to specific message |
| `deleteAllMessageReactions channelID messageID` | Remove all reactions |
| `deleteMessageReaction channelID messageID userID "emoji"` | Remove specific |
| `pinMessage channelID messageID` | Pin message |
| `unpinMessage channelID messageID` | Unpin message |
| `publishMessage channelID messageID` | Crosspost in announcement channel |
| `publishResponse` | Crosspost bot's response |
| `complexMessage key value...` | Build complex message (see below) |
| `complexMessageEdit key value...` | Edit complex message |

### complexMessage Keys
- `"content"` — Text content
- `"embed"` — Single embed (cembed) or multiple embeds (cslice of cembed, up to 10)
- `"file"` — File attachment (sdict "name" "file.txt" "content" "data")
- `"buttons"` — Button(s)
- `"menus"` — Select menu(s)
- `"components"` — Action rows
- `"reply"` — Message ID to reply to
- `"silent"` — Suppress notifications (bool)
- `"allowed_mentions"` — Control mentions (sdict)

## Role Functions
| Function | Description |
|----------|-------------|
| `getRole roleID` | Get role object |
| `getRoleID roleName` | Get role by name |
| `hasRole roleID` | Check if user has role |
| `hasRoleID roleID` | Alias |
| `hasRoleName name` | Check by name |
| `addRole roleID` | Add role to triggering user |
| `addRoleID roleID` | Alias |
| `addRoleName name` | Add by name |
| `removeRole roleID` | Remove role |
| `removeRoleID roleID` | Alias |
| `removeRoleName name` | Remove by name |
| `giveRole userID roleID` | Give role to any user |
| `giveRoleID userID roleID` | Alias |
| `giveRoleName userID name` | Give by name |
| `takeRole userID roleID` | Take role from any user |
| `takeRoleID userID roleID` | Alias |
| `takeRoleName userID name` | Take by name |
| `targetHasRole userID roleID` | Check other user |
| `targetHasRoleID userID roleID` | Alias |
| `targetHasRoleName userID name` | Check by name |
| `setRoles userID roleIDs` | Set exact roles |
| `mentionRole roleID` | Mention role |
| `mentionRoleID roleID` | Alias |
| `mentionRoleName name` | Mention by name |
| `mentionEveryone` | @everyone |
| `mentionHere` | @here |
| `roleAbove role1 role2` | Hierarchy check |

## String Functions
| Function | Description |
|----------|-------------|
| `lower string` | Lowercase |
| `upper string` | Uppercase |
| `title string` | Title case |
| `trimSpace string` | Trim whitespace |
| `split string sep` | Split into slice |
| `joinStr sep parts...` | Join strings |
| `hasPrefix string prefix` | Starts with check |
| `hasSuffix string suffix` | Ends with check |
| `print args...` | Concatenate (like fmt.Sprint) |
| `println args...` | With newline |
| `printf format args...` | Formatted (like fmt.Sprintf) |
| `sanitizeText text` | Remove markdown/mentions |
| `slice string start [end]` | Substring |

## Regex Functions
| Function | Description |
|----------|-------------|
| `reFind pattern string` | First match |
| `reFindAll pattern string` | All matches |
| `reFindAllSubmatches pattern string` | All submatches |
| `reReplace pattern string replacement` | Replace matches |
| `reSplit pattern string` | Split by pattern |
| `reQuoteMeta string` | Escape regex metacharacters |

## Time Functions
| Function | Description |
|----------|-------------|
| `currentTime` | Current time.Time |
| `formatTime time "layout"` | Format time |
| `parseTime time "layout"` | Parse string to time |
| `newDate year month day hour min sec [loc]` | Create time |
| `loadLocation "timezone"` | Load timezone |
| `humanizeDurationHours dur` | Duration → "X hours" |
| `humanizeDurationMinutes dur` | Duration → "X minutes" |
| `humanizeDurationSeconds dur` | Duration → "X seconds" |
| `humanizeTimeSinceDays time` | Days since time |
| `snowflakeToTime id` | Discord snowflake → time |
| `timestampToTime unix` | Unix timestamp → time |
| `weekNumber time` | ISO week number |

## Type Conversion
| Function | Description |
|----------|-------------|
| `toString x` / `str x` | Convert to string |
| `toInt x` | Convert to int |
| `toInt64 x` | Convert to int64 |
| `toFloat x` | Convert to float64 |
| `toDuration x` | Convert to duration |
| `toRune string` | String → rune slice |
| `toByte string` | String → byte slice |
| `structToSdict struct` | Struct → sdict (shallow) |

## Miscellaneous
| Function | Description |
|----------|-------------|
| `cembed key val...` | Create embed object |
| `cslice items...` | Create slice |
| `sdict key val...` | Create string-key map |
| `dict key val...` | Create any-key map |
| `seq start end` | Integer sequence [start, end) |
| `shuffle slice` | Randomize order |
| `sort slice` | Sort slice |
| `len x` | Length of slice/map/string |
| `index collection idx` | Get element at index |
| `in collection value` | Check membership |
| `inFold collection value` | Case-insensitive membership |
| `slice collection start [end]` | Sub-slice |
| `kindOf value` | Type name string |
| `sleep seconds` | Pause (max 60s total/CC) |
| `exec command args...` | Run YAGPDB command |
| `execAdmin command args...` | Run as admin |
| `parseArgs n msg cargs...` | Parse command arguments |
| `carg "type" "description"` | Define argument for parseArgs |
| `adjective` / `noun` / `verb` | Random word |
| `humanizeThousands n` | Format number with commas |
| `createTicket author topic` | Create support ticket |
| `getWarnings userID [page]` | Get user warnings |
| `sendTemplate channelID templateName data` | Send defined template |
| `sendTemplateDM templateName data` | DM a template |

### parseArgs Types
`int`, `float`, `string`, `user`, `userid`, `member`, `channel`, `role`, `duration`

### carg with range validation
```
{{ carg "int" "description" min max }}
{{ carg "float" "description" min max }}
{{ carg "duration" "description" min max }}
```

### Accessing parsed args
```
{{ $args := parseArgs 2 "Error msg" (carg "int" "num") (carg "string" "text") }}
{{ $num := $args.Get 0 }}
{{ $text := $args.Get 1 }}
{{ if $args.IsSet 2 }}...{{ end }}  // Optional arg check
```
