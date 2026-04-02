---
name: commit-push
description: >
  Commits all pending changes and pushes to the remote in one step.
  Use this skill whenever the user asks to "commit and push", "push all", "push everything",
  "send to remote", "upload to GitHub", "commit and upload", "save and push",
  or any variation involving committing changes and pushing them.
  Also applies when the user simply says "push" or "send it up" in a Git repo context.
  Do NOT use this skill if the user also wants to create a PR — use commit-pr instead.
  Requires a Git repo with a remote configured.
---

# Commit & Push

Automates: commit all changes → push to remote.

## Prerequisites

- Git repo with remote configured
- Pending changes in working directory

## Flow

### 1. Check repo state

```bash
git status --short
git remote -v
```

Stop if no pending changes or no remote configured.

### 2. Commit all

Stage everything, then read the diff to understand what changed:

```bash
git add -A
git diff --cached --stat
git diff --cached
```

Generate the commit message automatically using Conventional Commits based on the actual code changes. Do NOT ask the user for a message — analyze the diff and write the best one. Use the user's message only if they explicitly provided one.

**Do NOT add "Co-Authored-By" trailers or any Claude/AI attribution to the commit message.**

```bash
git commit -m "<generated-message>"
```

### 3. Push

```bash
git push -u origin $(git branch --show-current)
```

## Error handling

- Push rejected (non-fast-forward) → suggest `git pull --rebase origin <branch>` then retry
- No remote → guide user to add with `git remote add origin <url>`
- Conflicts on rebase → help user resolve before continuing
- Auth failed → suggest `gh auth status` or SSH config check
