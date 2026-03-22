# Common YAGPDB CC Patterns & Recipes

## Cooldown System
```
{{ if $cd := dbGet .User.ID "cmd_cooldown" }}
    On cooldown! Try again in {{ $cd.ExpiresAt.Sub currentTime | humanizeDurationSeconds }}.
    {{ return }}
{{ end }}
{{ dbSetExpire .User.ID "cmd_cooldown" true 60 }}
{{/* Command logic here */}}
```

## Global Cooldown
```
{{ if $cd := dbGet 0 "global_cooldown" }}
    {{ return }}
{{ end }}
{{ dbSetExpire 0 "global_cooldown" true 30 }}
```

## parseArgs Pattern
```
{{ $args := parseArgs 2 "Usage: `-cmd <number> <user>`"
    (carg "int" "amount" 1 100)
    (carg "member" "target user")
    (carg "string" "optional message")
}}
{{ $amount := $args.Get 0 }}
{{ $target := $args.Get 1 }}
{{ if $args.IsSet 2 }}
    {{ $msg := $args.Get 2 }}
{{ end }}
```

## Currency/Economy Pattern
```
{{/* Give currency */}}
{{ $balance := dbIncr .User.ID "CREDITS" 100 }}
You now have {{ toInt $balance }} credits.

{{/* Take currency */}}
{{ $balance := toInt (dbGet .User.ID "CREDITS").Value }}
{{ if lt $balance $cost }}
    Not enough credits! You have {{ $balance }}.
    {{ return }}
{{ end }}
{{ $newBalance := dbIncr .User.ID "CREDITS" (mult -1 $cost) }}
```

## Leaderboard Pattern
```
{{ $entries := dbTopEntries "CREDITS" 10 0 }}
{{ $embed := cembed "title" "Leaderboard" "color" 0x00ff00 }}
{{ $desc := "" }}
{{ range $i, $e := $entries }}
    {{- $desc = printf "%s\n**#%d** <@%d> — %d points" $desc (add $i 1) $e.UserID (toInt $e.Value) -}}
{{ end }}
{{ $embed := cembed "title" "Leaderboard" "description" $desc "color" 0x00ff00 }}
{{ sendMessage nil $embed }}
```

## Pagination Pattern
```
{{ $page := 0 }}
{{ with .CmdArgs }}{{ $page = sub (toInt (index . 0)) 1 }}{{ end }}
{{ if lt $page 0 }}{{ $page = 0 }}{{ end }}
{{ $perPage := 10 }}
{{ $entries := dbTopEntries "key%" $perPage (mult $page $perPage) }}
```

## Sticky Message Pattern
```
{{ if $db := dbGet .Channel.ID "sticky_msg_id" }}
    {{ deleteMessage nil (toInt $db.Value) 0 }}
{{ end }}
{{ $id := sendMessageRetID nil "Sticky content here" }}
{{ dbSet .Channel.ID "sticky_msg_id" (str $id) }}
```

## Self-Deleting Response
```
{{ $msg := sendMessageRetID nil "Temporary message" }}
{{ deleteMessage nil $msg 5 }}
{{ deleteTrigger 0 }}
```

## Embed with User Info
```
{{ $embed := cembed
    "author" (sdict "name" .User.Username "icon_url" (.User.AvatarURL "256"))
    "color" 0x7289DA
    "description" "Content here"
    "footer" (sdict "text" (printf "ID: %d" .User.ID))
    "timestamp" currentTime
}}
```

## Role Check Guard
```
{{ if not (hasRoleID $requiredRole) }}
    You don't have permission to use this command.
    {{ return }}
{{ end }}
```

## Channel-Specific Execution
```
{{ if ne .Channel.ID $allowedChannel }}
    This command can only be used in <#{{ $allowedChannel }}>.
    {{ return }}
{{ end }}
```

## Storing Complex Data
```
{{/* Store */}}
{{ $data := sdict "count" 0 "users" (cslice) "active" true }}
{{ dbSet 0 "game_data" $data }}

{{/* Retrieve and modify */}}
{{ $data := sdict (dbGet 0 "game_data").Value }}
{{ $data.Set "count" (add $data.count 1) }}
{{ dbSet 0 "game_data" $data }}
```

## Multi-CC Communication via execCC
```
{{/* Main CC */}}
{{ execCC $otherCCID nil 0 (sdict "action" "process" "userID" .User.ID "data" $someData) }}

{{/* Other CC (receiving) */}}
{{ with .ExecData }}
    {{ if eq .action "process" }}
        {{/* Handle the data */}}
    {{ end }}
{{ end }}
```

## Scheduled Recurring Task
```
{{/* Initial setup (run once) */}}
{{ scheduleUniqueCC .CCID nil 3600 "hourly_task" nil }}

{{/* In the CC itself */}}
{{ if .ExecData }}
    {{/* Do recurring work */}}
    {{ scheduleUniqueCC .CCID nil 3600 "hourly_task" nil }}
{{ end }}
```

## Button Confirmation Pattern
```
{{/* Send message with confirm/cancel buttons */}}
{{ $confirm := cbutton "label" "Confirm" "custom_id" "action-confirm" "style" 3 }}
{{ $cancel := cbutton "label" "Cancel" "custom_id" "action-cancel" "style" 4 }}
{{ sendMessage nil (complexMessage
    "content" "Are you sure?"
    "buttons" (cslice $confirm $cancel)
) }}

{{/* Component handler CC */}}
{{ if eq .StrippedID "confirm" }}
    {{ updateMessage nil "Action confirmed!" }}
{{ else }}
    {{ updateMessage nil "Action cancelled." }}
{{ end }}
```

## ID Storage Best Practice
```
{{/* Always store IDs as strings */}}
{{ dbSet 0 "target_user" (str .User.ID) }}

{{/* Retrieve and convert back */}}
{{ $userID := toInt (dbGet 0 "target_user").Value }}
```

## Error Handling
```
{{ $err := "" }}
{{ if not .CmdArgs }}
    {{ $err = "Please provide arguments." }}
{{ else if not (getMember (toInt (index .CmdArgs 0))) }}
    {{ $err = "User not found." }}
{{ end }}
{{ if $err }}
    {{ $msg := sendMessageRetID nil $err }}
    {{ deleteMessage nil $msg 5 }}
    {{ deleteTrigger 0 }}
    {{ return }}
{{ end }}
```
