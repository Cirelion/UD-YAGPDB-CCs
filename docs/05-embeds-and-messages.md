# Embeds & Messages Reference

## Creating Embeds in CCs

### cembed Function
```
{{ $embed := cembed
    "title" "My Title"
    "description" "Description with **markdown**"
    "color" 0xff0000
    "url" "https://example.com"
    "thumbnail" (sdict "url" "https://image.url/thumb.png")
    "image" (sdict "url" "https://image.url/big.png")
    "author" (sdict
        "name" .User.Username
        "icon_url" (.User.AvatarURL "256")
        "url" "https://example.com"
    )
    "footer" (sdict
        "text" "Footer text"
        "icon_url" "https://image.url/icon.png"
    )
    "fields" (cslice
        (sdict "name" "Field 1" "value" "Value 1" "inline" true)
        (sdict "name" "Field 2" "value" "Value 2" "inline" true)
    )
    "timestamp" currentTime
}}
{{ sendMessage nil $embed }}
```

### Colors
Use hex: `0xff0000` (red), `0x00ff00` (green), `0x0000ff` (blue). Convert from hex string using decimal format.

### Sending
```
{{ sendMessage nil $embed }}                    // Current channel
{{ sendMessage channelID $embed }}              // Specific channel
{{ $id := sendMessageRetID nil $embed }}        // Get message ID back
```

### Editing Embeds
Must convert to sdict first:
```
{{ $newEmbed := structToSdict $embed }}
{{ $newEmbed.Set "title" "New Title" }}
{{ $newEmbed.Set "description" "New Description" }}
{{ editMessage nil $messageID (cembed $newEmbed) }}
```

## Complex Messages
Combine content, embeds, buttons, menus:
```
{{ $msg := complexMessage
    "content" "Text above embed"
    "embed" $embed
    "buttons" (cbutton "label" "Click Me" "custom_id" "my-btn" "style" 1)
    "reply" .Message.ID
    "silent" true
}}
{{ sendMessage nil $msg }}
```

### Editing Complex Messages
```
{{ $edit := complexMessageEdit
    "content" "Updated text"
    "embed" $newEmbed
}}
{{ editMessage nil $messageID $edit }}
```

## SimpleEmbed Command (outside CCs)
```
-se -title "Title" -desc "Description" -color ff0000 -thumbnail "url" -image "url"
```

## CustomEmbed Command (outside CCs)
```
-ce {"title": "Title", "description": "Desc", "color": 16711680}
```

## Components V2 (Advanced)
New component system with richer layouts:

### Text Display
```
{{ $b := componentBuilder }}
{{ $b.Add "text" "Hello **world**" }}
```

### Sections (text + accessory)
```
{{ $b.Add "section" (sdict
    "text" "Section content"
    "accessory" (cbutton "label" "Click" "custom_id" "btn-1" "style" 1)
) }}
```

### Containers (grouping with color)
```
{{ $b.Add "container" (sdict
    "color" "#ff0000"
    "components" (cslice ...)
) }}
```

### Separators
```
{{ $b.Add "separator" true }}   // Large gap
{{ $b.Add "separator" false }}  // Small gap
```

### File Attachments
```
{{ $b.Add "file" (sdict
    "name" "output"
    "content" "File content here"
    "spoiler" false
) }}
```

### Gallery (multiple images)
```
{{ $b.Add "gallery" (cslice
    (sdict "url" "https://img1.png")
    (sdict "url" "https://img2.png")
) }}
```
