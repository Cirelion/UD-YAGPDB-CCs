# Custom Commands - Complete Guide

## Creating Custom Commands
Navigate to Core > Custom Commands on the control panel. Each CC gets a unique incrementing ID (cannot be modified).

## Command Groups
Organizational containers allowing role/channel restrictions across multiple commands. **Denylist takes precedence over allowlist.**

## Trigger Types

### 1. Command
Matches messages starting with server prefix OR bot mention + trigger text.

### 2. Starts With
Messages beginning with trigger text.

### 3. Contains
Messages including trigger text anywhere.

### 4. Regex
Pattern matching using Go RE2 regex (no lookaheads/lookbehinds).

### 5. Exact Match
Precise message matching.

### 6. Reaction
Emoji reactions. Options: Added Only, Removed Only, or Both.

### 7. Hourly/Minute Interval
- Min: 5 minutes, Max: 1 month
- Up to 5 triggers with ≤10min intervals per server
- Channel specification mandatory
- **No user/member context** — `.User.ID` returns nothing, member functions like `addRoleID` fail
- Supports excluding specific hours/weekdays (UTC)

### 8. Component (Button/Select Menu)
Uses RegEx matching on custom IDs. Triggered by button clicks or select menu selections.

### 9. Modal
Form submission triggers. Uses RegEx matching on custom IDs.

### 10. Crontab
Advanced scheduling using cron syntax (5 fields):

| Field | Values | Special Chars |
|-------|--------|--------------|
| Minutes | 0-59 | * / , - |
| Hours | 0-23 | * / , - |
| Day of month | 1-31 | * / , - ? |
| Month | 1-12 or JAN-DEC | * / , - |
| Day of week | 0-6 or SUN-SAT | * / , - ? |

**Examples:**
- `45 23 * * 6` → Saturday at 23:45
- `0 * * * *` → Every hour
- `0 0 * * *` → Daily at midnight
- Must have >10 minute intervals between executions

### 11. Role Changes
Fires when roles are added/removed from members.

**Special context:**
- `.TargetMember` / `.TargetUser` — Member/User whose roles changed
- `.Author` — User performing the change
- `.Role` — Added/removed role object
- `.RoleAdded` — Boolean (true=added, false=removed)

**Restrictions:** Won't trigger on bots. Cooldown: 5min free, 1min premium. Max: 1 free, 5 premium. Cannot use execCC/scheduleUniqueCC.

## CC Settings
- **Case Sensitivity:** Off by default
- **Edit Message Trigger:** Premium only
- **Output Errors Toggle:** Show execution errors in response
- **Command Enabled Toggle:** Disable entirely (also prevents execCC)
- **Save shortcut:** Alt + Shift + S

## CC Limits

| Limit | Free | Premium |
|-------|------|---------|
| Max CCs | 100 | 250 |
| Triggered per action | 3 | 5 |
| Character limit | 10,000 (5,000 for join/leave) | 10,000 |
| Max operations | 1,000,000 | 2,500,000 |
| Response char limit | 2,000 | 2,000 |
| Generic API calls | 100/CC | 100/CC |
| State lock actions | 500/CC | 500/CC |

### execCC Limits
| Limit | Free | Premium |
|-------|------|---------|
| Calls per CC | 1 | 10 |
| Stack depth | 2 (zero delay) | 2 |
| Channel limit | 10/channel/min | 10/channel/min |

### Other Limits
- sendDM: 1 call
- sendTemplate: 3 calls
- addReactions: 20 calls
- editChannelName: 10 calls
- regex cache: 10
- editNickname: 2 calls
- sleep: 60 seconds max
- scheduleUniqueCC: 1 free / 10 premium (1 per server per key)
- cancelScheduledUniqueCC: 10

## Database

### Entry Fields
- `.ID` — System-assigned unique ID
- `.GuildID` — Server ID
- `.UserID` — User-defined (any int64, including 0 for global)
- `.User` — User object (invalid when UserID is 0)
- `.CreatedAt` / `.UpdatedAt` / `.ExpiresAt` — Timestamps
- `.Key` — String, max 256 chars
- `.Value` — The stored data
- `.ValueSize` — Size in bytes, max 100kB

### Database Limits
| Limit | Free | Premium |
|-------|------|---------|
| Max entries | members×50 | members×500 |
| Key length | 256 chars | 256 chars |
| Value size | 100kB | 100kB |
| DB interactions/CC | 10 | 50 |
| Multi-entry interactions | 2 | 10 |

### Basic Functions
```
{{ dbSet userID "key" value }}          // Create/overwrite
{{ dbGet userID "key" }}                // Returns entry object, .Value for data
{{ dbDel userID "key" }}                // Delete
{{ dbIncr userID "key" increment }}     // Increment (returns float), auto-creates
{{ dbSetExpire userID "key" value seconds }}  // Auto-expiring entry
```

### Multi-Entry Functions
```
{{ dbCount userID }}                          // Count entries
{{ dbCount "pattern" }}                       // Count by pattern
{{ dbTopEntries "pattern" amount skip }}      // Descending by value (max 100)
{{ dbBottomEntries "pattern" amount skip }}   // Ascending by value
{{ dbGetPattern userID "pattern" amount skip }}
{{ dbGetPatternReverse userID "pattern" amount skip }}
{{ dbDelMultiple (sdict "userID" id "pattern" "pat%") amount skip }}
{{ dbRank (sdict "pattern" "key") userID "key" }}
```

### Pattern Syntax (SQL LIKE)
- `%` matches zero or more characters
- `_` matches exactly one character
- Escape: `\%` and `\_`

### Important DB Tips
- **IDs lose precision as floats** — always convert to string before storing: `{{ dbSet 0 "key" (str .User.ID) }}`
- **Global vs User entries:** Two entries are unique if UserID OR Key differs
- `sdict`, `dict`, and `cslice` retain type on retrieval
- Custom types (like `cembed`) need reconversion after retrieval

## Installing Community CCs

### Standalone Commands
1. Go to Core > Custom Commands on control panel
2. Set trigger type and value as specified
3. Copy code into response field
4. Apply channel/role restrictions if needed
5. Configure variables between config comments

### Command Systems (Multi-CC packages)
1. Create a CC group for organization
2. Add each CC component with its specified trigger
3. Configure shared variables (like CC IDs referencing each other)
4. Follow system-specific setup instructions
