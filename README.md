# 🎮 CS2 Case Bot V1.6 for Streamer.bot

<p align="center">
  <img src="CS2 Case Bot Logo.png" width="128" height="128">
</p>

![Version](https://img.shields.io/badge/version-1.6-blue.svg)
![Streamer.bot](https://img.shields.io/badge/Streamer.bot-1.0.4+-orange.svg)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-Support%20Me-FF5E5B?style=flat-square&logo=kofi&logoColor=white)](https://ko-fi.com/mk4gtiguy)
[![License](https://img.shields.io/badge/License-MIT%20%2B%20Disclaimer-yellow.svg?style=flat-square)](LICENSE)

**The COMPLETE CS2 case opening bot for Streamer.bot** – 12 cases, full sticker system, gold tiers, trading, gifting, jackpot, quests, lottery, giveaways, referrals, leaderboards, stats dashboard, and 45+ commands.

---
## **⚠️IMPORTAN UPDATE⚠️**
**⚠️TOOK SLEEP FROM CODE FOR RESPONSE 6/13/26 AT 8:05PM EST**
**IF YOU DOWNLED BEFORE THEN REDOWNLOAD AND UPDATE FILES**
**CODE FOR CS2OPEN_CASE AND CS2_OPENCAPSULE HAVE CHANGED⚠️**

## ⚠️ Disclaimer

> This bot is a **fan project for entertainment purposes only**. All CS2 skin names, case names, and trademarks are property of **Valve Corporation**. This bot is not affiliated with, endorsed by, or sponsored by Valve. No real money or actual CS2 items are involved — everything is simulated with fake currency and emoji-based items.

---

## 🚀 Quick Install (10 Minutes)

### One-Click Import (Actions)

1. Download [`CS2-Case-Bot-V1.6-Actions.sb`](CS2-Case-Bot-V1.6-Actions.sb)
2. In Streamer.bot → **Actions** → Right-click → **Import** → Select the file
3. Open [`persisted-globals.txt`](persisted-globals.txt) and copy each line (85 lines total)
4. In Streamer.bot → **Services** → **Global Variables** → **Persisted Globals** → Add each line
5. Set `!jackpotdraw` and admin commands to **Moderator** permission
6. Set up timers (see [Timer Setup](#timer-setup) below)
7. Type `!help` in your chat to test!

### Manual Setup (For Learning or Modifying)

Follow [`setup-guide.md`](setup-guide.md) and use the code in [`V1.6-Complete-Code-Reference.md`](V1.6-Complete-Code-Reference.md)

---

## ✨ Features

| Category | Features |
|----------|----------|
| **Cases** | 12 cases (Dreams, Recoil, Kilowatt, Revolution, Fracture, Clutch, Prisma, DangerZone, Horizon, Chroma3, Spectrum2, CS20) |
| **Stickers** | 5 capsules, 5 rarities (⭐✨💫🔥👑), apply to weapons, tracking history |
| **Gold Tiers** | Weapons: Common($150), Rare($300), Epic($600), Legendary($1000), Mythic($2500) |
| **Gold Sticker Tiers** | Common($30), Rare($75), Epic($150), Legendary($300) |
| **Trade-Up** | Blue→Purple→Pink→Red→Gold for weapons AND stickers |
| **Daily Reward** | 1 in 1,000,000 chance for $50,000 jackpot! |
| **Quests** | Daily quests with claimable rewards |
| **Lottery** | Weekly lottery with ticket purchases |
| **Referrals** | Locked down with anti-abuse measures |
| **Giveaway** | Auto-draw after 2 minutes |
| **Jackpot** | Auto-draw after 2 minutes of no entries |
| **Collections** | Classic, Operation, Legacy, Master collections |
| **Leaderboards** | Money, inventory, gold pulls, case opens, streaks |
| **Stats Dashboard** | Daily/weekly/monthly/cases/users tracking |
| **Admin Commands** | 12+ admin commands for full control |
| **Total Commands** | 45+ |

---

## 📋 Commands

### Case Commands
| Command | Description |
|---------|-------------|
| `!setcase [name]` | Switch your active case |
| `!case` | Open your selected case |
| `!caseinfo` | List all cases with prices |
| `!casestats` | Total opens per case |
| `!gift [qty] @user [case]` | Gift case(s) to someone |

### Sticker & Capsule Commands
| Command | Description |
|---------|-------------|
| `!setcapsule [name]` | Switch your active sticker capsule |
| `!capsules` | List all capsules with prices |
| `!opencapsule` | Open your selected capsule |
| `!giftcapsule [qty] @user [name]` | Gift capsule(s) to someone |
| `!stickers` | View your sticker inventory |
| `!apply [#] [#]` | Apply sticker to weapon |
| `!applied` | View your sticker application history |
| `!stickervalue` | Show sticker odds and values |

### Economy Commands
| Command | Description |
|---------|-------------|
| `!balance` | Check your balance |
| `!daily` | Claim daily reward ($10-$50,000) |
| `!inv` | View your weapon inventory |
| `!sell` | Show numbered inventory |
| `!sell [number]` | Sell specific item |
| `!sellall` | Sell entire inventory |
| `!sellhelp` | Selling instructions |
| `!tradeup [rarity]` | Trade up weapons |
| `!tradeup sticker [rarity]` | Trade up stickers |
| `!inventoryvalue` | Total inventory worth |

### Quest Commands
| Command | Description |
|---------|-------------|
| `!quests` | Show active daily quests |
| `!claim` | Claim completed quest rewards |

### Leaderboard Commands
| Command | Description |
|---------|-------------|
| `!topstreaks` | Longest open streaks |
| `!topmoney` | Richest users |
| `!topinventory` | Highest inventory value |
| `!topgolds` | Most gold pulls |
| `!topopens` | Most cases opened |

### Collection Command
| Command | Description |
|---------|-------------|
| `!collections` | Track collection progress |

### Lottery Commands
| Command | Description |
|---------|-------------|
| `!lottery` | Show lottery status |
| `!buyticket [quantity]` | Buy lottery tickets |

### Referral Commands
| Command | Description |
|---------|-------------|
| `!refer @user` | Refer a new user |
| `!referrals` | View your referral stats |

### Giveaway Commands
| Command | Description |
|---------|-------------|
| `!giveaway [amount]` | Start a giveaway (broadcaster only) |
| `!enter` | Enter active giveaway |

### Jackpot Commands
| Command | Description |
|---------|-------------|
| `!jackpot` | Enter jackpot with random weapon |
| `!jackpotinfo` | See current jackpot status |
| `!jackpotdraw` | Force draw winner (mods only - auto draws after 2 min) |

### Trading Commands
| Command | Description |
|---------|-------------|
| `!trade @user #` | Trade an item |
| `!accepttrade` | Accept trade offer |
| `!denytrade` | Deny trade offer |

### Stats Commands
| Command | Description |
|---------|-------------|
| `!mystats` | Your personal stats |
| `!streak` | Current open streak |
| `!badges` | Earned achievements |
| `!odds` | Drop rates and values |
| `!stats [daily/weekly/monthly/cases/users]` | Streamer stats dashboard |

### Admin Commands (Moderator+)
| Command | Description |
|---------|-------------|
| `!givebalance @user amount` | Add money |
| `!takebalance @user amount` | Remove money |
| `!giveitem @user type [case]` | Give item (skin/sticker/goldsticker) |
| `!resetuser @user` | Reset user data |
| `!setcaseprice Case price` | Change case cost |
| `!setcapsuleprice Capsule price` | Change capsule cost |
| `!givecapsule @user name` | Give and auto-open capsule |
| `!givegoldsticker @user tier` | Give gold sticker |

### Help
| Command | Description |
|---------|-------------|
| `!help` | Show all commands |
| `!help [command]` | Detailed help for specific command |

---

## 💰 Economy Values

### Weapon Values
| Rarity | Value |
|--------|-------|
| Blue | $0.25 |
| Purple | $1.00 |
| Pink | $4.00 |
| Red | $20.00 |

### Gold Weapon Tiers
| Tier | Value | Chance within Gold |
|------|-------|-------------------|
| Common | $150 | 50% |
| Rare | $300 | 30% |
| Epic | $600 | 15% |
| Legendary | $1000 | 4.5% |
| Mythic | $2500 | 0.5% |

### Sticker Values
| Rarity | Value |
|--------|-------|
| ⭐ | $0.10 |
| ✨ | $0.50 |
| 💫 | $2.00 |
| 🔥 | $10.00 |
| 👑 | $30-$300 |

### Gold Sticker Tiers
| Tier | Value | Chance |
|------|-------|--------|
| Common | $30 | 50% |
| Rare | $75 | 30% |
| Epic | $150 | 15% |
| Legendary | $300 | 5% |

### Streak Bonuses
| Streak | Bonus |
|--------|-------|
| 10 | $25 |
| 25 | $75 |
| 50 | $250 |
| 100 | $1000 |

---

## 🎲 Drop Rates

| Rarity | Chance |
|--------|--------|
| Gold | 2.6% |
| Red | 2.5% |
| Pink | 2.5% |
| Purple | 5% |
| Blue | 87.4% |

### Condition Multipliers
| Condition | Chance | Multiplier |
|-----------|--------|------------|
| Factory New | 5% | 2.0x |
| Minimal Wear | 15% | 1.5x |
| Field-Tested | 50% | 1.0x |
| Well-Worn | 20% | 0.75x |
| Battle-Scarred | 10% | 0.5x |

---

## ⏲️ Timer Setup

Timers are created directly from an action's Triggers tab:

1. Open the action (e.g., `ResetDailyStats_Command`)
2. Go to **Triggers** tab
3. Right-click → **Add Trigger** → **Core** → **Timed Actions**
4. Click **Create Timer**
5. Configure and click **OK**

| Timer Name | Interval | Action to Run |
|------------|----------|---------------|
| `DailyStatsReset` | 86400 | `ResetDailyStats_Command` |
| `WeeklyStatsReset` | 604800 | `ResetWeeklyStats_Command` |
| `MonthlyStatsReset` | 2629746 | `ResetMonthlyStats_Command` |
| `JackpotAutoDraw` | 60 | `Jackpot_Draw` |
| `GiveawayAutoDraw` | 120 | `DrawGiveaway_Command` |

---

## 📁 Files Included

| File | Description |
|------|-------------|
| `CS2-Case-Bot-Logo.png` | Repository logo |
| `README.md` | Main documentation |
| `LICENSE` | MIT License + IP Disclaimer |
| `setup-guide.md` | Step-by-step installation |
| `persisted-globals.txt` | Skin lists for copy/paste |
| `V1.6-Complete-Code-Reference.md` | All action C# code |
| `CS2-Case-Bot-V1.6-Actions.sb` | One-click action import |

---

## 🤝 Support the Project

[![Ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/mk4gtiguy)

---

## 📝 License

MIT License + IP Disclaimer — see [LICENSE](LICENSE) file.

---

## 🙏 Credits

- Created by **Mk4gtiguy**
- Skin names from Counter-Strike 2 © Valve Corporation
- Built for [Streamer.bot](https://streamer.bot)

---

## 📌 Version History

| Version | Changes |
|---------|---------|
| **V1.6** | CS20 case, full sticker system, gold tiers, expanded trade-up, daily jackpot, quests, lottery, referrals, giveaways, auto-draw, collections, stats dashboard, 45+ commands |
| V1.4 | 11 cases, basic economy, trading, jackpot |
| V1.3 | Case switching, costs, bug fixes |
| V1.2 | Initial release |
