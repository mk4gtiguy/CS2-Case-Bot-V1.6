**IMPORTAN UPDATE**
**TOOK SLEEP FROM CODE FOR RESPONSE**

Updated Core Actions
ACTION: CS2_OpenCase (V1.6 - No Sleep)
csharp
using System;
using System.Collections.Generic;

public class CPHInline
{
    public bool Execute()
    {
        var random = new Random();
        
        string user = CPH.GetGlobalVar<string>("User");
        string currentCase = CPH.GetGlobalVar<string>("CurrentCase");
        string casePrefix = currentCase.Replace(" ", "").Replace("&", "").Replace("-", "");
        
        // Update stats tracking
        int dailyOpens = CPH.GetGlobalVar<int>("Stats_DailyOpens");
        dailyOpens++;
        CPH.SetGlobalVar("Stats_DailyOpens", dailyOpens, true);
        
        int weeklyOpens = CPH.GetGlobalVar<int>("Stats_WeeklyOpens");
        weeklyOpens++;
        CPH.SetGlobalVar("Stats_WeeklyOpens", weeklyOpens, true);
        
        int monthlyOpens = CPH.GetGlobalVar<int>("Stats_MonthlyOpens");
        monthlyOpens++;
        CPH.SetGlobalVar("Stats_MonthlyOpens", monthlyOpens, true);
        
        // Update quest progress
        string quests = CPH.GetGlobalVar<string>(user + "_Quests");
        if (!string.IsNullOrEmpty(quests))
        {
            string[] parts = quests.Split(';');
            for (int i = 0; i < parts.Length; i++)
            {
                if (parts[i].StartsWith("Daily_3cases:"))
                {
                    int val = int.Parse(parts[i].Substring(13)) + 1;
                    parts[i] = "Daily_3cases:" + val;
                }
                if (parts[i].StartsWith("Daily_specific:"))
                {
                    string dailySpecificCase = CPH.GetGlobalVar<string>(user + "_DailySpecificCase");
                    if (currentCase == dailySpecificCase)
                    {
                        int val = int.Parse(parts[i].Substring(15)) + 1;
                        parts[i] = "Daily_specific:" + val;
                    }
                }
            }
            quests = string.Join(";", parts);
            CPH.SetGlobalVar(user + "_Quests", quests, true);
        }
        
        // Streak tracking
        int currentStreak = CPH.GetGlobalVar<int>(user + "_OpenStreak");
        currentStreak++;
        CPH.SetGlobalVar(user + "_OpenStreak", currentStreak, true);
        
        // Streak bonuses
        if (currentStreak == 10 || currentStreak == 25 || currentStreak == 50 || currentStreak == 100)
        {
            double bonus = 0;
            if (currentStreak == 10) bonus = 25.00;
            else if (currentStreak == 25) bonus = 75.00;
            else if (currentStreak == 50) bonus = 250.00;
            else if (currentStreak == 100) bonus = 1000.00;
            
            double balance = CPH.GetGlobalVar<double>(user + "_Balance");
            balance += bonus;
            CPH.SetGlobalVar(user + "_Balance", balance, true);
            CPH.SendMessage("🔥 " + user + " reached a " + currentStreak + " open streak! Bonus $" + bonus.ToString("F2") + " added!");
        }
        
        // Roll rarity
        int roll = random.Next(1, 1001);
        string rarityType;
        if (roll <= 26) rarityType = "Gold";
        else if (roll >= 975) rarityType = "Red";
        else if (roll >= 950) rarityType = "Pink";
        else if (roll >= 900) rarityType = "Purple";
        else rarityType = "Blue";
        
        // Base values
        string emoji;
        double baseValue;
        if (rarityType == "Gold") { emoji = "🟡"; baseValue = 150.00; }
        else if (rarityType == "Red") { emoji = "🔴"; baseValue = 20.00; }
        else if (rarityType == "Pink") { emoji = "💗"; baseValue = 4.00; }
        else if (rarityType == "Purple") { emoji = "🟣"; baseValue = 1.00; }
        else { emoji = "🔷"; baseValue = 0.25; }
        
        // Get skin list
        string skinList = CPH.GetGlobalVar<string>(casePrefix + rarityType + "List");
        if (string.IsNullOrEmpty(skinList)) skinList = CPH.GetGlobalVar<string>(rarityType + "List");
        
        var validSkins = new List<string>();
        foreach (string skin in skinList.Split(';'))
            if (!string.IsNullOrWhiteSpace(skin))
                validSkins.Add(skin.Trim());
        
        string currentSkin = validSkins[random.Next(0, validSkins.Count)];
        
        // Apply condition
        string[] conditions = { "Factory New", "Minimal Wear", "Field-Tested", "Well-Worn", "Battle-Scarred" };
        double[] conditionMultipliers = { 2.0, 1.5, 1.0, 0.75, 0.5 };
        int conditionRoll = random.Next(1, 101);
        int conditionIndex = 0;
        if (conditionRoll <= 5) conditionIndex = 0;
        else if (conditionRoll <= 20) conditionIndex = 1;
        else if (conditionRoll <= 70) conditionIndex = 2;
        else if (conditionRoll <= 90) conditionIndex = 3;
        else conditionIndex = 4;
        
        string condition = conditions[conditionIndex];
        double finalValue = baseValue * conditionMultipliers[conditionIndex];
        
        // Apply StatTrak
        bool isStatTrak = random.Next(1, 101) <= 10;
        if (isStatTrak) finalValue *= 2;
        
        // Apply Gold tier
        string goldTierTag = "";
        if (rarityType == "Gold")
        {
            int goldTierRoll = random.Next(1, 1001);
            if (goldTierRoll <= 500) { finalValue = 150.00; goldTierTag = " [Common]"; }
            else if (goldTierRoll <= 800) { finalValue = 300.00; goldTierTag = " [Rare]"; }
            else if (goldTierRoll <= 950) { finalValue = 600.00; goldTierTag = " [Epic]"; }
            else if (goldTierRoll <= 995) { finalValue = 1000.00; goldTierTag = " [Legendary]"; }
            else { finalValue = 2500.00; goldTierTag = " [Mythic]"; }
        }
        
        string statTrakTag = isStatTrak ? " (StatTrak™)" : "";
        string newItem = emoji + " " + currentSkin + " (" + condition + ")" + goldTierTag + statTrakTag;
        
        // Save to inventory
        string oldItems = CPH.GetGlobalVar<string>(user + "_Items");
        string newItemsList = string.IsNullOrEmpty(oldItems) ? newItem : oldItems + ";" + newItem;
        CPH.SetGlobalVar(user + "_Items", newItemsList, true);
        
        // Track case opens
        string caseKey = casePrefix + "_Opened";
        int caseOpened = CPH.GetGlobalVar<int>(caseKey);
        CPH.SetGlobalVar(caseKey, caseOpened + 1, true);
        
        int userCaseOpened = CPH.GetGlobalVar<int>(user + "_" + casePrefix + "_Opened");
        CPH.SetGlobalVar(user + "_" + casePrefix + "_Opened", userCaseOpened + 1, true);
        
        int totalOpened = CPH.GetGlobalVar<int>(user + "_TotalOpened");
        CPH.SetGlobalVar(user + "_TotalOpened", totalOpened + 1, true);
        
        // Case unlocks
        string unlockedCases = CPH.GetGlobalVar<string>(user + "_UnlockedCases");
        if (string.IsNullOrEmpty(unlockedCases)) unlockedCases = "Dreams;Recoil";
        
        int totalCasesOpened = totalOpened + 1;
        if (totalCasesOpened >= 25 && !unlockedCases.Contains("Kilowatt"))
        { unlockedCases += ";Kilowatt"; CPH.SendMessage("🔓 " + user + " unlocked the Kilowatt case!"); }
        if (totalCasesOpened >= 50 && !unlockedCases.Contains("Revolution"))
        { unlockedCases += ";Revolution"; CPH.SendMessage("🔓 " + user + " unlocked the Revolution case!"); }
        if (totalCasesOpened >= 75 && !unlockedCases.Contains("Fracture"))
        { unlockedCases += ";Fracture"; CPH.SendMessage("🔓 " + user + " unlocked the Fracture case!"); }
        if (totalCasesOpened >= 100 && !unlockedCases.Contains("Clutch"))
        { unlockedCases += ";Clutch"; CPH.SendMessage("🔓 " + user + " unlocked the Clutch case!"); }
        if (totalCasesOpened >= 150 && !unlockedCases.Contains("Prisma"))
        { unlockedCases += ";Prisma"; CPH.SendMessage("🔓 " + user + " unlocked the Prisma case!"); }
        if (totalCasesOpened >= 200 && !unlockedCases.Contains("DangerZone"))
        { unlockedCases += ";DangerZone"; CPH.SendMessage("🔓 " + user + " unlocked the Danger Zone case!"); }
        CPH.SetGlobalVar(user + "_UnlockedCases", unlockedCases, true);
        
        // Track rarity counts
        string rarityCounterKey = user + "_" + rarityType + "s";
        int rarityCount = CPH.GetGlobalVar<int>(rarityCounterKey);
        CPH.SetGlobalVar(rarityCounterKey, rarityCount + 1, true);
        
        // Achievements
        string achievements = CPH.GetGlobalVar<string>(user + "_Achievements");
        if (string.IsNullOrEmpty(achievements)) achievements = "";
        
        if (rarityType == "Gold" && !achievements.Contains("FirstGold"))
        { achievements += (achievements == "" ? "" : ";") + "FirstGold"; CPH.SendMessage("🏆 " + user + " earned 'First Gold'!"); }
        
        int tradeupCount = CPH.GetGlobalVar<int>(user + "_TradeupCount");
        if (tradeupCount >= 10 && !achievements.Contains("TradeUpMaster"))
        { achievements += (achievements == "" ? "" : ";") + "TradeUpMaster"; CPH.SendMessage("🏆 " + user + " earned 'Trade Up Master'!"); }
        
        int jackpotWins = CPH.GetGlobalVar<int>(user + "_JackpotWins");
        if (jackpotWins >= 1 && !achievements.Contains("JackpotWinner"))
        { achievements += (achievements == "" ? "" : ";") + "JackpotWinner"; CPH.SendMessage("🏆 " + user + " earned 'Jackpot Winner'!"); }
        
        CPH.SetGlobalVar(user + "_Achievements", achievements, true);
        
        // Gold tracking
        if (rarityType == "Gold")
        {
            int userGolds = CPH.GetGlobalVar<int>(user + "_Golds");
            CPH.SetGlobalVar(user + "_Golds", userGolds + 1, true);
            int totalGolds = CPH.GetGlobalVar<int>("GoldPulls_Total");
            CPH.SetGlobalVar("GoldPulls_Total", totalGolds + 1, true);
        }
        
        // Track all users
        string allUsers = CPH.GetGlobalVar<string>("AllUsers");
        if (string.IsNullOrEmpty(allUsers)) allUsers = user;
        else
        {
            bool userExists = false;
            foreach (string existingUser in allUsers.Split('§'))
                if (existingUser.Equals(user, StringComparison.OrdinalIgnoreCase))
                { userExists = true; break; }
            if (!userExists) allUsers += "§" + user;
        }
        CPH.SetGlobalVar("AllUsers", allUsers, true);
        
        CPH.SetGlobalVar("CurrentSkin", currentSkin, true);
        CPH.SetGlobalVar("RarityType", rarityType, true);
        CPH.SetGlobalVar("Emoji", emoji, true);
        CPH.SetGlobalVar("Value", finalValue.ToString("F2"), true);
        CPH.SetGlobalVar("CurrentValue", finalValue, true);
        
        CPH.SendMessage("🎁 " + user + " opened " + currentCase + " and got: " + newItem + " (Value: $" + finalValue.ToString("F2") + ")");
        return true;
    }
}
ACTION: CS2_OpenCapsule (V1.6 - No Sleep)
csharp
using System;

