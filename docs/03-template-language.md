# YAGPDB Template Language Reference

## Core Concepts

### The Dot (`.`)
The dot represents current context data. `.User` = triggering user, `.Channel` = trigger channel, etc.

### Variables
```
{{ $name := "value" }}    // Define (new variable)
{{ $name = "new" }}       // Reassign (existing variable)
```
Scope extends to enclosing control structure's `end`.

### Special Variable `$`
Always refers to the initial/global context. Essential inside `range` blocks where `.` is overwritten.

### Pipes
`{{ randInt 41 | add 2 }}` — chain output as next function's last argument.

## Data Types

### Strings
```
"Hello, world!"              // Standard string
"Line 1\nLine 2"             // With escape sequences
`Raw string with
newlines preserved`           // Raw string literal (backticks)
"He said \"hello\""          // Escaped quotes
```

### Numbers
- **Integers:** 64-bit signed. `42`, `0x2A` (hex), `0o52` (octal), `0b101010` (binary)
- **Floats:** 64-bit IEEE-754. `3.14`

### Booleans
`true` / `false`. Empty/zero values are falsy.

### Custom Types
- **cslice** (templates.Slice): Ordered list
- **sdict** (templates.SDict): String-keyed map
- **dict**: Any-comparable-key map

## Context Data Fields

### Global
| Field | Type | Description |
|-------|------|-------------|
| `.BotUser` | User | Bot's user object |
| `.CCID` | int64 | Current CC ID |
| `.CCRunCount` | int | Cached run count |
| `.CCTrigger` | string | Printable trigger name |
| `.CustomID` | string | Full custom ID (components/modals) |
| `.StrippedID` | string | Custom ID with trigger prefix removed |
| `.Values` | []string | Selected options or modal inputs |
| `.DomainRegex` | string | Domain-matching regex |
| `.IsMessageEdit` | bool | Whether triggered by edit |
| `.IsPremium` | bool | Guild premium status |
| `.LinkRegex` | string | Link-matching regex |
| `.Permissions` | Permissions | Permission bits |
| `.ServerPrefix` | string | Command prefix |
| `.ExecData` | interface | Data from execCC/scheduleUniqueCC |

### User (`.User`)
`.User.ID`, `.User.Username`, `.User.Discriminator`, `.User.Avatar`, `.User.Bot`, `.User.GlobalName`
- `.User.AvatarURL "size"` — Returns avatar URL
- `.User.Mention` — Mention string
- `.User.String` — username#discrim format

### Member (`.Member`)
`.Member.GuildID`, `.Member.JoinedAt`, `.Member.Nick`, `.Member.Roles`, `.Member.User`, `.Member.PremiumSince`

### Channel (`.Channel`)
`.Channel.ID`, `.Channel.Name`, `.Channel.Topic`, `.Channel.NSFW`, `.Channel.ParentID`, `.Channel.Position`, `.Channel.Type`
`.Channel.IsForum`, `.Channel.IsThread`, `.Channel.IsPrivate`

### Guild (`.Guild` / `.Server`)
`.Guild.ID`, `.Guild.Name`, `.Guild.Icon`, `.Guild.MemberCount`, `.Guild.OwnerID`, `.Guild.AfkChannelID`

### Message (`.Message`)
`.Message.ID`, `.Message.Content`, `.Message.Author`, `.Message.Attachments`, `.Message.Embeds`, `.Message.MentionRoles`, `.Message.Mentions`, `.Message.Reactions`, `.Message.GuildID`

### Reaction (`.Reaction`) — Reaction triggers only
`.Reaction.UserID`, `.Reaction.MessageID`, `.Reaction.ChannelID`, `.Reaction.Emoji.Name`, `.Reaction.Emoji.ID`, `.ReactionAdded` (bool)

### Interaction (`.Interaction`) — Component/Modal triggers
`.Interaction.Token` — Required for followups, unique per interaction

### Args
`.Args` — Slice of all words in message
`.CmdArgs` — Slice of args after trigger (Command trigger only)
`.Cmd` — The trigger word itself
`.StrippedMsg` — Message without trigger

## Control Flow

### If/Else
```
{{ if eq $x 5 }}
  Five!
{{ else if gt $x 5 }}
  More than five!
{{ else }}
  Less than five!
{{ end }}
```

### Comparison Functions
`eq` (==), `ne` (!=), `lt` (<), `le` (<=), `gt` (>), `ge` (>=)
- **Cannot compare different types** (e.g., number vs string)
- `eq` supports multiple args: `{{ eq $x 1 2 3 }}` = true if $x is 1, 2, OR 3

### Boolean Logic
`and`, `or`, `not`

### Range (loops)
```
// Over slices
{{ range $slice }}
  {{ . }}   {{/* dot = current element */}}
{{ end }}

// With index
{{ range $i, $v := $slice }}
  {{ $i }}: {{ $v }}
{{ end }}

// Over maps
{{ range $key, $val := $map }}
  {{ $key }}: {{ $val }}
{{ end }}

// Fixed iterations
{{ range 5 }}...{{ end }}
{{ range seq 5 10 }}...{{ end }}

// With else (empty collection)
{{ range $items }}...{{ else }}No items{{ end }}
```

**Inside range, use `$` to access global context:**
```
{{ range $items }}
  {{ $.User.Username }}   {{/* NOT .User.Username */}}
{{ end }}
```

### While
```
{{ while condition }}
  ...
{{ end }}
```

### Break and Continue
`{{ break }}` exits loop, `{{ continue }}` skips to next iteration.

### With
Executes if truthy, overwrites dot:
```
{{ with reFind "pattern" .Message.Content }}
  Found: {{ . }}
{{ end }}
```

### Try-Catch
```
{{ try }}
  {{ riskyOperation }}
{{ catch }}
  Error occurred
{{ end }}
```

### Return
`{{ return }}` — Exit CC early (guard clauses).

## Whitespace Control
- `{{- ` trims leading whitespace
- ` -}}` trims trailing whitespace
- Think of them as arrows eating whitespace in the direction they point

## Associated Templates
```
{{ define "myTemplate" }}
  Content here, dot is passed data
{{ end }}

{{ template "myTemplate" . }}
{{ execTemplate "myTemplate" $data }}  // Returns string output
```
