# useful-skills

> A collection of practical AI coding skills for Claude Code, Cursor, Codex, Gemini CLI, and more.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## What are Skills?

Skills are markdown instruction files that give AI coding assistants domain-specific knowledge. Install them with [load-skill](https://github.com/fix2015/load-skill):

```bash
npx load-skill install telegram-bot-integration
```

## Available Skills

| Skill | Description | Tags |
|-------|-------------|------|
| [telegram-bot-integration](skills/telegram-bot-integration/SKILL.md) | Build Telegram bot integrations — webhooks, deep-link account linking, bot commands, push notifications | telegram, bot, webhooks, notifications |
| [load-registry](skills/load-registry/SKILL.md) | Discover & install skills, hooks, agents, rules, and MCP servers from public registries using the load-* CLI family | registry, skills, hooks, agents, rules, mcp |
| [google-calendar-sync](skills/google-calendar-sync/SKILL.md) | Integrate Google Calendar with web apps — OAuth2, fetch events, sync as blocked slots, auto-refresh tokens | google, calendar, oauth2, api, integration |
| [add-custom-skill](skills/add-custom-skill/SKILL.md) | Step-by-step guide for contributing a new skill to the useful-skills registry and publishing via load-skill | contributing, skills, registry, workflow |
| [youtube-upload](skills/youtube-upload/SKILL.md) | Bulk upload and schedule YouTube videos using the youtube-publish CLI | youtube, video, upload, scheduling, automation |
| [generate-voice](skills/generate-voice/SKILL.md) | Generate voice audio from text using free Microsoft TTS — 400+ voices, no API key | tts, voice, audio, speech, cli |
| [generate-video](skills/generate-video/SKILL.md) | Generate TikTok-style videos with voice, captions, and code overlays from text | video, tiktok, tts, captions, cli |
| [download-video](skills/download-video/SKILL.md) | Download videos from YouTube, Instagram, TikTok, Facebook, Twitter, and 1000+ sites | download, video, youtube, instagram, tiktok |

## Structure

```
skills/
  <skill-name>/
    SKILL.md          # The skill file (YAML frontmatter + markdown)
```

Each `SKILL.md` includes:
- **YAML frontmatter** — name, description, tags, compatible tools
- **Markdown body** — architecture, code patterns, gotchas, checklists

## Adding a Skill

1. Create `skills/<skill-name>/SKILL.md`
2. Add YAML frontmatter with `name`, `description`, `tags`, `compatible`
3. Write the skill content
4. Add entry to the table in this README
5. Submit a PR

## Compatible Tools

- [Claude Code](https://claude.ai/code)
- [Cursor](https://cursor.sh)
- [Codex](https://openai.com/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)

## Install via load-skill

```bash
# Browse all skills from this repo and others
npx load-skill list --source useful-skills

# Install any skill
npx load-skill install <skill-name>
```

## License

MIT
