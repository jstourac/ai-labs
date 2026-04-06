---
name: package-skills
description: Package skills into zip files for distribution — trigger phrases: "package skills", "zip skills", "build skill zips"
user-invocable: true
---

# Package Skills

Package each skill directory under `skills/` into a separate zip file at the repo root, following the convention in AGENTS.md.

## Steps

1. List all subdirectories under `skills/` — each is a skill to package.
2. For each skill directory `skills/{skill-name}/`:
   - Skip if the zip is up to date (see Rules below).
   - Create a zip file at the repo root named `{skill-name}.zip`.
   - The zip must contain the skill files at the root level (not nested under a subdirectory), so that when extracted the files land directly in the destination skill folder.
   - Command (run from repo root): `cd skills/{skill-name} && zip -r ../../{skill-name}.zip . && cd ../..`
3. After processing all skills, report which zips were regenerated and which were skipped, along with file sizes for regenerated zips.

## Rules

- Only package directories that contain a `SKILL.md` file — skip any other directories.
- Do not regenerate a zip if none of the skill's files have changed since the zip was last built. Check this with `git status` and `git diff` to detect any tracked modifications or untracked new files inside `skills/{skill-name}/`. If the skill directory has no changes relative to the last commit and the zip file already exists, skip it.
- Overwrite existing zip files without prompting when regeneration is needed.
- Do not modify any skill source files.
- Zip files go at the repo root, never inside `skills/`.
