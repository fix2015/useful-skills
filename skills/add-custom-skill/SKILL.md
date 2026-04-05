---
name: add-custom-skill
description: >
  Step-by-step guide for contributing a new skill to the useful-skills registry
  and publishing it through load-skill on npm.
tags:
  - contributing
  - skills
  - registry
  - workflow
compatible:
  - claude-code
  - cursor
  - codex
  - gemini-cli
---

# Add Custom Skill — Contributor Guide

Step-by-step guide for adding a new skill to the [useful-skills](https://github.com/fix2015/useful-skills) registry and making it available via `npx load-skill install <name>`.

Use when: user wants to create a new skill, contribute a skill to the registry, or publish a skill for the community.

---

## Overview

Adding a skill is a 4-step process:

1. **Create the skill file** in the `useful-skills` repo
2. **Update the useful-skills README** with the new entry
3. **Register the skill** in the `load-skill` registry
4. **Publish** — push both repos and publish to npm

---

## Step 1: Create the Skill File

### Clone the repo
```bash
git clone https://github.com/fix2015/useful-skills.git
cd useful-skills
```

### Create the skill directory and file
```bash
mkdir -p skills/<skill-name>
```

Create `skills/<skill-name>/SKILL.md` with this structure:

```markdown
---
name: <skill-name>
description: >
  One-line description of what this skill does.
tags:
  - tag1
  - tag2
compatible:
  - claude-code
  - cursor
  - codex
  - gemini-cli
---

# Skill Title

One paragraph explaining what this skill does and when to use it.

Use when: list of trigger phrases or scenarios.

---

## Section 1 — Core Concept

Content here...

## Section 2 — Code Patterns

Code examples...

## Common Gotchas

Pitfalls and solutions...

## Testing Checklist

- [ ] Verify X works
- [ ] Verify Y works
```

### YAML Frontmatter Rules
- **name** — lowercase, hyphenated (e.g. `google-calendar-sync`)
- **description** — use `>` block scalar if the description contains colons or special characters
- **tags** — lowercase, relevant keywords for search
- **compatible** — list of supported tools (`claude-code`, `cursor`, `codex`, `gemini-cli`)

### Skill Content Guidelines
- Start with a clear one-liner and "Use when" triggers
- Include architecture overview for complex integrations
- Provide copy-paste code patterns (not full implementations)
- Document common gotchas — these are the most valuable part
- End with a testing checklist
- Keep it practical — real patterns over theory

---

## Step 2: Update the useful-skills README

Add a row to the "Available Skills" table in `README.md`:

```markdown
| [<skill-name>](skills/<skill-name>/SKILL.md) | Short description | tag1, tag2 |
```

### Push to GitHub
```bash
git add -A
git commit -m "Add <skill-name> skill"
git push origin main
```

---

## Step 3: Register in load-skill

### Clone load-skill
```bash
git clone https://github.com/fix2015/load-skill.git
cd load-skill
```

### Add to the registry JSON
Add a new entry at the end of the `skills` array in `data/skills-registry.json`:

```json
{
  "name": "<skill-name>",
  "description": "Short description",
  "tags": ["tag1", "tag2"],
  "source": "useful-skills",
  "compatible": ["claude-code", "cursor", "codex", "gemini-cli"],
  "raw_url": "https://raw.githubusercontent.com/fix2015/useful-skills/main/skills/<skill-name>/SKILL.md",
  "repo_url": "https://github.com/fix2015/useful-skills/tree/main/skills/<skill-name>"
}
```

### Update the README
Update the skill count in `README.md`:
- Update the number in `**X skills** from official and community sources`
- Update the Useful Skills row count in the Skill Sources table

### Bump version
In `package.json`, bump the patch version (e.g. `1.3.1` → `1.3.2`).

### Test, commit, push, and publish
```bash
npm test
git add -A
git commit -m "Add <skill-name> skill, bump to vX.Y.Z"
git push origin main
npm publish
```

---

## Step 4: Verify

```bash
# Check the skill is in the registry
npx load-skill info <skill-name>

# Install it
npx load-skill install <skill-name>

# Verify the file was created
cat .claude/skills/<skill-name>/SKILL.md
```

---

## Quick Reference

| What | Where |
|------|-------|
| Skill file | `useful-skills/skills/<name>/SKILL.md` |
| useful-skills README | `useful-skills/README.md` |
| Registry JSON | `load-skill/data/skills-registry.json` |
| load-skill README | `load-skill/README.md` |
| Version | `load-skill/package.json` |

---

## Common Mistakes

- **YAML parsing error** — description contains colons or quotes. Use `>` block scalar for the description field.
- **Skill not found after publish** — forgot to add entry to `skills-registry.json`
- **404 on install** — `raw_url` path doesn't match the actual file path in the repo
- **Tests fail** — every skill entry in the registry must have `name`, `description`, `tags`, `source`, `compatible`, `raw_url`, `repo_url`
