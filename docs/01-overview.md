# YAGPDB Overview & Getting Started

## What is YAGPDB?
YAGPDB (Yet Another General Purpose Discord Bot) is a feature-rich Discord bot with custom commands, moderation, notifications, role management, and more.

- **Website:** https://yagpdb.xyz
- **Control Panel:** https://yagpdb.xyz/manage
- **Official Docs:** https://help.yagpdb.xyz/
- **CC Repository:** https://yagpdb-cc.github.io/
- **Support Discord:** https://discord.gg/4udtcA5
- **GitHub:** https://github.com/botlabs-gg/yagpdb

## Adding the Bot
1. Requires "Manage Server" permission
2. Navigate to https://yagpdb.xyz/manage
3. Login with Discord, authorize, select server

## First Steps
- Check Commands section (Core > Command Settings)
- Default prefix: `-` (customizable)
- Test with sample commands like `-catfact` or `-dadjoke`

## Premium Features
| Feature | Free | Premium |
|---------|------|---------|
| Custom Commands | 100 | 250 |
| Triggered per action | 3 | 5 |
| CC Character limit | 10,000 | 20,000 |
| Max operations | 1,000,000 | 2,500,000 |
| DB interactions per CC | 10 | 50 |
| DB entries | members×50 | members×500 |
| execCC calls per CC | 1 | 10 |
| Edit message triggers | No | Yes |
| Reddit feeds | 100 | 1,000 |
| YouTube feeds | 10 | 250 |
| Automod rules | 25 | 150 |
| Automod lists | 5 | 25 |
| Deleted msg retention | 1 hour | 12 hours |
| Voice roles | 1 | 10 |
| RSS feeds | 2 | 10 |
| Twitch feeds | 3 | 15 |
| Soundboard sounds | 50 | 250 |

## Control Panel Access
- **Read Access:** Role-based, all members, or public
- **Write Access:** Anyone with write access can edit everything; Manage Server permission = auto write access
- **Control Panel Logs:** Audit trail for config changes

## Command Settings
- **Prefix:** Default `-`, customizable
- **Override Priority (least to most specific):** All commands enabled → Global restrictions → Channel-specific restrictions
- **Options:** Required/ignored roles, autodelete intervals, ephemeral slash commands, channel overrides
