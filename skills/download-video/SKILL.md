---
name: download-video
description: >
  Download videos from YouTube, Instagram, TikTok, Facebook, Twitter, and 1000+
  sites using the download-video CLI. Free, powered by yt-dlp.
tags:
  - download
  - video
  - youtube
  - instagram
  - tiktok
  - cli
compatible:
  - claude-code
  - cursor
  - codex
  - gemini-cli
---

# Download Video — Skill

Download videos from YouTube, Instagram, TikTok, Facebook, Twitter, and 1000+ sites using `download-video`.

Use when: user asks to download a video, save a video from a URL, extract audio from a video, download a YouTube/Instagram/TikTok video, or get video metadata.

---

## Quick Start

```bash
npx get-video "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
```

---

## Prerequisites

- Node.js >= 14
- Python 3 with pip (yt-dlp auto-installs)

---

## Download from Any Platform

```bash
# YouTube
npx get-video "https://www.youtube.com/watch?v=..."

# Instagram
npx get-video "https://www.instagram.com/reel/ABC123/"

# TikTok
npx get-video "https://www.tiktok.com/@user/video/123456"

# Facebook
npx get-video "https://www.facebook.com/watch?v=123456"

# Twitter / X
npx get-video "https://x.com/user/status/123456"

# Telegram
npx get-video "https://t.me/channel/123"

# Reddit
npx get-video "https://reddit.com/r/sub/comments/abc123/title/"
```

---

## Common Options

```bash
# Choose quality
npx get-video "URL" --quality 720

# Audio only (MP3)
npx get-video "URL" --audio-only

# Save to folder
npx get-video "URL" --output ./downloads/

# Download playlist
npx get-video "URL" --playlist

# With subtitles
npx get-video "URL" --subs --subs-lang en

# List formats
npx get-video "URL" --formats

# Show metadata
npx get-video "URL" --metadata

# Download thumbnail
npx get-video "URL" --thumbnail

# Limit speed
npx get-video "URL" --limit-rate 1M
```

---

## Options Reference

| Flag | Description | Default |
|------|-------------|---------|
| `-q, --quality <res>` | Max quality: 360, 480, 720, 1080, best | `best` |
| `-o, --output <path>` | Output directory | `.` |
| `-a, --audio-only` | Download audio as MP3 | — |
| `-f, --formats` | List available formats | — |
| `-s, --subs` | Download subtitles | — |
| `--subs-lang <lang>` | Subtitle language | `en` |
| `--playlist` | Download entire playlist | — |
| `--thumbnail` | Download thumbnail | — |
| `--metadata` | Show info without downloading | — |
| `--limit-rate <rate>` | Limit speed (1M, 500K) | — |
| `--proxy <url>` | Use proxy | — |
| `--cookies <file>` | Cookies for private videos | — |

---

## Supported Sites

YouTube, Instagram, TikTok, Facebook, Twitter/X, Telegram, Reddit, Vimeo, Twitch, Dailymotion, SoundCloud, Bandcamp, Bilibili, LinkedIn, Pinterest, and 1000+ more.

```bash
npx get-video sites
```
