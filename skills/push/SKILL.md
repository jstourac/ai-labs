---
name: push
description: Rebase, squash all branch commits into one Conventional Commit, and push to the remote. Trigger phrases: "push", "/push"
---

Rebase, squash all branch commits into one Conventional Commit, and push to the remote.

## Behavior

### 1. Preflight checks
- Confirm there are no uncommitted changes (`git status`). If there are, stop and tell the user to commit or stash them first.
- Identify the base branch (default: `main`; fall back to `master` if `main` doesn't exist).
- Detect the remote: use `git rev-parse --abbrev-ref --symbolic-full-name @{u}` to read the tracking remote from the current branch (e.g. `upstream/main` → remote is `upstream`). If no tracking remote is set, fall back to `origin`.
- Fetch the latest remote state: `git fetch <remote>`.

### 2. Rebase onto the base branch
- Run `git rebase <remote>/<base-branch>`.
- If the rebase succeeds, continue to step 3.
- If there are **conflicts**, stop immediately and show the user:
  - Which files have conflicts (`git diff --name-only --diff-filter=U`)
  - Concrete instructions to resolve them:
    ```
    # Option A — resolve manually
    1. Open each conflicting file and resolve the <<<<< / ===== / >>>>> markers
    2. git add <resolved-file>
    3. git rebase --continue
    4. Re-run /push when done

    # Option B — abort and start over
    git rebase --abort
    ```
  - Do NOT proceed with squash or push until the user confirms the rebase is clean.

### 3. Collect commits to squash
- Find the commits unique to this branch: `git log <remote>/<base-branch>..HEAD --oneline`
- If there is only **one commit**, skip the squash step (already a single commit) and jump to step 5.
- Show the user the list of commits that will be squashed and ask for confirmation before proceeding.

### 4. Squash into a single Conventional Commit
- Determine the squash range: `git reset --soft <remote>/<base-branch>`
- Inspect the full diff (`git diff --cached`) to compose a Conventional Commit message:
  - **Type**: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `ci`, `build`, `perf`, `style`, `revert`
  - **Scope**: the affected module or component (omit if too broad)
  - **Breaking change**: add `!` and a `BREAKING CHANGE:` footer if the public API changes
  - Format:
    ```
    <type>[(<scope>)][!]: <short description>

    [optional body — summarise key changes if non-obvious]

    [optional footers: BREAKING CHANGE, Closes #issue]
    ```
  - Description: imperative mood, lowercase, no trailing period, ≤ 72 chars
- Show the proposed message to the user and **ask for confirmation** before committing.
- On approval: `git commit -m "<confirmed message>"`

### 5. Push
- Run `git push <remote> HEAD:<current-branch>`.
- If the push is rejected (non-fast-forward after a rebase), use `git push --force-with-lease` — this is safe because the rebase already happened intentionally. Inform the user that a force-push was used.
- Report the resulting commit hash, title, and remote URL.
