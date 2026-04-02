---
name: bump-major
description: >
  Increment major version (e.g. 1.1.2 -> 2.0.0) across all project files.
  Trigger on "/bump-major", "bump major".
  Scans the project for app.json, app.config, project.pbxproj, Info.plist,
  build.gradle, package.json and updates all version references.
  Build number always increments +1.
---

# bump-major

Increment the **major** version across all project files. No questions — just do it.

## What it does

- `1.1.2` → `2.0.0` (minor and patch reset to 0)
- Build number: `46` → `47` (always +1, never resets)

## Steps

1. Find the project root (current working directory).
2. Scan for all version files.
3. Read the current version and build number.
4. Bump major: `major.minor.patch` → `(major+1).0.0`
5. Increment build number by 1.
6. Update **every** file found.
7. Show a before/after summary table.

## Files to scan and update

| File | Fields |
|------|--------|
| `package.json` | `version` |
| `app.json` | `expo.version`, `expo.ios.buildNumber`, `expo.android.versionCode` |
| `app.config.js` / `app.config.ts` | hardcoded `version`, `buildNumber`, `versionCode` |
| `ios/**/project.pbxproj` | `MARKETING_VERSION`, `CURRENT_PROJECT_VERSION` (all build configs: Debug + Release) |
| `ios/**/Info.plist` (skip Pods) | `CFBundleShortVersionString`, `CFBundleVersion` |
| `android/app/build.gradle` | `versionName`, `versionCode` |

## How to update each file

### package.json
Read JSON, update `"version"` field, write back with 2-space indent.

### app.json
Read JSON, update:
- `expo.version` → new version string
- `expo.ios.buildNumber` → new build number as string
- `expo.android.versionCode` → new build number as integer
Write back with 2-space indent.

### app.config.js / app.config.ts
If it has hardcoded version values, use regex to replace them.
If it reads from `package.json`, skip it and note that in the output.

### project.pbxproj
Use regex to replace ALL occurrences of:
- `MARKETING_VERSION = <old>;` → `MARKETING_VERSION = <new>;`
- `CURRENT_PROJECT_VERSION = <old>;` → `CURRENT_PROJECT_VERSION = <new_build>;`

### Info.plist
If values are Xcode variables like `$(MARKETING_VERSION)` — skip (resolved at build time).
Otherwise update `CFBundleShortVersionString` and `CFBundleVersion`.
Handle both binary and XML plist formats.

### build.gradle / build.gradle.kts
Use regex to replace `versionName` and `versionCode`.

## Output format

```
✅ bump-major: 1.1.2 → 2.0.0 (build 46 → 47)

| Arquivo         | Campo                      | Antes | Depois |
|-----------------|----------------------------|-------|--------|
| package.json    | version                    | 1.1.2 | 2.0.0  |
| app.json        | expo.version               | 1.1.2 | 2.0.0  |
| app.json        | expo.ios.buildNumber       | 46    | 47     |
| ...             | ...                        | ...   | ...    |
```

## Rules

- **DO NOT ask questions.** Just run immediately.
- Only update files that exist. Skip silently if not found.
- Build number **always** increments +1, never resets.
- Update ALL occurrences in project.pbxproj (there are usually multiple — one per build configuration).
