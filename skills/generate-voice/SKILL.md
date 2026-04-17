---
name: generate-voice
description: >
  Generate voice audio from text using free Microsoft TTS via the generate-voice
  CLI. 400+ voices, 100+ languages, no API key needed.
tags:
  - tts
  - voice
  - audio
  - speech
  - cli
  - automation
compatible:
  - claude-code
  - cursor
  - codex
  - gemini-cli
---

# Generate Voice — Skill

Generate voice audio from text using the `generate-voice` npm package. Free Microsoft TTS, 400+ voices, 100+ languages, no API key.

Use when: user asks to generate speech, convert text to audio, create voiceover, generate TTS, or needs voice audio files.

---

## Quick Start

```bash
npx generate-voice "Hello, how are you?"
```

Outputs an `.mp3` file. No setup, no API key, no account.

---

## Prerequisites

- Node.js >= 14
- Python 3 with pip (edge-tts auto-installs on first run)

Optional setup check:
```bash
npx generate-voice setup
```

---

## Usage

### Generate speech

```bash
# Default voice (en-US-GuyNeural)
npx generate-voice "Hello world"

# Choose a voice
npx generate-voice "Bonjour le monde" --voice fr-FR-HenriNeural

# Save to specific file
npx generate-voice "Hello" --output greeting.mp3

# Adjust speed
npx generate-voice "Fast speech" --rate "+30%"
npx generate-voice "Slow speech" --rate "-20%"

# Adjust pitch
npx generate-voice "Higher voice" --pitch "+5Hz"

# Combine options
npx generate-voice "Custom" --voice en-GB-SoniaNeural --rate "+10%" --output custom.mp3
```

### Browse voices

```bash
# All 400+ voices
npx generate-voice --voices

# Filter by language
npx generate-voice --voices --lang en
npx generate-voice --voices --lang fr
npx generate-voice --voices --lang ja
```

---

## Options Reference

| Flag | Description | Default |
|------|-------------|---------|
| `-v, --voice <name>` | TTS voice name | `en-US-GuyNeural` |
| `-o, --output <file>` | Output file path | Auto from text |
| `-r, --rate <rate>` | Speech rate (`+20%`, `-10%`) | Normal |
| `-p, --pitch <pitch>` | Voice pitch (`+5Hz`, `-2Hz`) | Normal |
| `--voices` | List available voices | — |
| `-l, --lang <code>` | Filter voice list by language | — |

---

## Popular Voices

| Voice | Language | Gender |
|-------|----------|--------|
| `en-US-GuyNeural` | English (US) | Male |
| `en-US-JennyNeural` | English (US) | Female |
| `en-GB-SoniaNeural` | English (UK) | Female |
| `fr-FR-HenriNeural` | French | Male |
| `de-DE-ConradNeural` | German | Male |
| `es-ES-AlvaroNeural` | Spanish | Male |
| `ja-JP-KeitaNeural` | Japanese | Male |
| `zh-CN-XiaoxiaoNeural` | Chinese | Female |

---

## How It Works

Uses [edge-tts](https://github.com/rany2/edge-tts), a Python library that accesses Microsoft Edge's free text-to-speech API. No API key, no account, no rate limits.
