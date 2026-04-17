---
name: generate-video
description: >
  Generate TikTok-style videos with voice, captions, lip-synced avatar, preview
  intro, code overlays, and built-in script bank. Free TTS, no API key.
tags:
  - video
  - tiktok
  - tts
  - captions
  - avatar
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

Generate TikTok-style vertical videos with voiceover, animated captions, lip-synced avatar, preview intro, and code overlays using `generate-video`.

Use when: user asks to create a video from text, generate TikTok/Shorts content, add voiceover to video, create caption videos, add avatar to video, or automate video content creation.

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

### With all features

```bash
npx generate-video "Your text" \
  --title "My Video" \
  --code "const x = 42;" \
  --avatar \
  --preview \
  --logo ./logo.png
```

---

## Avatar (lip-sync)

Generates a cartoon avatar with 4 mouth positions (closed, small, medium, wide) synced to audio amplitude.

```bash
npx generate-video "Your text" --avatar
npx generate-video --topic 0 --avatar
```

---

## Preview Intro Frame

Adds a branded 1.5-second intro frame before the main content. Audio is automatically delayed to sync.

```bash
npx generate-video "Your text" --title "My Video" --preview
npx generate-video "Your text" --preview --preview-bg ./background.png
npx generate-video "Your text" --preview --preview-duration 2.0
```

---

## Built-in Script Bank

12 pre-written topics (RAG, JavaScript, etc.) with title, hook, script text, code overlay, and hashtags.

```bash
# List all topics
npx generate-video --topics

# Generate from a topic
npx generate-video --topic 0
npx generate-video --topic 5 --avatar --preview
```

---

## Title and Code Overlays

```bash
npx generate-video "Your text" --title "Closures 101"
npx generate-video "Your text" --code "function outer() {\n  let x = 10;\n  return () => x;\n}"
npx generate-video "Your text" --title "My Title" --code "const x = 42;"
```

---

## Voice Options

```bash
# Choose a voice
npx generate-video "Bonjour" --voice fr-FR-HenriNeural

# Adjust speed and pitch
npx generate-video "Fast speech" --rate "+30%"
npx generate-video "Deep voice" --pitch "-5Hz"

# Browse voices
npx generate-video --voices --lang en
```

---

## Custom Look

```bash
# Custom colors
npx generate-video "Your text" --bg-color 1a1a2e --accent-color e94560

# Logo
npx generate-video "Your text" --logo ./my-logo.png

# Landscape (YouTube)
npx generate-video "Your text" --width 1920 --height 1080

# Square (Instagram)
npx generate-video "Your text" --width 1080 --height 1080
```

---

## Other Options

```bash
# Use existing audio instead of TTS
npx generate-video "Caption text" --audio ./voiceover.mp3

# Disable captions
npx generate-video "Your text" --no-captions

# Preview without generating
npx generate-video "Your text" --dry-run

# Custom output path
npx generate-video "Your text" --output my-video.mp4
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
| `--avatar` | Enable lip-synced avatar | off |
| `--preview` | Add preview intro frame | off |
| `--preview-bg <path>` | Preview background image | — |
| `--preview-duration <s>` | Preview duration | `1.5` |
| `--topic <index>` | Use built-in topic | — |
| `--topics` | List built-in topics | — |
| `--width <px>` | Video width | `720` |
| `--height <px>` | Video height | `1280` |
| `--fps <n>` | Frames per second | `30` |
| `--bg-color <hex>` | Background color | `0f172a` |
| `--accent-color <hex>` | Accent color | `7c3aed` |
| `--no-captions` | Disable captions | — |
| `--voices` | List TTS voices | — |
| `-l, --lang <code>` | Filter voices | — |
| `--dry-run` | Preview only | — |

---

## How It Works

1. **edge-tts** generates voice audio with word-level timestamps (free)
2. **Pillow** renders background frame with title, code box, logo
3. **Pillow** renders animated caption frames synced to word timings
4. **Pillow** generates avatar with 4 mouth states (closed, small, medium, wide)
5. **FFmpeg** analyzes audio amplitude for lip-sync
6. **FFmpeg** composites frames + audio into final video
