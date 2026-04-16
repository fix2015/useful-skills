---
name: youtube-upload
description: >
  Bulk upload and schedule YouTube videos using the youtube-publish CLI.
  Setup Google credentials, upload folders of videos, schedule at peak times,
  filter by keyword, manage playlists.
tags:
  - youtube
  - video
  - upload
  - scheduling
  - cli
  - automation
compatible:
  - claude-code
  - cursor
  - codex
  - gemini-cli
---

# YouTube Upload — Skill

Bulk upload and schedule YouTube videos from the command line using the `youtube-publish` npm package.

Use when: user asks to upload videos to YouTube, schedule YouTube uploads, bulk publish videos, automate YouTube publishing, or set up a YouTube upload pipeline.

---

## Prerequisites

```bash
npm install -g youtube-publish
```

Also requires Python 3 with pip (the CLI installs Python deps automatically).

---

## 1. First-Time Setup

### Get Google API credentials

Run the built-in guide:

```bash
npx youtube-publish guide
```

This prints step-by-step instructions. Summary:

1. Go to https://console.cloud.google.com
2. Create a project → Enable **YouTube Data API v3**
3. Configure OAuth consent screen (External, add yourself as test user)
4. Create **OAuth 2.0 Client ID** (Desktop app)
5. Download the JSON file → `client_secret.json`

### Save credentials and authenticate

```bash
npx youtube-publish setup --client ./client_secret.json
npx youtube-publish auth
```

`setup` copies credentials to `~/.youtube-publish/` and installs Python deps.
`auth` opens a browser for one-time Google OAuth login. Token auto-refreshes after that.

---

## 2. Uploading Videos

### Upload all videos from a folder

```bash
npx youtube-publish upload --path ./videos/
```

- Scans for `.mp4` files
- Deduplicates by topic (keeps latest version per filename)
- Skips already-uploaded videos (tracked in `.yt-upload-history.json`)
- Generates title from filename, auto-generates description and tags

### Upload a single file

```bash
npx youtube-publish upload --file ./my-video.mp4
```

### Preview without uploading

```bash
npx youtube-publish upload --path ./videos/ --dry-run
```

### Filter by keyword

Only upload videos whose filename contains a keyword:

```bash
npx youtube-publish upload --path ./videos/ --filter react
npx youtube-publish upload --path ./videos/ --filter closure
```

---

## 3. Scheduling

### Schedule 1 video/day

```bash
# 1 video/day at 6PM UTC starting May 1st
npx youtube-publish upload --path ./videos/ --schedule 2026-05-01

# Every 2 days
npx youtube-publish upload --path ./videos/ --schedule 2026-05-01 --interval 2

# Custom time (2PM UTC)
npx youtube-publish upload --path ./videos/ --schedule 2026-05-01 --time 14:00
```

### Auto-schedule 3 videos/day at peak times

```bash
# 3/day at 8AM, 2PM, 6PM UTC (peak engagement times)
npx youtube-publish upload --path ./videos/ --auto

# Starting from a specific date
npx youtube-publish upload --path ./videos/ --auto --auto-from 2026-05-01
```

### Combine filter with schedule

```bash
npx youtube-publish upload --path ./videos/ --filter react --schedule 2026-05-01
```

---

## 4. Playlists

Add videos to a playlist (creates the playlist if it doesn't exist):

```bash
npx youtube-publish upload --path ./videos/ --playlist "JavaScript Tips"
npx youtube-publish upload --path ./videos/ --playlist PLxxxxxxxxxxxxxxx
```

---

## 5. Privacy and Category

```bash
# Set privacy (public, unlisted, private)
npx youtube-publish upload --path ./videos/ --privacy unlisted

# Set YouTube category
npx youtube-publish upload --path ./videos/ --category 27
```

Common category IDs: 22=People & Blogs, 24=Entertainment, 27=Education, 28=Science & Tech (default)

---

## 6. Status and Management

### Check upload status

```bash
npx youtube-publish list --path ./videos/
npx youtube-publish list --path ./videos/ --filter react
```

Shows which videos are uploaded, their YouTube URLs, and scheduled times.

### Clear history to re-upload

```bash
npx youtube-publish reset --path ./videos/
```

---

## 7. Filename Convention

Videos should follow this naming pattern for best results:

```
YYYYMMDD_HHMMSS_topic_name.mp4  →  Title: "Topic Name"
my_cool_video.mp4                →  Title: "My Cool Video"
```

The timestamp prefix is optional and stripped to derive the title.

---

## 8. File Locations

| What | Where |
|------|-------|
| Credentials | `~/.youtube-publish/client_secret.json` |
| Auth token | `~/.youtube-publish/youtube_token.json` |
| Upload history | `<videos-dir>/.yt-upload-history.json` |

---

## 9. All Options Reference

| Flag | Description | Default |
|------|-------------|---------|
| `-p, --path <dir>` | Videos directory | `./videos` |
| `-f, --file <file>` | Upload single file | — |
| `--filter <keyword>` | Only videos matching keyword | — |
| `-s, --schedule <date>` | Schedule start date (YYYY-MM-DD) | — |
| `-i, --interval <days>` | Days between scheduled uploads | `1` |
| `-t, --time <HH:MM>` | Publish time in UTC | `18:00` |
| `--auto` | Auto-schedule 3/day at peak times | — |
| `--auto-from <date>` | Start date for --auto | tomorrow |
| `--privacy <status>` | public, private, unlisted | `public` |
| `--playlist <name>` | Playlist name or ID | — |
| `--category <id>` | YouTube category ID | `28` |
| `--dry-run` | Preview without uploading | — |

---

## 10. Common Gotchas

- **"unverified app" warning** — Normal for personal use. Click Advanced → Go to app.
- **OAuth consent screen not configured** — Must add yourself as test user before auth works.
- **API not enabled** — YouTube Data API v3 must be explicitly enabled in Google Cloud Console.
- **Token expired** — Tokens auto-refresh. If broken, delete `~/.youtube-publish/youtube_token.json` and run `auth` again.
- **Duplicate uploads** — History is tracked per-folder. Use `reset` to clear and re-upload.
- **Schedule must be future** — Scheduled times in the past are skipped.