public class CPHInline
{
    public bool Execute()
    {
        var random = new Random();
        
        string user = CPH.GetGlobalVar<string>("User");
        string currentCapsule = CPH.GetGlobalVar<string>("CurrentCapsule");
        
        int roll = random.Next(1, 1001);
        
        string rarity;
        string emoji;
        double value;
        
        if (roll <= 26) { rarity = "Gold"; emoji = "👑"; value = 30.00; }
        else if (roll >= 975) { rarity = "Red"; emoji = "🔥"; value = 10.00; }
        else if (roll >= 950) { rarity = "Pink"; emoji = "💫"; value = 2.00; }
        else if (roll >= 900) { rarity = "Purple"; emoji = "✨"; value = 0.50; }
        else { rarity = "Blue"; emoji = "⭐"; value = 0.10; }
        
        // Apply Gold tier
        string goldTierTag = "";
        if (rarity == "Gold")
        {
            int goldTierRoll = random.Next(1, 1001);
            if (goldTierRoll <= 500) { value = 30.00; goldTierTag = " [Common]"; }
            else if (goldTierRoll <= 800) { value = 75.00; goldTierTag = " [Rare]"; }
            else if (goldTierRoll <= 950) { value = 150.00; goldTierTag = " [Epic]"; }
            else { value = 300.00; goldTierTag = " [Legendary]"; }
        }
        
        string stickerListKey = currentCapsule + "Sticker" + rarity + "List";
        string stickerList = CPH.GetGlobalVar<string>(stickerListKey);
        
        if (string.IsNullOrEmpty(stickerList))
        {
            CPH.SendMessage(user + " error: no sticker list found for " + currentCapsule + " " + rarity + "!");
            return false;
        }
        
        string[] stickers = stickerList.Split(';');
        string sticker = stickers[random.Next(0, stickers.Length)].Trim();
        
        string newSticker = emoji + " " + sticker + goldTierTag;
        
        string oldItems = CPH.GetGlobalVar<string>(user + "_Items");
        string newItemsList = string.IsNullOrEmpty(oldItems) ? newSticker : oldItems + ";" + newSticker;
        CPH.SetGlobalVar(user + "_Items", newItemsList, true);
        
        CPH.SendMessage("📯 " + user + " opened a " + currentCapsule + " capsule and got: " + newSticker + " (Value: $" + value.ToString("F2") + ")");
        return true;
    }
}
What Changed
Action	Removed
CS2_OpenCase	System.Threading.Thread.Sleep(2500);
CS2_OpenCapsule	System.Threading.Thread.Sleep(1000);


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
