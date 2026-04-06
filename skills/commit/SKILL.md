---
name: commit
description: Generate and apply a Conventional Commit message based on staged or all changes. Trigger phrases: "commit", "/commit"
---

Generate and apply a [Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/) message based on staged (or all) changes.

## Behavior

1. Run `git status` and `git diff HEAD` to inspect current changes
2. If nothing is staged, stage all tracked modified files (`git add -u`) — ask before adding untracked files
3. Infer the commit type, optional scope, and short description from the diff:
   - **Type**: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `ci`, `build`, `perf`, `style`, `revert`
   - **Scope**: the affected module, directory, or component (omit if too broad)
   - **Breaking change**: add `!` after type/scope if the change breaks the public API; include a `BREAKING CHANGE:` footer
4. Format the commit message:
   ```
   <type>[(<scope>)][!]: <short description>

   [optional body — only if changes are non-obvious]

   [optional footers: BREAKING CHANGE, Closes #issue]
   ```
   - Description: imperative mood, lowercase, no period, ≤ 72 chars
   - Body and footers: include only when the *why* or impact isn't obvious from the title
5. Show the proposed message to the user and ask for confirmation before committing
6. On approval, run `git commit -m "..."` with the confirmed message
7. Report the resulting commit hash and title
