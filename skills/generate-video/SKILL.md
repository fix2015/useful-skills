---
name: generate-video
description: >
  Generate TikTok-style videos with voice, captions, and code overlays from text
  using the generate-video CLI. Free TTS, no API key.
tags:
  - video
  - tiktok
  - tts
  - captions
  - cli
  - automation
  - content-creator
compatible:
  - claude-code
  - cursor
  - codex
  - gemini-cli
---

# Generate Video — Skill

Generate TikTok-style vertical videos with voiceover, animated captions, title overlays, and code boxes from text using `generate-video`.

Use when: user asks to create a video from text, generate TikTok/Shorts content, add voiceover to video, create caption videos, or automate video content creation.

---

## Quick Start

```bash
npx generate-video "JavaScript closures explained in 30 seconds"
```

Outputs a 720x1280 `.mp4` with voiceover and animated captions.

---

## Prerequisites

- Node.js >= 14
- Python 3 with pip (deps auto-install)
- FFmpeg (`brew install ffmpeg` / `apt install ffmpeg`)

```bash
npx generate-video setup
```

---

## Usage

### Basic video from text

```bash
npx generate-video "Your script text here"
```

### With title and code overlay

```bash
npx generate-video "Closures capture variables" \
  --title "Closures 101" \
  --code "function outer() {\n  let x = 10;\n  return () => x;\n}"
```

### Custom voice

```bash
npx generate-video "Bonjour" --voice fr-FR-HenriNeural
```

### Adjust speed and pitch

```bash
npx generate-video "Fast speech" --rate "+30%"
npx generate-video "Deep voice" --pitch "-5Hz"
```

### With logo

```bash
npx generate-video "Your text" --logo ./logo.png
```

### Use existing audio

```bash
npx generate-video "Caption text" --audio ./voiceover.mp3
```

### Custom colors and dimensions

```bash
npx generate-video "Your text" --bg-color 1a1a2e --accent-color e94560
npx generate-video "Your text" --width 1920 --height 1080  # Landscape
npx generate-video "Your text" --width 1080 --height 1080  # Square
```

### Disable captions

```bash
npx generate-video "Your text" --no-captions
```

### Preview without generating

```bash
npx generate-video "Your text" --dry-run
```

### Browse voices

```bash
npx generate-video --voices --lang en
```

---

## Options Reference

| Flag | Description | Default |
|------|-------------|---------|
| `-v, --voice <name>` | TTS voice | `en-US-GuyNeural` |
| `-o, --output <file>` | Output path | Auto-generated |
| `-t, --title <text>` | Title overlay | — |
| `-c, --code <text>` | Code box overlay | — |
| `--logo <path>` | Logo image (PNG) | — |
| `--audio <path>` | Existing audio file | — |
| `-r, --rate <rate>` | Speech rate | Normal |
| `-p, --pitch <pitch>` | Voice pitch | Normal |
| `--width <px>` | Video width | `720` |
| `--height <px>` | Video height | `1280` |
| `--fps <n>` | Frames per second | `30` |
| `--bg-color <hex>` | Background color | `0f172a` |
| `--accent-color <hex>` | Accent color | `7c3aed` |
| `--no-captions` | Disable captions | — |
| `--dry-run` | Preview only | — |

---

## How It Works

1. **edge-tts** generates voice audio with word-level timestamps (free)
2. **Pillow** renders background frame with title, code box, logo
3. **Pillow** renders animated caption frames synced to word timings
4. **FFmpeg** composites frames + audio into final video
