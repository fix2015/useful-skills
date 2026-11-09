---
name: mac-notify
description: >
  Show live progress of long-running agent work (deploys, CI pipelines, builds,
  tests, migrations) in a floating macOS panel via the mac-notify CLI. Use
  whenever a task will take more than ~30 seconds or the user asks to watch a pipeline.
tags:
  - macos
  - notifications
  - progress
  - ci
  - github-actions
  - deploy
  - cli
compatible:
  - claude-code
  - cursor
  - codex
  - gemini-cli
---

> **Requires** [mac-notify](https://github.com/fix2015/mac-notify) (macOS 13+):
> `git clone https://github.com/fix2015/mac-notify.git && cd mac-notify && make install`


# mac-notify — progress cards for the user's desktop

The user keeps a small floating panel on their main monitor. You push compact
progress cards to it with the `mac-notify` CLI so they can glance at what you're
doing without reading your full output. Cards show: title, current step, a
progress bar, elapsed time, and source. Successful cards auto-hide after ~8s;
failed cards stay ~2 min. The panel hides itself when empty.

`mac-notify` is on PATH (`~/.local/bin/mac-notify`) and auto-starts the panel.
It never fails your command if the panel is unavailable, so don't guard calls.

## Rules

- **One card per logical task**, with a stable, unique id (`deploy-prod-booking`,
  `ci-1234567`, `migrate-users`). Re-using an id after it finished starts a new run.
- **Always finish the card** with `done` or `fail` — including when you abort,
  hit an error, or the user cancels. A card left in "running" lingers for an hour.
- Steps are **short** (≤ ~60 chars), human-level phases — "Building images",
  "Job 2/3: deploy → ssh", not raw log lines.
- Give real progress when you can count phases (`--progress 2/5`); omit it for
  an indeterminate bar.
- Don't send updates more than about once per second.
- Don't use it for quick tasks (< ~20s) or for every tool call.

## Commands

```bash
mac-notify start  <id> "<title>" [--step "..."] [--progress P] [--url URL] [--source S]
mac-notify update <id> [--step "..."] [--progress P | --indeterminate] [--url URL]
mac-notify done   <id> ["message"]
mac-notify fail   <id> ["reason"]
mac-notify info   "<title>" ["message"]      # one-off card, auto-hides
mac-notify remove <id>
mac-notify list
```

`P` = `0.4`, `40%`, or `3/7`. `--source` defaults to the current directory name.
`--url` makes the card clickable (e.g. the Actions run page).

## GitHub Actions / pipelines — use `gh-watch`

After pushing or triggering a workflow, follow the run with real step-based
progress. It blocks until the run completes and exits 0 on success, 1 otherwise,
printing the run URL and failing job/step — so you can react to failures.

```bash
git push
sleep 5                                          # let GitHub register the run
mac-notify gh-watch                              # latest run in current repo
mac-notify gh-watch <run-id> --repo owner/name --title "Deploy prod-booking"
```

Run it with the Bash tool's `run_in_background: true` if you want to keep
working while it watches; you'll be notified when it exits. For chained
workflows (CI → Deploy), watch the first, then find the next run
(`gh run list --workflow <name> --limit 1`) and watch it with a new card.

## Wrapping a long local command — use `run`

```bash
mac-notify run build-app "Building app" -- npm run build
```

Streams output as usual, shows the latest output line as the step, marks
done/fail by exit code, and returns the command's exit code.

## Multi-phase task you drive yourself

```bash
mac-notify start deploy-api "Deploy api → prod" --progress 0/4 --step "Running tests"
# ... run tests
mac-notify update deploy-api --progress 1/4 --step "Building image"
# ... build
mac-notify update deploy-api --progress 2/4 --step "Pushing to registry"
# ... push, then watch the pipeline
mac-notify update deploy-api --progress 3/4 --step "Waiting for health check"
mac-notify done deploy-api "v1.4.2 live"
# on any failure along the way:
mac-notify fail deploy-api "Health check timed out"
```

## Troubleshooting

- `mac-notify list` shows what the panel currently holds (`[]` if not running).
- Panel log: `/tmp/macnotify.log`. Source repo: `~/Projects/my/mac-notify`.
