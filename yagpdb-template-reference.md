# YAGPDB Template Reference - Complete Documentation

## Part 1: Syntax and Data

### Preface

The documentation covers YAGPDB's templating system, a modified version of Go's `text/template` package. All template content uses standard ASCII quotation marks: `"` for double quotes, `'` for single quotes, and `` ` `` for backticks.

### Core Concepts

#### The Dot and Variables

The dot (`.`) represents the current context encompassing all active data. For example, `.User` refers to the triggering user object. Variables are initialized with `$variable := value` syntax, with scope extending to the enclosing control structure's `end` action or command conclusion.

The special variable `$` maintains access to global context from anywhere in the template, unless explicitly redefined.

#### Pipes

Pipes (`|`) chain actions sequentially, passing one function's output as the next function's input. Example: `{{randInt 41| add 2}}` pipelines a random integer through addition.

### Context Data Fields

#### Global Fields

| Field | Purpose |
|-------|---------|
| `.BotUser` | Bot's user object |
| `.CCID` | Current custom command ID (int64) |
| `.CCRunCount` | Cached run count (+-30 min accuracy) |
| `.CCTrigger` | Printable trigger name |
| `.CustomID` | Full custom ID for components/modals |
| `.DomainRegex` | Domain-matching regex string |
| `.IsMessageEdit` | Boolean for edited messages |
| `.IsPremium` | Guild premium status |
| `.LinkRegex` | Link-matching regex string |
| `.Permissions` | Permission bits (e.g., `.Permissions.AddReactions` = 64) |
| `.ServerPrefix` | Command prefix |

#### Channel

| Field | Description |
|-------|-------------|
| `.Channel.AppliedTags` | Forum post tag IDs |
| `.Channel.AvailableTags` | Available forum tags slice |
| `.Channel.Bitrate` | Voice channel bitrate |
| `.Channel.GuildID` | Parent guild ID |
| `.Channel.ID` | Channel ID |
| `.Channel.IsForum` | Forum channel boolean |
| `.Channel.IsPrivate` | DM channel boolean |
| `.Channel.IsThread` | Thread boolean |
| `.Channel.Mention` | Channel mention |
| `.Channel.Name` | Channel name |
| `.Channel.NSFW` | NSFW status |
| `.Channel.OwnerID` | Thread creator ID |
| `.Channel.ParentID` | Parent category ID |
| `.Channel.PermissionOverwrites` | Permission overwrites slice |
| `.Channel.Position` | Top-down position |
| `.Channel.ThreadMetadata` | Thread-specific metadata |
| `.Channel.Topic` | Channel topic |
| `.Channel.Type` | Channel type |

**Thread Metadata Fields:**
- `.Archived` - Archived status
- `.AutoArchiveDuration` - Auto-archive minutes
- `.ArchiveTimestamp` - Archive time
- `.Locked` - Lock status
- `.Invitable` - Non-moderator add permission

#### Guild/Server

| Field | Description |
|-------|-------------|
| `.Guild.AfkChannelID` | AFK channel ID |
| `.Guild.AfkTimeout` | AFK timeout duration |
| `.Guild.Banner` | Banner hash |
| `.Guild.Channels` | Channels slice |
| `.Guild.DefaultMessageNotifications` | Notification level |
| `.Guild.Emojis` | Guild emojis list |
| `.Guild.ExplicitContentFilter` | Content filter level |
| `.Guild.Features` | Enabled features |
| `.Guild.Icon` | Icon hash |
| `.Guild.ID` | Guild ID |
| `.Guild.MemberCount` | Member count |
| `.Guild.MfaLevel` | MFA requirement level |
| `.Guild.Name` | Guild name |
| `.Guild.OwnerID` | Owner ID |
| `.Guild.PreferredLocale` | Preferred language |
| `.Guild.Roles` | All roles slice |
| `.Guild.Stickers` | Stickers slice |
| `.Guild.Splash` | Splash hash |
| `.Guild.SystemChannelID` | System notices channel |
| `.Guild.Threads` | Active threads slice |
| `.Guild.VerificationLevel` | Verification requirement |
| `.Guild.VoiceStates` | Connected users slice |
| `.Guild.WidgetChannelID` | Widget channel ID |
| `.Guild.WidgetEnabled` | Widget enabled boolean |

**Guild Methods:**
- `.Guild.BannerURL "256"` - Banner URL with size
- `.Guild.GetChannel id` - Get channel by ID
- `.Guild.GetEmoji id` - Get emoji by ID
- `.Guild.GetMemberPermissions channelID memberID memberRoles` - Calculate permissions
- `.Guild.GetRole id` - Get role by ID
- `.Guild.GetVoiceState userID` - Get user's voice state
- `.Guild.IconURL "size"` - Guild icon URL

#### Interaction

Accessible when triggered by components or modals.

| Field | Description |
|-------|-------------|
| `.Interaction.ChannelID` | Interaction channel ID |
| `.InteractionData` | Component or modal data |
| `.Interaction.ID` | Unique interaction ID |
| `.Interaction.Locale` | User's locale |
| `.Interaction.Member` | Interacting member |
| `.Interaction.Message` | Associated message |
| `.Interaction.RespondedTo` | Response boolean |
| `.Interaction.Token` | Unique interaction token |

**InteractionData Fields:**
- `.Args` - All custom ID arguments
- `.Cmd` - Trigger command
- `.CmdArgs` - Arguments after trigger
- `.CustomID` - Triggering ID
- `.IsButton` - Button trigger boolean
- `.IsMenu` - Menu trigger boolean
- `.MenuType` - Menu type (string/user/role/mentionable/channel)
- `.StrippedID` - Custom ID remainder
- `.Values` - Selected menu options or modal inputs

#### Member

| Field | Description |
|-------|-------------|
| `.Member.Avatar` | Member avatar hash |
| `.Member.CommunicationDisabledUntil` | Timeout expiration |
| `.Member.Flags` | Guild member flags |
| `.Member.GuildID` | Guild ID |
| `.Member.JoinedAt` | Join timestamp |
| `.Member.Nick` | Member nickname |
| `.Member.Pending` | Screening pending |
| `.Member.PremiumSince` | Boost start time |
| `.Member.Roles` | Role IDs slice |
| `.Member.User` | Underlying user object |

**Member Methods:**
- `.Member.AvatarURL "256"` - Member avatar URL

#### Message

| Field | Description |
|-------|-------------|
| `.Message.Activity` | Rich presence activity |
| `.Message.ApplicationID` | Application ID |
| `.Message.Attachments` | Attachments slice |
| `.Message.Author` | Message author (User) |
| `.Message.ChannelID` | Channel ID |
| `.Message.Components` | Action rows slice |
| `.Message.Content` | Message text |
| `.Message.ContentWithMentionsReplaced` | Mentions replaced with usernames |
| `.Message.EditedTimestamp` | Last edit time |
| `.Message.Embeds` | Embeds slice |
| `.Message.Flags` | Message flags |
| `.Message.GuildID` | Guild ID |
| `.Message.ID` | Message ID |
| `.Message.Link` | Discord message link |
| `.Message.Member` | Message author member object |
| `.Message.MentionEveryone` | Everyone mention boolean |
| `.Message.MentionRoles` | Mentioned roles slice |
| `.Message.Mentions` | Mentioned users slice |
| `.Message.MessageReference` | Reply/crosspost reference |
| `.Message.Pinned` | Pinned boolean |
| `.Message.Reactions` | Reactions slice |
| `.Message.Reference` | Message reference |
| `.Message.ReferencedMessage` | Replied-to message |
| `.Message.MessageSnapshots` | Message snapshots |
| `.Message.StickerItems` | Sticker items slice |
| `.Message.Timestamp` | Message time |
| `.Message.TTS` | Text-to-speech boolean |
| `.Message.Type` | Message type |
| `.Message.WebhookID` | Webhook ID |
| `.Message.RoleSubscriptionData` | Role subscription info |

**Message Data Fields:**
- `.Args` - Message content split
- `.Cmd` - Trigger command
- `.CmdArgs` - Arguments after trigger
- `.StrippedMsg` - Message remainder after trigger

**RoleSubscriptionData Fields:**
- `.RoleSubscriptionListingID` - SKU and listing ID
- `.TierName` - Subscription tier
- `.TotalMonthsSubscribed` - Cumulative months
- `.IsRenewal` - Renewal vs new purchase

#### Reaction

Accessible when reaction trigger is used.

| Field | Description |
|-------|-------------|
| `.Reaction` | Reaction object (UserID, MessageID, Emoji, ChannelID, GuildID) |
| `.Reaction.Emoji.APIName` | API-formatted emoji name |
| `.Reaction.Emoji.MessageFormat` | Message-ready emoji format |
| `.ReactionAdded` | Added vs removed boolean |
| `.ReactionMessage` / `.Message` | Message object (partial) |

#### Role Change

Accessible when Role Changes trigger is used.

| Field | Description |
|-------|-------------|
| `.TargetMember` | Member object of changed member |
| `.TargetUser` | User object of changed user |
| `.Author` | User who performed change |
| `.Role` | Modified role object |
| `.RoleAdded` | Added vs removed boolean |

#### User

| Field | Description |
|-------|-------------|
| `.User` | Username with discriminator |
| `.User.Avatar` | Avatar hash |
| `.User.Bot` | Bot boolean |
| `.User.Discriminator` | Four-digit tag |
| `.User.Globalname` | New username system |
| `.User.ID` | User ID |
| `.User.Mention` | User mention |
| `.User.PrimaryGuild` | Primary guild info |
| `.User.String` | Username with legacy discriminator |
| `.User.Username` | Username |
| `.UsernameHasInvite` | Invite in username (join/leave only) |
| `.RealUsername` | Real username when invite censored |

**User Methods:**
- `.User.AvatarURL "256"` - Avatar URL with size

**Primary Guild Fields:**
- `.IdentityGuildID` - Primary guild ID
- `.IdentityEnabled` - Tag display boolean
- `.Tag` - Server tag text
- `.Badge` - Badge hash
- `.BadgeURL` - Badge URL

### Control Actions

#### If (Conditional Branching)

```
{{if (condition)}} output {{end}}
{{if (condition)}} output1 {{else if (condition)}} output2 {{end}}
{{if (condition)}} output1 {{else}} output2 {{end}}
```

#### Boolean Logic

| Operator | Syntax |
|----------|--------|
| AND | `{{if and (cond1) (cond2) (cond3)}} output {{end}}` |
| NOT | `{{if not (condition)}} output {{end}}` |
| OR | `{{if or (cond1) (cond2) (cond3)}} output {{end}}` |

#### Comparison Operators

| Operator | Example |
|----------|---------|
| Equal: `eq` | `{{if eq .Channel.ID ########}} output {{end}}` |
| Not equal: `ne` | `{{$x := 7}} {{$y := 8}} {{ne $x $y}}` returns `true` |
| Less than: `lt` | `{{if lt (len .Args) 5}} output {{end}}` |
| Less or equal: `le` | `{{le $x $y}}` returns `true` |
| Greater than: `gt` | `{{if gt (len .Args) 1}} output {{end}}` |
| Greater or equal: `ge` | `{{ge $x $y}}` returns `false` |

The `eq` function accepts multiple arguments and checks if first equals any following argument.

#### Range

Iterates over integers, slices, arrays, maps, or channels. The dot (`.`) becomes successive elements.

```
{{range 2}}{{.}}{{end}}
{{range $k, $v := toInt64 2}}{{$k}}{{$v}}{{end}}
{{range $index, $element := cslice "YAGPDB" "IS COOL!" }}
  {{$index}} : {{$element}}
{{end}}
{{range $key, $value := dict "SO" "SAY" "WE" "ALL!" }}
  {{$key}} : {{$value}}
{{end}}
{{range seq 1 1}} no output {{else}} output here {{end}}
```

Use `{{continue}}` to skip to next iteration or `{{break}}` to exit loop.

To access outer context inside `range`, use `$` prefix (e.g., `$.User.ID`).

**Whitespace stripping:** Use `{{- expr -}}` to remove whitespace and prevent response size errors.

#### Try-Catch

Handles errors gracefully:

```
{{try}}
  {{addReactions "wave"}}
  added reactions successfully
{{catch}}
  {{if in .Error "Reaction blocked"}}
    user blocked YAG :(
  {{else}}
    different issue occurred: {{.Error}}
  {{end}}
{{end}}
```

#### While

Iterates while condition is true:

```
{{while (condition)}}
  {{/* loop body */}}
{{end}}
```

Supports `{{break}}` and `{{continue}}`.

#### With

Assigns pipeline value as dot (`.`) inside control structure:

```
{{with (pipeline)}}
  {{/* dot is pipeline value */}}
{{end}}

{{with (pipeline)}} output {{else}} alternative {{end}}

{{with false}} not executed {{else if eq $x "42"}} executed {{end}}
```

To access outer context, use `$` prefix (e.g., `$.User.ID`).

### Associated Templates

#### Definition

```
{{define "template_name"}}
  {{/* template body */}}
{{end}}
```

Definitions must be at top level (not nested). Template names are string constants, not variables. Use `{{return}}` to exit:

```
{{define "getOne"}} {{return 1}} {{end}}
```

#### Execution

**Template action** (ignores return value):
```
{{template "sayHi"}}
{{template "sayHi" "YAG"}}
```

**Block action** (combines define and template):
```
{{block "sayHi" $name}}
  hi there, {{.}}
{{end}}
```

**execTemplate function** (returns value):
```
{{define "factorial"}}
  {{- $n := 1 }}
  {{- range seq 2 (add . 1) }}
    {{- $n = mult $n . }}
  {{- end }}
  {{- return $n -}}
{{end}}

{{$fac := execTemplate "factorial" 5}}
2 * 5! = {{mult $fac 2}}
```

### Custom Types

#### templates.Slice (cslice)

Custom slice type using `interface{}` elements.

**Functions:**
- `cslice value1 value2 ...` - Creates slice

**Methods:**
- `.Append arg` - Appends value, returns new cslice (max 10,000 length)
- `.AppendSlice arg` - Appends slice (max 10,000 length)
- `.Set int value` - Sets index to value
- `.StringSlice strict-flag` - Returns `[]string` of string/time elements

**Examples:**
```
{{$x := (cslice "red" "red")}} {{$x}}
{{$x = $x.Append "green"}} {{$x}}
{{$x.Set 1 "blue"}} {{$x}}
{{index $x 1}}
{{$x.AppendSlice (cslice "yellow" "magenta")}}
{{printf "%T" $x}}
{{(cslice currentTime.Month 42 "YAGPDB").StringSlice}}
```

#### templates.SDict (sdict)

Custom map type with string keys and `interface{}` values.

**Functions:**
- `sdict "key1" value1 "key2" value2 ...` - Creates ordered map

**Methods:**
- `.Del "key"` - Deletes key
- `.Get "key"` - Retrieves value
- `.HasKey "key"` - Returns boolean
- `.Set "key" value` - Sets key or creates new

**Examples:**
```
{{$x := sdict "color1" "green" "color2" "red"}} {{$x}}
{{$x.Get "color2"}}
{{$x.Set "color2" "yellow"}} {{$x}}
{{$x.Set "color3" "blue"}} {{$x}}
{{$x.Del "color1"}} {{$x}}
```

**Note:** Slices and dictionaries exhibit reference-type behavior -- modifying a copy affects the original. Use range + Set for copying.

### Database

Key-value store with max 50 x user_count (500 x for premium) entries. Keys are strings/numbers (max 256 bytes). Values serialized to max 100kB.

**Interaction limits:** 10 per CC (50 for premium). `dbTop/BottomEntries`, `dbCount`, `dbGetPattern`, `dbDelMultiple` limited to 2 calls each.

Patterns use PostgreSQL syntax: `_` matches single char, `%` matches any sequence.

#### DBEntry

| Field | Description |
|-------|-------------|
| `.ID` | Entry ID (int64) |
| `.GuildID` | Server ID (int64) |
| `.UserID` | User ID (int64) or 0 for global |
| `.User` | User object (ID field only) |
| `.CreatedAt` | Creation time |
| `.UpdatedAt` | Last update time |
| `.ExpiresAt` | Expiration time |
| `.Key` | Key (string) |
| `.Value` | Value (returns floats as float64) |
| `.ValueSize` | Value size in bytes |

### Tickets

#### Template Ticket

| Field | Description |
|-------|-------------|
| `.AuthorID` | Ticket author ID |
| `.AuthorUsernameDiscrim` | Discord tag format |
| `.ChannelID` | Ticket channel ID |
| `.ClosedAt` | Closure time |
| `.CreatedAt` | Creation time |
| `.GuildID` | Guild ID |
| `.LocalID` | Ticket ID |
| `.LogsID` | Log ID |
| `.Title` | Ticket title |

### Time

Go's time package fields and methods:

| Field | Description |
|-------|-------------|
| `.DiscordEpoch` | Discord epoch as time.Time |
| `.UnixEpoch` | Unix epoch as time.Time |
| `.TimeHour` | 1 hour duration |
| `.TimeMinute` | 1 minute duration |
| `.TimeSecond` | 1 second duration |

`discordgo.Timestamp` types convert to `time.Time` with `.Parse` method.

---

## Part 2: Functions

### Channel Functions

**addThreadMember**
```
{{ addThreadMember <thread> <member> }}
```
Adds a member to an existing thread. Does nothing if either argument is invalid.

**closeThread**
```
{{ closeThread <thread> <lock> }}
```
Closes or locks the given thread. You cannot lock a closed thread, but you can close a locked thread.
- `lock`: whether to lock the thread instead. Default `false`.

**createForumPost**
```
{{ $post := createForumPost <channel> <name> <content> [values] }}
```
Creates a new forum post. Returns a channel object on success.
- `channel`: the forum channel to post to.
- `name`: The post title. May not be empty. Must be a string.
- `content`: the initial message's content; may be a string, an embed, or a complex message. May not be empty.
- `values` (optional): Additional options for the post. May include:
  - `"slowmode"`: The thread's slowmode in seconds.
  - `"tags"`: One or more forum tag name or ID. Duplicate and invalid tags are ignored.

**createThread**
```
{{ $thread := createThread <channel> <messageID> <name> [private] [auto_archive_duration] [invitable] }}
```
Creates a new thread in the specified channel. Returns a channel object on success.
- `channel`: the parent channel to create the thread in.
- `message`: either `nil` to create a channel thread, or a message ID to create a message thread.
- `private`: whether the thread is private. Default `false`.
- `auto_archive_duration`: how long the thread will show in the channel list after inactivity. Valid values are 60, 1440, 4320, and 10080 minutes. Defaults to 10080 (7 days).
- `invitable`: whether non-moderators can add other members to the thread. (true/false)

Note: There is no functional difference between a channel thread and a message thread.

Example to create a public thread in the current channel with no message reference that is archived after an hour and allows non-moderators to add others:
```
{{ createThread nil nil "new thread" false 60 true }}
```

**deleteForumPost**
```
{{ deleteForumPost <post> }}
```
Deletes the given forum post. Functionally the same as `deleteThread`.

**deleteThread**
```
{{ deleteThread <thread> }}
```
Deletes the given thread. Functionally the same as `deleteForumPost`.

**editChannelName**
```
{{ editChannelName <channel> <newName> }}
```
Edits the name of the given channel.
- `newName`: the new name for the channel. Must be a string.

This function is, together with `editChannelTopic`, limited to 10 calls per custom command execution. In addition to this, Discord limits the number of channel modifications to 2 per 10 minutes.

**editChannelTopic**
```
{{ editChannelTopic <channel> <newTopic> }}
```
Edits the topic of the given channel.
- `newTopic`: the channel's new topic. Must be a string. Discord markdown is supported.

This function is, together with `editChannelName`, limited to 10 calls per custom command execution. In addition to this, Discord limits the number of channel modifications to 2 per 10 minutes.

**editThread**
```
{{ editThread <thread> <opts> }}
```
Edits the specified thread.
- `opts`: a sdict containing the thread parameters to edit, supporting the following keys (all optional):
  - `slowmode`: the thread's slowmode in seconds.
  - `tags`: one or more forum tag name or ID. Duplicate and invalid tags are ignored.
  - `auto_archive_duration`: how long the thread will show in the channel list after inactivity.
  - `invitable`: whether non-moderators can add other members to the thread. Defaults to false.

**getChannelOrThread**
```
{{ $channel := getChannelOrThread <channel> }}
```
Returns the full channel or thread object for the given channel.

**getChannelPins**
```
{{ $pins := getChannelPins <channel> }}
```
Returns a slice of message objects pinned to the given channel or thread.

Rate-limited to 2 (premium: 4) calls per custom command execution.

**getChannel**
```
{{ $channel := getChannel <channel> }}
```
Returns the full channel object for the given channel. Will not work for threads.

**getPinCount**
```
{{ $numPins := getPinCount <channel> }}
```
Returns the number of pinned messages in given channel.

**getThread**
```
{{ $thread := getThread <thread> }}
```
Returns the full thread object for the given thread. Will not work for channels.

**openThread**
```
{{ openThread <thread> }}
```
Reopens the given thread.

**pinForumPost**
```
{{ pinForumPost <post> }}
```
Pins the given forum post, which may be specified by its ID or name.

**removeThreadMember**
```
{{ removeThreadMember <thread> <member> }}
```
Removes the given member from the given thread.

**unpinForumPost**
```
{{ unpinForumPost <post> }}
```
Unpins the given forum post, which may be specified by its ID or name.

### Database Functions

**dbBottomEntries**
```
{{ $entries := dbBottomEntries <pattern> <amount> <nSkip> }}
```
Returns up to `amount` entries from the database, sorted in ascending order by the numeric value, then by entry ID.
- `amount`: the maximum number of entries to return, capped at 100.
- `pattern`: the PostgreSQL pattern to match entries against.
- `nSkip`: the number of entries to skip before returning results.

**dbCount**
```
{{ $count := dbCount <userID|pattern|query> }}
```
Returns the count of all matching database entries that are not expired.

The argument must be one of the following:
- `userID`: count entries for the given user ID.
- `pattern`: count only entries with keys matching the given pattern.
- `query`: an sdict with the following (all optional) keys:
  - `userID`: only count entries with a matching UserID field. Defaults to all UserIDs.
  - `pattern`: only counts entries with keys matching the given pattern. Defaults to all keys.

**dbDelByID**
```
{{ dbDelByID <userID> <ID> }}
```
Deletes a database entry under the given `userID` by its `ID`.

**dbDelMultiple**
```
{{ $numDeleted := dbDelMultiple <query> <amount> <nSkip> }}
```
Deletes up to `amount` entries from the database matching the given criteria. Returns the number of deleted entries.
- `query`: an sdict with the following (all optional) keys:
  - `userID`: only delete entries with a matching UserID field. Defaults to all UserIDs.
  - `pattern`: only delete entries with keys matching the given pattern. Defaults to all keys.
  - `reverse`: whether to delete entries with the lowest value first. Default is `false` (highest value first).
- `amount`: the maximum number of entries to delete, capped at 100.
- `nSkip`: the number of entries to skip before deleting.

**dbDel**
```
{{ dbDel <userID> <key> }}
```
Deletes the specified entry from the database.

**dbGetPatternReverse**
```
{{ $entries := dbGetPatternReverse <userID> <pattern> <amount> <nSkip> }}
```
Retrieves up to `amount` entries from the database in descending order as a slice.
- `userID`: the user ID to retrieve entries for.
- `pattern`: the PostgreSQL pattern to match entries against.
- `amount`: the maximum number of entries to return, capped at 100.
- `nSkip`: the number of entries to skip before returning results.

**dbGetPattern**
```
{{ $entries := dbGetPattern <userID> <pattern> <amount> <nSkip> }}
```
Returns up to `amount` entries from the database in ascending order as a slice.
- `userID`: the user ID to retrieve entries for.
- `pattern`: the PostgreSQL pattern to match entries against.
- `amount`: the maximum number of entries to return, capped at 100.
- `nSkip`: the number of entries to skip before returning results.

**dbGet**
```
{{ $entry := dbGet <userID> <key> }}
```
Returns the specified database entry.

**dbIncr**
```
{{ $newValue := dbIncr <userID> <key> <incrBy> }}
```
Increments the value of the specified database entry by `incrBy`. Returns the new value as a floating-point number.
- `incrBy`: the amount to increment the value by. Must be a valid number.

**dbRank**
```
{{ $rank := dbRank <query> <userID> <key> }}
```
Returns the rank of the specified entry in the set of entries as defined by `query`.
- `query`: an sdict with the following (all optional) keys:
  - `userID`: only include entries with the given user ID.
  - `pattern`: only include entries with keys matching the given pattern.
  - `reverse`: if `true`, entries with lower values have higher ranks. Default is `false`.

**dbSetExpire**
```
{{ dbSetExpire <userID> <key> <value> <ttl> }}
```
Same as `dbSet` but with an additional expiration `ttl` in seconds.

**dbSet**
```
{{ dbSet <userID> <key> <value> }}
```
Sets the value for the specified `key` and `userID` to `value`.
- `value`: an arbitrary value to set.

**dbTopEntries**
```
{{ $entries := dbTopEntries <pattern> <amount> <nSkip> }}
```
Returns up to `amount` entries from the database, sorted in descending order by the numeric value, then by entry ID.
- `pattern`: the PostgreSQL pattern to match entries against.
- `amount`: the maximum number of entries to return, capped at 100.
- `nSkip`: the number of entries to skip before returning results.

Caution: Numerical values are stored as floating-point numbers in the database; large numbers such as user IDs will lose precision. To avoid this, convert them to a string before writing to the database.

Because numerical `dict` keys are retrieved as an `int64`, you have to first convert to `int64` when retrieving such values. So, to retrieve the value associated with the numerical key `N`: `{{ $dict.Get (toInt64 N)}}`.

### Encoding and Decoding Functions

**decodeBase64**
```
{{ $decoded := decodeBase64 <string> }}
```
Undoes the transformation performed by `encodeBase64`, converting the base64-encoded `string` back to its original form.

**encodeBase64**
```
{{ $encoded := encodeBase64 <string> }}
```
Encodes the input `string` to base64.

**hash**
```
{{ $hash := hash <string> }}
```
Generates the SHA256 hash of the input string.

**json**
```
{{ $json := json <value> [indent] }}
```
Encodes `value` as JSON. If the `indent` flag is `true`, the output is pretty-printed with appropriate indentation.

**jsonToSdict**
```
{{ $sdict := jsonToSdict <json> }}
```
Parses the JSON-encoded data into a string-dictionary, returning an error if the input was invalid JSON.

**urlescape**
```
{{ $result := urlescape <string> }}
```
Escapes the input `string` such that it can be safely placed inside a URL path segment, replacing special characters (including `/`) with `%XX` sequences as needed.

**urlunescape**
```
{{ $result := urlunescape <string> }}
```
Undos the transformation performed by `urlescape`, converting encoded substrings of the form `%AB` to the byte `0xAB`.

**urlquery**
```
{{ $result := urlquery <string> }}
```
Returns the escaped value of the textual representation of the arguments in a form suitable for embedding in a URL query.

### Executing Custom Commands Functions

**cancelScheduledUniqueCC**
```
{{ cancelScheduledUniqueCC <ccID> <key> }}
```
Cancels a previously scheduled custom command execution using `scheduleUniqueCC`.

**execCC**
```
{{ execCC <ccID> <channel> <delay> <data> }}
```
Executes another custom command specified by `ccID`.
- `ccID`: the ID of the custom command to execute.
- `channel`: the channel to execute the custom command in. May be `nil`, a channel ID, or a channel name.
- `delay`: the delay in seconds before executing the custom command.
- `data`: some arbitrary data to pass to the executed custom command.

Calling `execCC` with 0 delay sets `.StackDepth` to the current recursion depth and limits it to 2. `execCC` is rate-limited strictly to a maximum of 10 delayed custom commands executed per channel per minute. Executions beyond this number will be dropped.

Example:
```
{{ if .ExecData }}
  {{ sendMessage nil (print "Executing custom command... Got data: " .ExecData) }}
  {{ return }}
{{ end }}

{{ sendMessage nil "Starting up..." }}
{{ execCC .CCID nil 5 "Hello, world!" }}
```

**scheduleUniqueCC**
```
{{ scheduleUniqueCC <ccID> <channel> <delay> <key> <data> }}
```
Schedules a custom command execution to occur in the future, identified by `key`.
- `ccID`: the ID of the custom command to execute.
- `channel`: the channel to execute the custom command in. May be `nil`, a channel ID, or a channel name.
- `delay`: the delay in seconds before executing the custom command.
- `key`: a unique key to identify the scheduled custom command.
- `data`: some arbitrary data to pass to the executed custom command.

To cancel such a scheduled custom command before it runs, use `cancelScheduledUniqueCC`.

### Interactions Functions

#### Interaction Responses

Only one interaction response may be sent to each interaction. If you do not send an interaction response, members will see "This application did not respond" on Discord. You may only send an interaction response to the interaction which triggered the command. Text output directly to the response is automatically sent as an interaction response if the interaction hasn't already been responded to. A CC executed with `execCC` by the triggered CC will be able to send initial responses to the triggering interaction. A response is not the same thing as a followup.

**sendModal**
```
{{ sendModal <modal> }}
```
Sends a modal to the member who triggered the interaction.
- `modal`: an sdict with the following keys:
  - `title`: the title of the modal.
  - `custom_id`: a unique identifier for the modal.
  - `fields`: a slice of sdicts with the following keys:
    - `label`: the label for the field.
    - `placeholder`: the placeholder text for the field.
    - `value`: the default value for the field.
    - `required`: whether the field is required.
    - `style`: the style of the field (1 for short, 2 for long).
    - `min_length`: the minimum length of the field.
    - `max_length`: the maximum length of the field.

Alternatively, you can create a modal object using the `cmodal` function.

Example:
```
{{ $modal := sdict
  "title" "My Custom Modal"
  "custom_id" "modals-my_first_modal"
  "fields" (cslice
    (sdict "label" "Name" "placeholder" "Duck" "required" true)
    (sdict "label" "Do you like ducks?" "value" "Heck no")
    (sdict "label" "Duck hate essay" "min_length" 100 "style")) }}
{{ sendModal $modal }}
```

**updateMessage**
```
{{ updateMessage <newMessage> }}
```
Edits the message on which the button, select menu, or modal was triggered on.
- `newMessage`: the new message content. May be a string, an embed, or a complex message.

Example:
```
{{ $button := cbutton "label" "I won!" "custom_id" "i_won" }}
{{ $content := printf "Press this button when you win! The last person who won was %s! They wanted to say they are a %s %s." .User.Mention adjective noun }}

{{ $message := complexMessageEdit "content" $content "buttons" $button }}
{{ updateMessage $message }}
```

**updateMessageNoEscape**
```
{{ updateMessageNoEscape <newMessage> }}
```
Same as `updateMessage`, plus it does not escape mentions.

#### Interaction Followups

Interaction followups may be sent up to 15 minutes after an interaction. To send a followup, you must have the interaction token of the interaction you are following up. You can send as many followups as you'd like. Text output directly to the response is automatically sent as an interaction followup if the interaction has already been responded to. A followup is not the same thing as a response.

**editResponse**
```
{{ editResponse <interactionToken> <messageID> <newContent> }}
```
Edits a response to an interaction.
- `interactionToken`: the token of the interaction to edit. `nil` for the triggering interaction.
- `messageID`: the ID of a follow-up message. `nil` for the original interaction response.
- `newContent`: the new content for the message.

Example:
```
{{ $token := .Interaction.Token }}

{{ sendResponse nil "Here's the first message!" }}
{{ $id := sendResponseRetID $token (complexMessage "content" "Here's a sneaky one!" "ephemeral" true) }}

{{ sleep 2 }}

{{ editResponse $token $id (print "I've edited this message to say " noun) }}
{{ $editedResponse := getResponse $token $id }}
{{ editResponse $token nil $editedResponse.Content }}
```

**editResponseNoEscape**
```
{{ editResponseNoEscape <interactionToken> <messageID> <newContent> }}
```
Same as `editResponse`, plus it does not escape mentions.

**sendResponse**
```
{{ sendResponse <interactionToken> <message> }}
```
Sends a message in response to an interaction. Supports the `ephemeral` flag in `complexMessage`.

**sendResponseNoEscape**
```
{{ sendResponseNoEscape <interactionToken> <message> }}
```
Same as `sendResponse`, plus it does not escape mentions.

**sendResponseNoEscapeRetID**
```
{{ sendResponseNoEscapeRetID <interactionToken> <message> }}
```
Same as `sendResponseNoEscape`, but also returns the message ID.

**sendResponseRetID**
```
{{ sendResponseRetID <interactionToken> <message> }}
```
Same as `sendResponse`, but also returns the message ID.

#### Interaction Miscellaneous

**cbutton**
```
{{ $button := cbutton "list of button values" }}
```
Creates a button object for use in interactions.

A link style button _must_ have a URL and may not have a Custom ID. All other styles _must_ have a Custom ID and cannot have a URL. All buttons must have either a label or an emoji.

Example:
```
{{ $button := cbutton "label" "Button" "custom_id" "buttons-duck" }}
{{ $message := complexMessage "buttons" $button }}
{{ sendMessage nil $message }}
```

**cmenu**
```
{{ $menu := cmenu "list of select menu values" }}
```
Creates a select menu object for use in interactions.

The type should be provided as a string: `"text"`, `"user"`, `"role"`, `"mentionable"`, or `"channel"`. Text type menus _must_ have `options`, while all other types cannot.

Example:
```
{{ $menu := cmenu
  "type" "text"
  "placeholder" "Choose a terrible thing"
  "custom_id" "menus-duck"
  "options" (cslice
    (sdict "label" "Two Ducks" "value" "opt-1" "default" true)
    (sdict "label" "A Duck" "value" "duck-option" "emoji" (sdict "name" "duck_emoji"))
    (sdict "label" "Half a Duck" "value" "third-option" "description" "Don't let the smaller amount fool you."))
  "max_values" 3 }}

{{ sendMessage nil (complexMessage "menus" $menu) }}
```

**cmodal**
```
{{ $modal := cmodal "list of modal values" }}
```
Creates a modal object for use in interactions.

**deleteInteractionResponse**
```
{{ deleteInteractionResponse <interactionToken> <messageID> [delay] }}
```
Deletes the specified response or follow-up message.
- `interactionToken`: a valid interaction token or `nil` for the triggering interaction.
- `messageID`: valid message ID of a follow-up, or `nil` for the original interaction response.
- `delay`: an optional delay in seconds, max 10 seconds. Default: 10 seconds.

If you require a delay of more than 10 seconds, consider using `execCC` for deletion of an ephemeral response, or `deleteMessage` to delete a regular interaction response.

**ephemeralResponse**
```
{{ ephemeralResponse }}
```
Tells the bot to send the response text as an ephemeral message. Only works when triggered by an interaction. Works on responses and follow-ups.

Example:
```
{{ ephemeralResponse }}

This text is invisible to others!
```

**getResponse**
```
{{ $response := getResponse <interactionToken> <messageID> }}
```
Returns the response or follow-up with the specified message ID belonging to the given interaction as a message object. Is also valid for ephemeral messages.

### Components V2 Functions

**componentBuilder**
```
{{ $component := componentBuilder (sdict [text] [section] [gallery] [file] [separator] [container] [buttons] [menus] [interactive_components] [allowed_mentions] [reply] [silent] [ephemeral]) }}
```
Returns a complex message object with the given components.

All keys are optional, but the Discord API will reject completely empty messages, so some content is required.

- `text`: A string or a slice of strings.
- `section`: A layout block that shows text with one mandatory accessory: either a button or a thumbnail with the following keys:
  - `text`: A string or a slice of strings.
  - `button`: A button object.
  - `thumbnail`: An sdict with the following keys:
    - `media`: A string.
    - `description`: A string.
    - `spoiler`: A bool.
- `gallery`: Displays one or more media items with optional descriptions and spoiler flags with the following keys:
  - `media`: A string.
  - `description`: A string.
  - `spoiler`: A bool.
- `file`: Attaches text files to the message and optionally displays them with the following keys:
  - `content`: A string. (max 100 000 chars)
  - `name`: A string. (.txt appended automatically)
- `separator`: Adds spacing between components with the following keys:
  - `true`: large separator
  - `false` or `nil`: small separator
- `container`: Top-level layout. Containers offer the ability to visually encapsulate a collection of components, and have an optional customizable accent color bar.
  - `components`: A componentBuilder or a slice thereof.
  - `color`: hex accent color (optional).
  - `spoiler`: hides content until revealed (optional).
- `buttons`: Interactive buttons users can click. Can be single or multiple.
- `menus`: Interactive menus users can select from. Can be single or multiple.
- `interactive_components`: Mix of buttons and menus, auto-distributed.
- `allowed_mentions`: A sdict with the following keys:
  - `users`: A slice of user IDs.
  - `roles`: A slice of role IDs.
  - `everyone`: A bool.
  - `replied_user`: A bool.
- `reply`: A sdict with the following keys:
  - `message_id`: A string.
  - `thread_id`: A string.
- `silent`: A bool.
- `ephemeral`: A bool.

#### Component Builder Functions

**ComponentBuilder.Add**
```
{{ $builder.Add <key> <value> }}
```
Adds a single component entry to the builder under the given key.
- `key` - The top-level key for the component (e.g., "text", "section", "buttons").
- `value` - The component data (string, sdict, Button, SelectMenu, etc.).

**ComponentBuilder.AddSlice**
```
{{ $builder.AddSlice <key> <values...> }}
```
Adds multiple components under one key.
- `key` - The top-level key for the component (e.g., "text", "section", "buttons").
- `values` - The component data (string, sdict, Button, SelectMenu, etc.).

**ComponentBuilder.Merge**
```
{{ $builder.Merge <other> }}
```
Combine another builder into the current one.
- `other` - The other component builder to merge.

**ComponentBuilder.Get**
```
{{ $value := <builder>.Get <key> }}
```
Returns the component data for the given key.
- `key` - The top-level key for the component (e.g., "text", "section", "buttons").

#### Modal Components

**modalBuilder**
```
{{ $builder := modalBuilder }}
```
Returns a `modalBuilder`, which simplifies the construction of modal components.

You can optionally pass the following positional arguments to initialize the modal with the given values:
```
{{ $builder := modalBuilder title custom_id components... }}
```

**modalBuilder.Set**
```
{{ $builder.Set <key> <value> }}
```
Sets the value of a modal component field. `key` must be one of the following:
- `title`: the modal's title. Max 45 characters.
- `custom_id`: a unique identifier for the modal.
- `components`: a slice of modal components, created with below functions.

**modalBuilder.AddComponents**
```
{{ $builder.AddComponents (values...) }}
```
Adds one or more modal components to the builder. `values` may be a single component or a slice of components.

All following components must be wrapped in a label to be used in a modal.

**ccheckbox**
```
{{ $checkbox := ccheckbox (values...) }}
```
Returns a checkbox component for use in modals.

`values` may be an sdict or a list of key-value pairs with the following keys:
- `custom_id`: a unique identifier for the checkbox.
- `default`: optional bool for whether the checkbox is checked by default.

**ccheckboxGroup**
```
{{ $checkboxGroup := ccheckboxGroup (values...) }}
```
Returns a checkbox group component for use in modals.

`values` may be an sdict or a list of key-value pairs with the following keys:
- `custom_id`: a unique identifier for the checkbox group.
- `min_values`: optionally the minimum number of checkboxes that must be checked. Min 0, max 10, defaults to 1. If 0, `required` must be false.
- `max_values`: optionally the maximum number of checkboxes that can be checked. Min 1, max the number of options, defaults to number of options.
- `required`: optional bool for whether selecting within the group is required.
- `options`: a slice of sdicts with the following keys:
  - `label`: User-facing label for the checkbox.
  - `value`: the value that will be sent when the checkbox is checked.
  - `description`: the optional description for the checkbox. Max 100 characters.
  - `default`: optional bool for whether the checkbox is checked by default.

**cradioGroup**
```
{{ $radioGroup := cradioGroup (values...) }}
```
Returns a radio group component for use in modals for selecting exactly one option from a defined list.

`values` may be an sdict or a list of key-value pairs with the following keys:
- `custom_id`: a unique identifier for the radio group.
- `required`: optional bool for whether selecting an option is required. Defaults to true.
- `options`: a slice of sdicts with the following keys:
  - `label`: User-facing label for the radio option.
  - `value`: the value that will be sent when the option is selected.
  - `description`: the optional description for the option. Max 100 characters.
  - `default`: optional bool for whether the option is selected by default. Only one option can be selected by default.

**ctextInput**
```
{{ $textInput := ctextInput (values...) }}
```
Returns a text input component for use in modals.

`values` may be an sdict or a list of key-value pairs with the following keys:
- `custom_id`: a unique identifier for the text input.
- `style`: the style of the text input, either 1 for short, 2 for long.
- `min_length`: the optional minimum length of the input. Min 0, max 4000.
- `max_length`: the optional maximum length of the input. Min 1, max 4000.
- `required`: optional bool for whether the text input is required. Defaults to true.
- `value`: the optional default value for the text input. Max 4000 characters.
- `placeholder`: the optional placeholder text for the text input. Max 100 characters.

**clabel**
```
{{ $label := clabel (values...) }}
```
Returns a label component for use in modals.

Labels are top-level components that wrap modal components.

`values` may be an sdict or a list of key-value pairs with the following keys:
- `label`: the label text. Max 45 characters.
- `component`: The component within.
- `description`: the optional label description. Max 100 characters.

**ctextDisplay**
```
{{ $textDisplay := ctextDisplay (values...) }}
```
Creates a text display component.

`values` may be an sdict or a list of key-value pairs with the following keys:
- `content`: the text to display. Markdown supported.

### Math Functions

**abs**
```
{{ $result := abs x }}
```
Returns the absolute value of the provided number.

**add**
```
{{ $sum := add x y [...] }}
```
Returns the sum of the provided numbers. Detects the first number's type and performs the operation accordingly.

**bitwiseAnd**
```
{{ $result := bitwiseAnd x y [...] }}
```
Performs a bitwise AND operation on the provided numbers and returns the result.

**bitwiseAndNot**
```
{{ $result := bitwiseAndNot x y }}
```
Performs a bitwise AND NOT operation on the two provided numbers and returns the result.

**bitwiseNot**
```
{{ $result := bitwiseNot x }}
```
Performs a bitwise NOT operation on the provided number and returns the result.

**bitwiseOr**
```
{{ $result := bitwiseOr x y [...] }}
```
Performs a bitwise OR operation on the provided numbers and returns the result.

**bitwiseXor**
```
{{ $result := bitwiseXor x y }}
```
Performs a bitwise XOR operation on the two provided numbers and returns the result.

**bitwiseLeftShift**
```
{{ $result := bitwiseLeftShift x y }}
```
Shifts X left by Y bits and returns the result.

**bitwiseRightShift**
```
{{ $result := bitwiseRightShift x y }}
```
Shifts X right by Y bits and returns the result.

**cbrt**
```
{{ $result := cbrt x }}
```
Returns the cube root of the provided number.

**div**
```
{{ $result := div x y [...] }}
```
Performs division on the provided numbers. Detects the first number's type and performs the operation accordingly. If you need a floating-point number as a result of integer division, use `fdiv`.

**fdiv**
```
{{ $result := fdiv x y [...] }}
```
Special case of `div`; always returns a floating-point number as result.

**log**
```
{{ $result := log x [base] }}
```
Returns the logarithm of X with the given base. If no base is provided, the natural logarithm is used.

**mathConst**
```
{{ $result := mathConst "constant" }}
```
Returns the value of the specified math constant.

**max**
```
{{ $result := max x y }}
```
Returns the larger of the two provided numbers.

**min**
```
{{ $result := min x y }}
```
Returns the smaller of the two provided numbers.

**mod**
```
{{ $result := mod x y }}
```
Returns the floating-point remainder of the division of X by Y.

Takes the sign of X, so `mod -5 3` results in `-2`, not `1`. To ensure a non-negative result, use `mod` twice: `{{ mod (add (mod x y) y) y }}`.

**mult**
```
{{ $result := mult x y [...] }}
```
Performs multiplication on the provided numbers. Detects the first number's type and returns the result accordingly.

**pow**
```
{{ $result := pow x y }}
```
Returns X raised to the power of Y as a floating-point number.

**randInt**
```
{{ $result := randInt [start] stop }}
```
Returns a random integer in the right-closed interval of `[0, stop)` or `[start, stop)` if two arguments are provided. That is, the result is always greater than or equal to `start` and strictly less than `stop`.

**round**
```
{{ $result := round x }}
```
Returns the nearest integer to X as float. Normal rounding rules apply.

**roundCeil**
```
{{ $result := roundCeil x }}
```
Returns the smallest integer greater than or equal to X. Put simply, always round up.

**roundEven**
```
{{ $result := roundEven x }}
```
Returns the nearest integer to X, rounding ties (x.5) to the nearest even integer.

**roundFloor**
```
{{ $result := roundFloor x }}
```
Returns the largest integer less than or equal to X. Put simply, always round down.

**sqrt**
```
{{ $result := sqrt x }}
```
Returns the square root of X as a floating-point number.

**sub**
```
{{ $result := sub x y [...] }}
```
Subtracts the provided numbers from each other. Detects the first number's type and returns the result accordingly.

### Member Functions

**editNickname**
```
{{ editNickname "newNick" }}
```
Edits the nickname of the member who triggered the command. The bot must have the `MANAGE_NICKNAMES` permission and be higher in the role hierarchy than the member. The bot cannot change the nickname of the server owner.

**getMember**
```
{{ $member := getMember <mention|userID> }}
```
Returns the member object for the given mention or user ID.

**getMemberVoiceState**
```
{{$state := getMemberVoiceState <member> }}
```
Returns the voice state for the member specified by ID or mention, or `nil` if the member is not in a voice channel.

**getTargetPermissionsIn**
```
{{ $perms := getTargetPermissionsIn <memberID> <channelID> }}
```
Returns the permissions of the specified member in the given channel as a permissions bitfield.

Example:
```
{{ $perms := getTargetPermissionsIn .User.ID $someChannel }}
{{ $mask := bitwiseAnd .Permissions.ManageRoles $perms }}
{{ if eq $mask .Permissions.ManageRoles }}
  You have the permissions to manage roles!
{{ else }}
  You do not have the permissions to manage roles!
{{ end }}
```

**hasPermissions**
```
{{ $hasPerms := hasPermissions <permission> }}
```
Returns whether the member who triggered the command has the specified permission bit.

Example:
```
{{ if hasPermissions .Permissions.Administrator }}
  You have the Administrator permission!
{{ else }}
  You do not have the Administrator permission.
{{ end }}
```

**memberAbove**
```
{{ $isAbove := memberAbove <a> <b> }}
```
Returns whether member `a` is higher than member `b` in the role hierarchy. Both `a` and `b` must be a member object.

**memberAboveRole**
```
{{ $isAbove := memberAboveRole <member> <role> }}
```
Returns whether the specified member is higher than the specified role in the role hierarchy. `member` must be a member object, `role` must be a role object.

**onlineCount**
```
{{ $count := onlineCount }}
```
Returns the count of online members on the current server, including bots.

**targetHasPermissions**
```
{{ $hasPerms := targetHasPermissions <memberID> <permission> }}
```
Returns whether the specified member has the specified permission bit.

### Mentions Functions

**mentionEveryone**
```
{{ mentionEveryone }}
```
Mentions `@everyone` without escaping it.

**mentionHere**
```
{{ mentionHere }}
```
Mentions `@here` without escaping it.

**mentionRole**
```
{{ mentionRole <role> }}
```
Mentions `role`, which may be specified by ID, mention, name, or role object, without escaping it.

**mentionRoleID**
```
{{ mentionRoleID <roleID> }}
```
Mentions the given role without escaping it.

**mentionRoleName**
```
{{ mentionRoleName <roleName> }}
```
Mentions the role with the given name without escaping it. Searches for first case-insensitive match.

Prefer `mentionRoleID`, as IDs are guaranteed to be unique and do not change with role edits.

### Message Functions

**addMessageReactions**
```
{{ addMessageReactions <channel> <messageID> <emojis...> }}
```
Adds reactions to a message with the given ID.
- `emojis...`: a list of emojis to add as reactions. May also be a slice of emojis.

Default emojis are best used in the Unicode format for these purposes. Custom emojis follow a specific format in these functions.

Example:
```
{{ addMessageReactions nil .Message.ID "thumbsup" "thumbsdown" "yagpdb:505114640032858114" }}
```

**addReactions**
```
{{ addReactions <emojis...> }}
```
Adds reactions to the message that triggered the command.
- `emojis...`: a list of emojis to add as reactions. May also be a slice of emojis.

**addResponseReactions**
```
{{ addResponseReactions <emojis...> }}
```
Adds reactions to the response message.
- `emojis...`: a list of emojis to add as reactions. May also be a slice of emojis.

Note that a message sent via `sendMessage` is not the response -- use `addMessageReactions` for that.

**complexMessageEdit**
```
{{ $message := complexMessageEdit [allowed_mentions] [content] [embed] [silent] [suppress_embeds] [is_components_v2] }}
```
Creates a complex message object for use in `editMessage` or `editMessageNoEscape`.
- `allowed_mentions`: an sdict with the following keys:
  - `parse`: a slice of accepted values for mentions. May include `users`, `roles`, and `everyone`.
  - `users`: a slice of user IDs to mention.
  - `roles`: a slice of role IDs to mention.
  - `replied_user`: whether to mention the replied user.
- `content`: the new content for the message.
- `embed`: an embed object or a slice of up to 10 embed objects.
- `silent`: whether to suppress push and desktop notifications.
- `suppress_embeds`: whether to suppress embeds in the message.
- `is_components_v2`: whether the message uses components v2. Irreversibly switches the message to use components v2.

All of these keys are optional, but providing none of them will have no effect.

Setting `suppress_embeds` to `false` on a message with already suppressed embeds will not re-enable them.

**complexMessage**
```
{{ $message := complexMessage [allowed_mentions] [content] [embed] [file] [filename] [reply] [silent] [menus] [buttons] [sticker] [forward] [suppress_embeds] [is_components_v2] }}
```
Creates a complex message object for use in `sendMessage` functions.
- `allowed_mentions`: an sdict with the following keys:
  - `parse`: a slice of accepted values for mentions. May include `users`, `roles`, and `everyone`.
  - `users`: a slice of user IDs to mention.
  - `roles`: a slice of role IDs to mention.
  - `replied_user`: whether to mention the replied user.
- `content`: the message content.
- `embed`: an embed object or a slice of up to 10 embed objects.
- `file`: the content to print as a file.
- `filename`: the name of the file.
- `reply`: the ID of the message to reply to.
- `silent`: whether to suppress push and desktop notifications.
- `menus`: a single select menu object.
- `buttons`: a slice of button objects.
- `sticker`: single sticker ID or a slice of sticker IDs
- `forward`: an sdict containing `channel` and `message` keys specifying the message to forward
- `suppress_embeds`: whether to suppress embeds in the message.
- `is_components_v2`: whether the message uses components v2. Irreversibly switches the message to use components v2.

All of these keys are optional, but providing an empty content, file, or no embeds will result in no message being sent.

Example:
```
{{ $message := complexMessage
  "allowed_mentions" (sdict
    "replied_user" true
  )
  "content" "Hello, world!"
  "embed" (cembed
    "title" "Embed Title"
    "description" "Embed Description"
    "color" 0xff0000
  )
  "file" "This is a file."
  "filename" "example.txt"
  "reply" .Message.ID
  "silent" true
}}
{{ sendMessage nil $message }}
```

Example for sending a sticker message:
```
{{ sendMessage nil ( complexMessage
  "sticker" 749054660769218631
) }}
```

Example for sending multiple stickers in a message:
```
{{ sendMessage nil ( complexMessage
  "sticker" ( cslice 749054660769218631 819128604311027752 1308344035563274291 )
) }}
```

Example of a message forward:
```
{{ sendMessage nil (complexMessage
    "forward" (sdict
        "channel" .Channel.ID
        "message" .Message.ID
    )
) }}
```

**deleteAllMessageReactions**
```
{{ deleteAllMessageReactions <channel> <messageID> [emojis...] }}
```
Deletes all reactions from a message, optionally constrained to specific emojis.
- `emojis`: the emojis to delete. May also be a slice. Not providing this argument will delete any and all reactions.

**deleteMessageReaction**
```
{{ deleteMessageReaction <channel> <messageID> <userID> <emojis...> }}
```
Deletes a specific user's reaction from a message.
- `userID`: the ID of the user whose reaction to delete.
- `emojis...`: the emojis to delete. May also be a slice.

**deleteMessage**
```
{{ deleteMessage <channel> <messageID> [delay] }}
```
Deletes the specified message.
- `delay`: an optional delay in seconds to delete the message after. Defaults to 10 seconds. Max 86400 seconds (1 day).

**deleteResponse**
```
{{ deleteResponse [delay] }}
```
Deletes the response message.
- `delay`: an optional delay in seconds to delete after. Defaults to 10 seconds. Max 86400 seconds (1 day).

**deleteTrigger**
```
{{ deleteTrigger [delay] }}
```
Deletes the triggering message.
- `delay`: an optional delay in seconds to delete after. Defaults to 10 seconds. Max 86400 seconds (1 day).

**editMessageNoEscape**
```
{{ editMessageNoEscape <channel> <messageID> <newMessageContent> }}
```
Edits the given message without escaping mentions.
- `newMessageContent`: the new content for the message. May also be the result from `complexMessageEdit`.

**editMessage**
```
{{ editMessage <channel> <messageID> <newMessageContent> }}
```
Edits the given message with escaping mentions.
- `newMessageContent`: the new content for the message. May also be the result from `complexMessageEdit`.

**getMessage**
```
{{ $message := getMessage <channel> <messageID> }}
```
Returns the message object for the given message ID in the specified channel.

**pinMessage**
```
{{ pinMessage <channel> <messageID> }}
```
Pins the specified message.

**publishMessage**
```
{{ publishMessage <channel> <messageID> }}
```
Publishes the specified message.

**publishResponse**
```
{{ publishResponse }}
```
Publishes the response message. For this to work, the custom command must be running in such an announcement channel.

**sendDM**
```
{{ sendDM <message> }}
```
Sends a direct message to the triggering user.
- `message`: the message to send. May also be the result from a `complexMessage` call.

**sendMessageNoEscapeRetID**
```
{{ $messageID := sendMessageNoEscapeRetID <channel> <message> }}
```
Same as `sendMessageNoEscape`, but also returns the message ID.

**sendMessageNoEscape**
```
{{ sendMessageNoEscape <channel> <message> }}
```
Same as `sendMessage`, but does not escape mentions.

**sendMessageRetID**
```
{{ $messageID := sendMessageRetID <channel> <message> }}
```
Same as `sendMessage`, but also returns the message ID.

**sendMessage**
```
{{ sendMessage <channel> <message> }}
```
Sends a message in the specified channel.
- `message`: the message to send. May also be the result from a `complexMessage` call.

**unpinMessage**
```
{{ unpinMessage <channel> <messageID> }}
```
Unpins the specified message.

### Regex Functions

When supplying regular expressions to the following functions, it is convenient to use backticks (`) instead of double quotation marks. The backticks ensure that the content is interpreted verbatim as a _raw string_, which avoids needing to double-escape backslashes.

For example, to search for the regular expression `\w+` in the string `$s`:
```
{{ reFind `\w+` $s }} {{/* ok */}}
{{ reFind "\w+" $s }} {{/* not ok; \w is interpreted as string escape sequence */}}
{{ reFind "\\w+" $s }} {{/* also ok; same result as first line */}}
```

**reFind**
```
{{ $result := reFind <regex> <text> }}
```
Returns the first match of the regular expression `regex` in `text`, or the empty string if the pattern did not match anywhere.

**reFindAll**
```
{{ $result := reFindAll <regex> <text> [count] }}
```
Returns a slice of successive matches of `regex` in `text`. If `count` is provided, the number of matches is limited to that amount; otherwise, all matches are returned.

**reFindAllSubmatches**
```
{{ $result := reFindAllSubmatches <regex> <text> [count] }}
```
Returns a slice of successive submatches of `regex` in `text`. Each submatch is itself a slice containing the match of the entire expression, followed by any matches of capturing groups. If `count` is provided, the number of submatches is limited to that amount; otherwise, all submatches are returned.

**reQuoteMeta**
```
{{ $result := reQuoteMeta <string> }}
```
Escapes all regular expression metacharacters in the input `string`; the result is a regular expression matching the literal input string.

**reReplace**
```
{{ $result := reReplace <regex> <text> <replacement> }}
```
Replaces all matches of `regex` in `text` with `replacement`.

**reSplit**
```
{{ $result := reSplit <regex> <text> [count] }}
```
Splits the `text` around each match of `regex`, returning a slice of delimited substrings.

if the `count` parameter is specified, it limits the number of substrings to return:
- `count > 0`: at most `count` substrings; the last substring will be the unsplit remainder;
- `count == 0`: the result is `nil` (zero substrings);
- `count < 0`: all substrings.

### Role Functions

**addRole**
```
{{ $role := addRole <role> [delay] }}
```
Adds the specified role to the triggering member.
- `role`: a role ID, mention, name, or role object.
- `delay`: an optional delay in seconds.

**addRoleID**
```
{{ addRoleID <roleID> [delay] }}
```
Adds the specified role ID to the triggering member.
- `delay`: an optional delay in seconds.

**addRoleName**
```
{{ addRoleName <roleName> [delay] }}
```
Adds the first case-insensitive matching role name to the triggering member.
- `delay`: an optional delay in seconds.

**getRole**
```
{{ $role := getRole <role> }}
```
Returns a role object. `role` may either be an ID or a name to match against (ignoring case).

**getRoleID**
```
{{ $role := getRoleID <roleID> }}
```
Returns a role object by its ID.

**getRoleName**
```
{{ $role := getRoleName <roleName> }}
```
Returns a role object by its name (case-insensitive).

**giveRole**
```
{{ giveRole <member> <role> [delay] }}
```
Gives the specified role to the target member.
- `role`: a role ID, mention, name, or role object.
- `delay`: an optional delay in seconds.

**giveRoleID**
```
{{ giveRoleID <member> <roleID> [delay] }}
```
Gives the role specified by its ID to the target member.
- `member`: the member to target. Either an ID, mention, or user object, but must be part of the server.
- `delay`: an optional delay in seconds.

**giveRoleName**
```
{{ giveRoleName <member> <roleName> [delay] }}
```
Gives the first case-insensitive matching role name to the target member.
- `delay`: an optional delay in seconds.

**hasRole**
```
{{ $result := hasRole <role> }}
```
Reports whether the triggering member has the specified role.
- `role`: a role ID, mention, name, or role object.

**hasRoleID**
```
{{ $result := hasRoleID <roleID> }}
```
Reports whether the triggering member has the specified role ID.

**hasRoleName**
```
{{ $result := hasRoleName <roleName> }}
```
Reports whether the triggering member has the specified role name (case-insensitive).

**removeRole**
```
{{ removeRole <role> [delay] }}
```
Removes the specified role from the triggering member.
- `role`: a role ID, mention, name, or role object.
- `delay`: an optional delay in seconds.

**removeRoleID**
```
{{ removeRoleID <roleID> [delay] }}
```
Removes the role with the specified ID from the triggering member with an optional delay in seconds.

**removeRoleName**
```
{{ removeRoleName <roleName> [delay] }}
```
Removes the first case-insensitive matching role name from the triggering member with an optional delay in seconds.

**roleAbove**
```
{{ $result := roleAbove <role1> <role2> }}
```
Reports whether `role1` is above `role2` in the role hierarchy. Both arguments must be a role object.

**setRoles**
```
{{ setRoles <target> <newRoles> }}
```
Sets the roles of the specified user to the provided list of role IDs. The roles are overwritten, so any existing roles are removed if not included in the new list. `newRoles` must be a slice. `target` may be a user ID, mention, or user object, but must be a member of the server.

**takeRole**
```
{{ takeRole <target> <role> [delay] }}
```
Removes the specified role from the target member.
- `target`: a user ID, mention, or user object. The target must be part of the server.
- `role`: a role ID, mention, name or role object.
- `delay`: an optional delay in seconds.

**takeRoleID**
```
{{ takeRoleID <target> <roleID> [delay] }}
```
Removes the specified role from `target`. `target` may be a user ID, mention, or user object, but must be a member of the server.
- `delay`: an optional delay in seconds.

**takeRoleName**
```
{{ takeRoleName <target> <roleName> [delay] }}
```
Removes the first case-insensitive matching role name from `target` with an optional delay in seconds. `target` may be a user ID, mention, or user object, but must be a member of the server.

**targetHasRole**
```
{{ $result := targetHasRole <target> <role> }}
```
Reports whether the target member has the specified role.
- `role`: a role ID, mention, name or role object.

**targetHasRoleID**
```
{{ $result := targetHasRoleID <target> <roleID> }}
```
Reports whether the specified target has the specified role ID. `target` may be a user ID, mention, or user object, but must be a member of the server.

**targetHasRoleName**
```
{{ $result := targetHasRoleName <target> <roleName> }}
```
Reports whether the specified target has the specified role name (case-insensitive). `target` may be a user ID, mention, or user object, but must be a member of the server.

### String Manipulation Functions

All regexp functions are limited to 10 different pattern calls per CC.

**hasPrefix**
```
{{ $result := hasPrefix <string> <prefix> }}
```
Reports whether the given `string` begins with `prefix`.

**hasSuffix**
```
{{ $result := hasSuffix <string> <suffix> }}
```
Reports whether the given `string` ends with `suffix`.

**joinStr**
```
{{ $result := joinStr <separator> <args...> }}
```
Concatenates `args...` in order, inserting the given `separator` between consecutive arguments and returns the result. As a special case, slices of strings are formatted as if each element was provided separately, so

```
{{ joinStr " " (cslice "cat" "dog") }}
```

yields `cat dog`.

**lower**
```
{{ $result := lower <string> }}
```
Converts `string` to all lowercase and returns the result.

**print**
```
{{ $result := print <args...> }}
```
Concatenates the arguments in order, adding spaces between arguments when neither is a string.

**println**
```
{{ $result := println <args...> }}
```
Concatenates the arguments in order, adding spaces between arguments and inserting a newline at the end.

**printf**
```
{{ $result := printf <format> <args...> }}
```
Interpolates `args...` according to `format`.

**sanitizeText**
```
{{ $result := sanitizeText <string> }}
```
Replaces accented and confusable characters in `string` with their normal, ISO-Latin variants.

**split**
```
{{ $result := split <string> <separator> }}
```
Splits `string` around each instance of `separator`, returning a slice of delimited substrings.

**title**
```
{{ $result := title <string> }}
```
Returns the string with the first letter of each word capitalized. (Title Case)

**trimSpace**
```
{{ $result := trimSpace <string> }}
```
Returns the string with all leading and trailing white space removed.

**upper**
```
{{ $result := upper <string> }}
```
Converts `string` to all uppercase and returns the result.

### Time Functions

**currentTime**
```
{{ $time := currentTime }}
```
Returns the current time in UTC.

**formatTime**
```
{{ $formatted := formatTime <time> <layout> }}
```
Formats `time` according to `layout`. Within the layout string, certain phrases represent placeholders that are replaced with the actual data from `time`: for instance, `Monday` is replaced with the weekday.

| Placeholder | Meaning |
|---|---|
| Mon | Weekday (abbreviated) |
| Monday | Weekday (full name) |
| 2 | Day of month (single digit) |
| 02 | Day of month (zero padded) |
| Jan | Month (abbreviated) |
| January | Month (full name) |
| 1 | Month (single digit) |
| 01 | Month (zero padded) |
| 15 | Hour (24-hour format) |
| 3 | Hour (12-hour format) |
| 04 | Minute (zero padded) |
| 05 | Second (zero padded) |
| MST | Timezone (abbreviated) |
| 2006 | Year (full year) |
| PM | AM-PM |

**humanizeDurationHours**
```
{{ $formatted := humanizeDurationHours <duration> }}
```
Returns `duration` as a human-readable string, rounded down to the nearest hour.

**humanizeDurationMinutes**
```
{{ $formatted := humanizeDurationMinutes <duration> }}
```
Returns `duration` as a human-readable string, rounded down to the nearest minute.

**humanizeDurationSeconds**
```
{{ $formatted := humanizeDurationSeconds <duration> }}
```
Returns `duration` as a human-readable string, rounded down to the nearest second.

**humanizeTimeSinceDays**
```
{{ $formatted := humanizeTimeSinceDays <time> }}
```
Returns the duration that has passed since the specified `time` as a human-readable string, rounded down to the nearest day.

**loadLocation**
```
{{ $location := loadLocation "location" }}
```
Searches the IANA Time Zone database for the given location name, returning the corresponding location object on success. (Given a time object `$time`, `$time.In $location` then returns a copy of the time set in the given location for display purposes.)

As a special case, providing `UTC`, or the empty string `""`, yields the UTC location. Providing `Local` yields the local time zone of the host YAGPDB server.

**newDate**
```
{{ $time := newDate <year> <month> <day> <hour> <minute> <second> [location] }}
```
Returns the time object corresponding to `yyyy-mm-dd hh:mm:ss` in the appropriate zone for that time in the given location.

The month, day, hour, min, and sec values may be outside their usual ranges and will be normalized during the conversion. For example, October 32 converts to November 1.

A daylight savings time transition skips or repeats times. For example, in the United States, March 13, 2011 2:15am never occurred, while November 6, 2011 1:15am occurred twice. In such cases, the choice of time zone, and therefore the time, is not well-defined. `newDate` returns a time that is correct in one of the two zones involved in the transition, but it does not guarantee which.

**parseTime**
```
{{ $time := parseTime <input> <layout> [location] }}
```
Undos the operation performed by `formatTime`: that is, given some `input` string representing a time using the given `layout`, `parseTime` returns the corresponding time object in the specified location, or UTC by default. If the input is invalid or does not follow `layout`, the zero time is returned.

A slice of layouts may be provided, in which case the input is matched against each in order until one matches or the end of the slice is reached.

**snowflakeToTime**
```
{{ $time := snowflakeToTime <snowflake> }}
```
Returns the UTC time at which the given Discord snowflake was created.

**timestampToTime**
```
{{ $time := timestampToTime <unixSeconds> }}
```
Returns the UTC time corresponding to the given UNIX time, measured in seconds since January 1, 1970.

**weekNumber**
```
{{ $week := weekNumber <time> }}
```
Returns the ISO 8601 week number in which the time occurs, ranging between 1 and 53. Jan 01 to Jan 03 of year n might belong to week 52 or 53 of year n-1, and Dec 29 to Dec 31 might belong to week 1 of year n+1.

Note: Discord Timestamp Formatting

Discord Timestamp Styles can be done using the `print` function:

`{{print "<t:" currentTime.Unix ":F>"}}` for "Long Date/Time" formatting.

### Type Conversion Functions

**structToSdict**
```
{{ $sdict := structToSdict <struct> }}
```
Constructs a sdict from the fields of `struct`.

**toByte**
```
{{ $bytes := toByte <string> }}
```
Converts `string` to a slice of UTF-8 bytes.

**toDuration**
```
{{ $duration := toDuration <x> }}
```
Converts the input, which may be a number (interpreted as nanoseconds) or a duration string such as `5m`, to a duration object. Returns the zero duration for invalid inputs.

**toFloat**
```
{{ $y := toFloat <x> }}
```
Converts the input `x` to a float64, returning zero for invalid inputs.

**toInt64**
```
{{ $n := toInt64 <x> [base] }}
```
Converts the input to an int64 in the given base (0, 2 to 36), returning zero for invalid inputs. Defaults to base 10 (decimal) if not specified.

**toInt**
```
{{ $n := toInt <x> [base] }}
```
Converts the input to an integer in the given base (0, 2 to 36), returning zero for invalid inputs. Defaults to base 10 (decimal) if not specified.

**toRune**
```
{{ $runes := toRune <string> }}
```
Converts the given string to a slice of runes (Unicode code points).

**toString**
```
{{ $str := toString <x> }}
```
Converts the input to a string, returning the empty string for invalid inputs.

Aliases: `str`.

### User Functions

**currentUserAgeHuman**
```
{{ $age := currentUserAgeHuman }}
```
Returns the account age of the current user as a human-readable string.

**currentUserAgeMinutes**
```
{{ $age := currentUserAgeMinutes }}
```
Returns the account age of the current user in a human-readable format, rounded down to minutes.

**currentUserCreated**
```
{{ $time := currentUserCreated }}
```
Returns the time object corresponding to when the current user was created.

**userArg**
```
{{ $user := userArg <input> }}
```
Returns the full user object specified by `input`, which can be an ID or a mention.

### Miscellaneous Functions

**adjective**
```
{{ $adj := adjective }}
```
Returns a random adjective.

**cembed**
```
{{ $embed := cembed [title] [url] [description] [color] [fields] [author] [thumbnail] [image] [footer] }}
```
Returns an embed object to send with `sendMessage`-related functions.

All keys are optional, but the Discord API will reject completely empty embeds, so _some_ content is required.
- `title`: the title of the embed
- `url`: the URL to hyperlink the title with
- `description`: the main text
- `color`: which color to display on the left side of the embed
- `fields`: a slice of sdicts with the following keys:
  - `name`: the name of the field
  - `value`: which text to have inside this field
  - `inline`: an optional boolean whether this field should be displayed in-line with other fields
- `author`: Shows some details at the very top of the embed. Is an sdict with the following keys:
  - `name`: The name of the author
  - `url`: the URL to hyperlink the name with
  - `icon_url`: the author's icon
- `thumbnail`: a small image in the top-right corner. Is an sdict with the following keys:
  - `url`: the image's URL
- `image`: an image to display at full width at the bottom of the embed. Is an sdict with the following keys:
  - `url`: the image's URL
- `footer`: Shows some details at the very bottom of the embed. Is an sdict with the following keys:
  - `text`: the footer's text
  - `icon_url`: a small icon to display to the left of the footer's text
- `timestamp`: a (static) timestamp to display to the right of the footer's text

Tip: Custom Commands Embed Generator - https://yagpdbembeds.netlify.app

**createTicket**
```
{{ $ticket := createTicket <author> <topic> }}
```
Creates a new ticket associated to the specified author with given topic, returning a template ticket for that ticket.
- `author`: the member to associate this ticket with.
- `topic`: the topic of this ticket. Must be a string.

Warning: For this function to work correctly, the ticketing system must be enabled.

**cslice**
```
{{ $slice := cslice [values...] }}
```
Creates a slice of the provided values.

**dict**
```
{{ $dict := dict [values...] }}
```
Creates a dictionary from the provided key-value pairs. The number of parameters must be even.

**execAdmin**
```
{{ $resp := execAdmin "command" [args...] }}
```
Runs the given command with the provided (optional) arguments as the bot and returns the response.

This will not work for commands which have their response marked as a manual response, i.e. `wouldyourather` and `poll`.

**execTemplate**
```
{{ execTemplate "template" [data...] }}
```
Executes the associated `"template"` template, optionally with data.

**exec**
```
{{ $resp := exec "command" [args...] }}
```
Executes the given command with the provided (optional) arguments as the triggering user and returns the response.

This will not work for commands which have their response marked as a manual response, i.e. `wouldyourather` and `poll`.

**getWarnings**
```
{{ $warnings := getWarnings <user> }}
```
Returns a slice of warnings imposed on the specified user.
- `user`: the user to get the warnings for. Can be either a user ID or a user object.

**humanizeThousands**
```
{{ $formatted := humanizeThousands <number> }}
```
Places commas to separate groups of thousands in a number.
- `number`: the number to format. Must be an int or a string. Must be a whole number.

**in**
```
{{ $result := in <sequence> <value> }}
```
Returns whether the case-sensitive `value` is in the `sequence`. `sequence` can be a slice or string.

**inFold**
```
{{ $result := inFold <sequence> <value> }}
```
Same as `in`, but is case-insensitive. `sequence` can be a slice or string.

**index**
```
{{ $item := index <list> <index> }}
```
Returns the item at the specified index in `list`.

`list` is zero-indexed, so the first item is at index 0. `list` can be a slice, map, or string.

If you are indexing a map (that is, `dict` or `sdict`), `index` must be matching the key (as maps are unordered). Indexing a string type returns the character at that position as a rune.

You may optionally add additional indices in case you have nested structures, like `index $x 0 1`. This is equivalent to chaining `index` calls, e.g. `index (index $x 0) 1`.

**kindOf**
```
{{ $kind := kindOf <value> [indirect] }}
```
Returns the kind of the provided value.

If `value` is behind an `interface{}` or pointer, set `indirect` to true to read the inner value. Most users of this function will want to do this.

**len**
```
{{ $length := len <list> }}
```
Returns the length of the provided list. `list` can be a slice, map, or string.

**noun**
```
{{ $noun := noun }}
```
Returns a random noun.

**parseArgs**
```
{{ $args := parseArgs <requiredArgs> <errorMessage> [...cargs] }}
```
Parses arguments passed to the custom command. Ensures that at least `requiredArgs` are passed and checks that these arguments match as specified by `carg...`, emits `errorMessage` otherwise.

Passing an empty string as `errorMessage` will generate one for you based on the provided argument definitions.

The result has the `.Get N` and `.IsSet N` methods available, returning the value or reporting whether the argument is present, respectively, at position `N` (starting from 0).

- `...cargs`: a list of argument definitions. Must have at least `requiredArgs` elements. Has the following arguments:
  - `"type"`: the type of this argument as a quoted string.
  - `"name"`: the name of this argument. Must be a string.

An argument's `"type"` must be one of the following:
- `int`: whole number
- `float`: decimal number
- `string`: text
- `user`: user mentions, resolves to a user object
- `userid`: mentions or user IDs, resolves to the ID itself
- `member`: mentions or user IDs, resolves to a member object
- `channel`: channel mention or ID, resolves to a channel object
- `role`: role name or ID, resolves to a role object
- `duration`: duration that is human-readable, i.e `10h5m` or `10 hour 5 minutes` would both resolve to the same duration

Additionally, the `int`, `float`, and `duration` type support validation ranges in the interval `(min, max)`, where for `duration` it is in time.Duration format values.

Example:
```
{{ $args := parseArgs 1 "" (carg "int" "coolness level" 0 100) (carg "member" "target member") }}
Coolness: {{ $args.Get 0 }}
{{- if $args.IsSet 1 }}
Target: {{ ($args.Get 1).User }}
{{- else }}
Target: {{ .User }}
{{ end }}
```

**sdict**
```
{{ $map := sdict [values...] }}
```
Creates a dictionary from the provided key-value pairs. The number of parameters must be even. The keys must be of string type.

**sendTemplateDM**
```
{{ $messageID := sendTemplateDM "template" [data] }}
```
Same as `sendTemplate`, but sends it to the triggering user's direct messages instead and returns the _response_ message's ID.

**sendTemplate**
```
{{ $messageID := sendTemplate <channel> "template" [data] }}
```
Sends an Associated Template to `channel`, with optional `data`, returning the _response_ message's ID.

**seq**
```
{{ $sequence := seq <start> <stop> }}
```
Creates a new slice with integer-type elements, starting from `start` and ending at `stop-1`. `start` and `stop` must be whole numbers. Limited to 10,000 elements.

**shuffle**
```
{{ $shuffled := shuffle <list> }}
```
Returns a shuffled (randomized) version of the provided list.

**sleep**
```
{{ sleep <seconds> }}
```
Pauses the execution of the custom command for the specified number of seconds. The maximum duration is 60 seconds, combined across all `sleep` calls within the custom command and its associated templates.

**slice**
```
{{ $result := slice <item> <start> [end] }}
```
Returns a subslice of the input `item` (which may be an array, slice, or string) containing the elements starting at index `start` (inclusive), and ending at index `end`, exclusive. If only `start` is provided, it is interpreted as the start index and the subslice extends to the end of `item`.

**sort**
```
{{ $sorted := sort <list> [options] }}
```
Returns the given list in a sorted order. The list's items must all be of the same type. The optional `options` argument is an sdict with the following (optional) keys:
- `key`: if sorting a list of maps, the key's value to sort by. This key must be present on all maps in the slice.
- `reverse`: whether to sort in reverse (descending) order. Default: `false`.

Limited to 1 call on regular servers and 3 calls on premium servers.

**verb**
```
{{ $verb := verb }}
```
Returns a random verb.
