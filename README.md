# 🧠 Second Brain

Personal knowledge vault for Shivansh. Built on Obsidian.

## Start here
1. Open Obsidian
2. Click **"Open folder as vault"**
3. Pick `~/Documents/SecondBrain`
4. Open `Home.md` — your map of content

## Vault structure
```
00-Inbox       → unsorted captures
01-Notes       → atomic, evergreen ideas
02-Projects    → active work with a finish line
03-Areas       → ongoing responsibilities
04-Resources   → reference material
05-Archive     → done / inactive
Daily          → daily notes
Weekly         → weekly reviews
Templates      → note templates
Attachments    → images, PDFs
```

## What's pre-configured
- ✅ Daily notes (Templates/Daily, folder: Daily)
- ✅ Templates folder
- ✅ Wikilinks, line numbers, spellcheck
- ✅ New notes default to `00-Inbox`
- ✅ Attachments default to `Attachments`
- ✅ Bookmarks: Home, How-to guide, Inbox, Daily
- ✅ Hotkeys set (see below)
- ✅ Workspace layout (Home open, sidebars configured)
- ✅ Starter atomic notes + MOC

## You still need to (5 minutes)
1. Open Obsidian → Settings → **Community plugins** → "Turn on community plugins"
2. **Browse** and install:
   - Dataview
   - Templater
   - Calendar
   - Omnisearch
   - Excalidraw
   - Advanced Tables
   - Periodic Notes
   - Tasks
3. (Optional) Settings → Appearance → pick a theme like **Minimal** or **Things**

## Hotkeys
| Action | Key |
|--------|-----|
| Open today's daily note | `Cmd+Shift+D` |
| Command palette | `Cmd+P` |
| Quick switcher | `Cmd+O` |
| Global search | `Cmd+Shift+F` |
| Graph view | `Cmd+Shift+G` |
| New note (→ Inbox) | `Cmd+N` |
| Insert template | `Cmd+Shift+T` |
| Random note | `Cmd+Shift+R` |
| Toggle preview/edit | `Cmd+E` |

## The workflow
- **Daily:** Capture in today's note. Process to atomic notes at end of day if anything is worth keeping.
- **Weekly:** Use `Templates/Weekly-Review` to empty inbox, review projects, set next week's top 3.
- **Always:** One idea per note. Every note links to 2+ others. Title notes as claims, not topics.

## Sync (pick one)
- **Obsidian Sync** ($4/mo, recommended) — Settings → Sync
- **iCloud Drive** — move the `SecondBrain` folder to `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/`
- **Git** — `cd ~/Documents/SecondBrain && git init && git add . && git commit -m "init"` + push to private repo
