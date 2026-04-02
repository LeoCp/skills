---
name: commit-pr
description: >
  Commits all changes into a new branch, pushes, and creates a Pull Request via GitHub CLI.
  Always creates a new branch to keep the original branch completely clean (local and remote).
  Use this skill whenever the user asks to "create a PR", "open PR", "commit and create PR",
  "prepare PR", "send for review", "push PR",
  or any variation involving committing changes and opening a Pull Request.
  Requires GitHub CLI (gh) authenticated and a remote configured.
---

# Commit & PR

Automates: stash changes → create new branch → apply and commit → push → create Pull Request.

The original branch is left completely untouched — no staged changes, no new commits, clean local and remote.

## Prerequisites

- Git repo with remote configured
- GitHub CLI (`gh`) authenticated — check with `gh auth status`
- Pending changes in working directory

## Flow

### 1. Check repo state

```bash
git status
git remote -v
gh auth status
```

Stop if `gh` is not authenticated or there are no pending changes.

### 2. Analyze changes first

Preview the changes to determine the branch name and commit message before touching anything:

```bash
git diff --stat
git diff
```

Use this analysis for both the branch name and the commit message.

### 3. Stash changes and create new branch

Save all changes (tracked and untracked) to stash, so the current branch stays completely clean:

```bash
git stash push -u -m "commit-pr: temporary stash"
```

Now create a new branch from the current position. Generate a descriptive branch name based on the diff using the pattern `feat/`, `fix/`, `refactor/`, `chore/`, or `docs/` followed by a short kebab-case summary. Use the user's branch name if they explicitly provided one.

```bash
git checkout -b <generated-branch-name>
```

### 4. Apply stash and commit

Restore the changes on the new branch:

```bash
git stash pop
git add -A
```

Generate the commit message automatically using Conventional Commits based on the actual code changes. Do NOT ask the user — analyze the diff and write the best message. Use the user's message only if they explicitly provided one.

**Do NOT add "Co-Authored-By" trailers or any Claude/AI attribution to the commit message.**

```bash
git commit -m "<generated-message>"
```

### 5. Push

```bash
git push -u origin <new-branch-name>
```

### 6. Create Pull Request

Determine the base branch:

```bash
git remote show origin | grep 'HEAD branch'
```

Generate the PR title and body automatically from the commits:

```bash
git log <base-branch>..HEAD --oneline
```

- **Title**: Derive from the changes — clear, descriptive summary.
- **Body**: Summarize all changes grouped logically. **Do NOT include "Generated with Claude Code" or any AI attribution in the PR body.**

```bash
gh pr create --title "<generated-title>" --body "<generated-body>" --base <base-branch>
```

Do NOT ask the user for title or body — generate them from the code changes. Only use `--reviewer`, `--label`, `--draft` if the user explicitly requests them.

## Error handling

- Stash pop conflicts → help user resolve before continuing
- Push rejected → suggest `git pull --rebase origin <branch>`
- PR already exists for this branch → show link with `gh pr view`
- No remote → guide user to add with `git remote add origin <url>`
