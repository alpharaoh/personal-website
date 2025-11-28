---
title: 'Among Us Discord Bot'
description: 'Discord bot that auto-mutes/unmutes players based on Among Us game state using OCR'
pubDate: 'Sep 15 2020'
github: 'https://github.com/alpharaoh/AmongUsBot'
---

A Discord bot for Among Us that automatically mutes and unmutes players based on game events, eliminating the need for manual muting during gameplay.

## System Architecture

```
+-------------------+      +-------------------+      +-------------------+
|                   |      |                   |      |                   |
|   Among Us Game   | ---> |   Screen Capture  | ---> |   OCR Engine      |
|   (Host Screen)   |      |   + Detection     |      |   (Pytesseract)   |
|                   |      |                   |      |                   |
+-------------------+      +-------------------+      +--------+----------+
                                                               |
                                                               v
+-------------------+      +-------------------+      +-------------------+
|                   |      |                   |      |                   |
|   Discord Voice   | <--- |   Discord.py Bot  | <--- |   Game State      |
|   Channel         |      |   (Mute/Unmute)   |      |   Parser          |
|                   |      |                   |      |                   |
+-------------------+      +-------------------+      +-------------------+
```

## Game State Flow

```
+-------------+     +-------------+     +-------------+
|   LOBBY     | --> |   TASKS     | --> |  DISCUSSION |
| (Unmuted)   |     |  (Muted)    |     |  (Unmuted)  |
+-------------+     +------+------+     +------+------+
      ^                    |                   |
      |                    v                   v
      |            +-------------+     +-------------+
      +----------- |  GAME OVER  | <-- |   VOTING    |
                   |  (Unmuted)  |     |  (Unmuted)  |
                   +-------------+     +-------------+
```

## How It Works

```
Host runs bot on their machine
            |
            v
+------------------------+
| Screen capture loop    |
| (continuous)           |
+------------------------+
            |
            v
+------------------------+
| OCR reads game text    |
| "EMERGENCY MEETING"    |
| "VICTORY" / "DEFEAT"   |
| "SHHHHHH" (tasks)      |
+------------------------+
            |
            v
+------------------------+
| State change detected? |
+----------+-------------+
           |
    +------+------+
    |             |
    v             v
  [YES]         [NO]
    |             |
    v             |
+---------------+ |
| Send Discord  | |
| mute/unmute   | |
| commands      | |
+---------------+ |
    |             |
    +------+------+
           |
           v
    [Continue loop]
```

## Commands

```
.dead          - Report yourself as dead (ghost mode)
.kill @player  - Mark a player as dead
.start         - Begin automatic detection
.stop          - Stop the bot
```

## Tech Stack

- **Language:** Python 3
- **Discord:** Discord.py
- **OCR:** Pytesseract
- **Networking:** Flask

## Note

This project is discontinued (September 2020). For an actively maintained alternative, check out [amongusdiscord](https://github.com/denverquane/amongusdiscord).

**Stats:** 110 stars, 22 forks
