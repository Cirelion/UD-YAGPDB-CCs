# Regex Reference for YAGPDB

## Engine
YAGPDB uses Go's RE2 regex engine. **No lookaheads or lookbehinds.**

## Basic Patterns
| Pattern | Matches |
|---------|---------|
| `.` | Any character (except newline) |
| `*` | 0 or more of previous |
| `+` | 1 or more of previous |
| `?` | 0 or 1 of previous |
| `\d` | Digit [0-9] |
| `\w` | Word char [a-zA-Z0-9_] |
| `\s` | Whitespace |
| `\D`, `\W`, `\S` | Negated versions |
| `^` or `\A` | Start of string |
| `$` or `\z` | End of string |
| `[abc]` | Character class |
| `[^abc]` | Negated class |
| `[A-z]` | Range |
| `(...)` | Capture group |
| `(?:...)` | Non-capturing group |
| `a|b` | Alternation |
| `{n}` | Exactly n times |
| `{n,m}` | Between n and m times |

## YAGPDB Regex Functions
```
{{ reFind `pattern` "string" }}                    // First match
{{ reFindAll `pattern` "string" }}                 // All matches (slice)
{{ reFindAllSubmatches `pattern` "string" }}       // All submatches
{{ reReplace `pattern` "string" "replacement" }}   // Replace
{{ reSplit `pattern` "string" }}                   // Split
{{ reQuoteMeta "string" }}                         // Escape metacharacters
```

## Common YAGPDB Patterns
```
\A                          // Start of string (use in triggers)
\A-mycommand\b              // Command trigger
\A(-|<@!?204255221017214977>\s*)  // Prefix or bot mention
\d+                         // One or more digits
(?i)                        // Case insensitive flag
(?:color=)(red|blue|green)  // Non-capturing group + alternatives
```

## Tips
- Use backticks for regex in templates: `` `pattern` `` (avoids escaping)
- Test at regex101.com (select Go/RE2 flavor)
- Regex cache limit: 10 per CC
- YAGPDB link regex: `(?i)([a-z\d]+:[//])([\w-._~:/?#\[\]@!$&'()*+,;%=]+(?:(?:\.[\w_-]+)+))([\w.,@?^=%&:/~+#-]*[\w@?^=%&/~+#-])`

## How to Get IDs
- **User ID:** `\@username` or right-click > Copy ID (developer mode)
- **Channel ID:** `\#channel` or right-click > Copy ID
- **Role ID:** `-listroles` command
- **Custom Emoji ID:** `\:emojiname:` to see full format
- **Animated Emoji:** Same, check URL for ID
