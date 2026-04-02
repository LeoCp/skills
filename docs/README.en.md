# Claude Code Skills

A collection of custom skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Each skill is a reusable slash command that automates common development workflows.

## Skills

| Skill | Command | Description |
|-------|---------|-------------|
| **commit-push** | `/commit-push` | Commits all pending changes and pushes to the remote in one step. |
| **commit-pr** | `/commit-pr` | Commits changes into a new branch, pushes, and creates a Pull Request via `gh`. Keeps the original branch clean. |
| **bump-patch** | `/bump-patch` | Increments patch version (e.g. `1.1.2` → `1.1.3`) across all project files. |
| **bump-minor** | `/bump-minor` | Increments minor version (e.g. `1.1.2` → `1.2.0`) across all project files. |
| **bump-major** | `/bump-major` | Increments major version (e.g. `1.1.2` → `2.0.0`) across all project files. |
| **interrogate** | `/interrogate` | Stress-tests a plan, design, or architecture through relentless questioning until reaching shared understanding. |

### Version Bump Details

The bump skills scan and update version references in:

- `package.json`
- `app.json` (Expo)
- `app.config.js` / `app.config.ts`
- `ios/**/project.pbxproj`
- `ios/**/Info.plist`
- `android/app/build.gradle`

Build number always increments by 1 and never resets.

## Installation

Clone this repo and add the path to your Claude Code settings (`~/.claude/settings.json`):

```json
{
  "skills": [
    "/path/to/skills/commit-push",
    "/path/to/skills/commit-pr",
    "/path/to/skills/bump-patch",
    "/path/to/skills/bump-minor",
    "/path/to/skills/bump-major",
    "/path/to/skills/interrogate"
  ]
}
```

Or add the entire directory to load all skills at once.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- Git with a remote configured (for commit skills)
- [GitHub CLI](https://cli.github.com/) authenticated (for `commit-pr`)
