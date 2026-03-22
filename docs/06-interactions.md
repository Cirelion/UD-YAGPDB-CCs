# Custom Interactions Guide

## Interaction Lifecycle
1. User clicks button/menu or submits modal
2. Interaction sent to YAGPDB, triggers matching CC
3. Bot MUST respond within **3 seconds** (message, modal, or message update)
4. Optional followups for up to **15 minutes**

## Buttons

### Creating Buttons
```
{{ $btn := cbutton
    "label" "Click Me"
    "custom_id" "my-button"
    "style" 1
    "emoji" (sdict "name" "🎉")
    "disabled" false
}}
{{ sendMessage nil (complexMessage "buttons" $btn) }}
```

### Button Styles
| Style | Name | Color |
|-------|------|-------|
| 1 | Primary | Blurple |
| 2 | Secondary | Grey |
| 3 | Success | Green |
| 4 | Danger | Red |
| 5 | Link | Grey (opens URL, no interaction) |

### Multiple Buttons
```
{{ $btn1 := cbutton "label" "Join" "custom_id" "game-join" "style" 3 }}
{{ $btn2 := cbutton "label" "Leave" "custom_id" "game-leave" "style" 4 }}
{{ sendMessage nil (complexMessage "buttons" (cslice $btn1 $btn2)) }}
```

### Handling Button Clicks
CC Trigger: **Component**, Custom ID regex: `\Agame-`
```
{{ if eq .StrippedID "join" }}
    {{ sendResponse nil "You joined!" }}
{{ else if eq .StrippedID "leave" }}
    {{ sendResponse nil "You left!" }}
{{ end }}
```

## Select Menus

### Text Select Menu
```
{{ $menu := cmenu
    "type" "text"
    "custom_id" "color-picker"
    "placeholder" "Choose a color"
    "min_values" 1
    "max_values" 1
    "options" (cslice
        (sdict "label" "Red" "value" "red" "description" "A warm color" "emoji" (sdict "name" "🔴"))
        (sdict "label" "Blue" "value" "blue" "description" "A cool color" "emoji" (sdict "name" "🔵"))
    )
}}
{{ sendMessage nil (complexMessage "menus" $menu) }}
```

### Other Menu Types
- `"user"` — Auto-populated user list
- `"role"` — Auto-populated role list
- `"channel"` — Auto-populated channels (filter with `"channel_types"`)
- `"mentionable"` — Users + roles combined

### Handling Menu Selections
`.Values` contains selected options as a string slice:
```
{{ $selected := index .Values 0 }}
{{ sendResponse nil (printf "You selected: %s" $selected) }}
```

## Modals

### Creating Modals
```
{{ $modal := cmodal
    "title" "Feedback Form"
    "custom_id" "feedback-modal"
    "fields" (cslice
        (sdict
            "label" "Subject"
            "placeholder" "Enter subject..."
            "required" true
            "style" 1
            "min_length" 3
            "max_length" 100
        )
        (sdict
            "label" "Details"
            "placeholder" "Tell us more..."
            "required" false
            "style" 2
            "min_length" 0
            "max_length" 2000
        )
    )
}}
{{ sendModal $modal }}
```

### Field Styles
- `1` — Short (single line)
- `2` — Paragraph (multi-line)

### Handling Modal Submissions
CC Trigger: **Modal**, Custom ID regex matching your modal's custom_id.
`.Values` contains submitted text in field order:
```
{{ $subject := index .Values 0 }}
{{ $details := index .Values 1 }}
{{ sendResponse nil (printf "Subject: %s\nDetails: %s" $subject $details) }}
```

**Cannot send a modal in response to a modal submission.**

## Responding to Interactions

### Initial Response (required within 3 seconds)
1. Output text directly in CC response field
2. `{{ sendResponse nil "message" }}` — or with NoEscape/RetID variants
3. `{{ sendModal $modal }}` — Show a modal
4. `{{ updateMessage nil "new content" }}` — Edit the triggering message
5. `{{ ephemeralResponse }}` — Make response visible only to user (call before sendResponse)

### Followups (up to 15 minutes)
After initial response, `sendResponse` automatically becomes a followup.
- Edit: `{{ editResponse nil $messageID "new content" }}` (needs `.Interaction.Token`)
- Get: `{{ getResponse nil $messageID }}`

### Saving Token for Later
```
{{ dbSet .User.ID "interaction_token" .Interaction.Token }}
```

## Action Row Layout Control
```
{{ $row1 := cslice $btn1 $btn2 $btn3 }}
{{ $row2 := cslice $btn4 $btn5 }}
{{ sendMessage nil (complexMessage "components" (cslice $row1 $row2)) }}
```

## Emoji Format for Components
Components use partial emoji objects:
- Unicode: `(sdict "name" "🎉")`
- Custom: `(sdict "id" "123456789")`
- Custom with name: `(sdict "id" "123456789" "name" "myemoji")`
