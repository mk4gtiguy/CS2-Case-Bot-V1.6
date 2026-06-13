# CS2 Case Bot V1.6 - Detailed Setup Guide

**Created by Mk4gtiguy**

## Prerequisites

- [Streamer.bot](https://streamer.bot) v1.0.4 or higher
- Your streaming platform connected (Twitch, YouTube, or Trovo)

---

## Step 1: Add Persisted Globals (Skin Lists)

1. Open Streamer.bot → **Services** → **Global Variables** → **Persisted Globals**
2. Open `persisted-globals.txt` from this repo
3. Copy each line (one at a time) and click **Add New Variable**
4. Paste the name and value
5. Repeat for all 60+ skin lists

---

## Step 2: Import Actions

1. Download `CS2-Case-Bot-V1.6-Actions.sb`
2. In Streamer.bot → **Actions** → Right-click → **Import** → Select the file
3. All actions and triggers will be imported automatically

---

## Step 3: Set Permissions

| Action | Permission |
|--------|------------|
| `Jackpot_Draw` | Moderator |
| `Giveaway_Command` | Broadcaster |
| All admin commands (`!givebalance`, etc.) | Moderator |

---

## Step 4: Set Up Sub-Actions (Already Set in Import)

| Command Action | Sub-Action |
|----------------|------------|
| `Case_Command` | Run Action → `CS2_OpenCase` |
| `OpenCapsule_Command` | Run Action → `CS2_OpenCapsule` |
| `Gift_Command` | Run Action → `CS2_OpenCase` |
| `GiftCapsule_Command` | Run Action → `CS2_OpenCapsule` |

---

## Step 5: Set Up Timers

Timers are created directly from an action's Triggers tab:

1. Open the action (e.g., `ResetDailyStats_Command`)
2. Go to **Triggers** tab
3. Right-click → **Add Trigger** → **Core** → **Timed Actions**
4. Click **Create Timer**
5. Configure and click **OK**

| Timer Name | Interval | Action to Run |
|------------|----------|---------------|
| `DailyStatsReset` | 86400 (24 hours) | `ResetDailyStats_Command` |
| `WeeklyStatsReset` | 604800 (7 days) | `ResetWeeklyStats_Command` |
| `MonthlyStatsReset` | 2629746 (~30 days) | `ResetMonthlyStats_Command` |
| `JackpotAutoDraw` | 60 (1 minute) | `Jackpot_Draw` |
| `GiveawayAutoDraw` | 120 (2 minutes) | `DrawGiveaway_Command` |

---

## Step 6: Test Your Bot

Type these commands in chat:

```text
!help
!setcase Dreams
!balance
!daily
!case
!inv
!setcapsule Recoil
!opencapsule
!stickers
!quests
!streak
!collections

If everything works, you are ready to go!

Troubleshooting

Stickers show double emojis – Remove emojis from your sticker globals (names should not have ⭐✨💫🔥👑)

!setcaseprice doesn't change price – It changes the price for opening, but !caseinfo shows the new price

Timers not working – Make sure Repeat and Enabled are checked when creating the timer

Jackpot auto-draw not working – Check that JackpotAutoDraw timer is enabled and set to 60 seconds

Support

If this bot helps your stream, consider supporting me on Ko-fi: https://ko-fi.com/mk4gtiguy