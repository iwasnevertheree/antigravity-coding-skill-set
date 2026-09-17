# Detailed Git Workflow

This document provides comprehensive guidance for repository state inspection, safe changes, diff reviews, committing, branching, and pull requests. Read this whenever a task involves version control operations.

---

## 1. Repository State Inspection

Always understand repository state before making any modifications:
- **Check status**: Run `git status` to see unstaged changes, staged files, and untracked files.
- **Check current branch**: Run `git branch --show-current` or `git branch` to confirm current branch.
- **Check recent history**: Run `git log --oneline -10` to inspect commit conventions and recent history.
- **Check pending operations**: Look for in-progress merge, rebase, cherry-pick, or stash entries (`git stash list`).

If uncommitted modifications or staged changes exist:
1. Acknowledge and inform the user of the existing state.
2. Do not overwrite, reset, or stash their changes without asking.
3. Confine modifications strictly to the files needed for your task.

---

## 2. Branch Awareness

- **Protected branches**: Treat `main`, `master`, `trunk`, and release branches as protected. Warn the user immediately if working directly on a protected branch.
- **Branching conventions**: Follow the repository's established branch naming pattern (e.g., `feature/*`, `fix/*`, ticket prefixes).
- **Atomic scope**: Keep branch and changeset focused on a single responsibility.

---

## 3. Making Safe Changes & Diff Review

- Always work from a clean, known base state when possible.
- Make atomic, focused edits. Do not bundle unrelated refactoring or formatting sweeps.
- Before committing, run `git diff` (unstaged) and `git diff --staged` (staged) to verify:
  - [ ] No accidental whitespace churn or unrelated formatting edits
  - [ ] No debug statements, print logs, or temporary scaffolding left behind
  - [ ] No commented-out dead code blocks
  - [ ] No hardcoded secrets, private paths, or temporary tokens
  - [ ] Only intended files are staged

---

## 4. Commit & PR Workflow

- **Intentional staging**: Stage files explicitly with `git add <file1> <file2>`. Avoid `git add .` or `git add -A` which easily include unintended files.
- **Message format**: Follow Conventional Commits format via [commit-message-format.md](../resources/commit-message-format.md).
- **Logical commits**: One logical change per commit.
- **Authorization & Execution**:
  - If the user explicitly asks to commit (e.g., "Commit these changes"): that request **is** the authorization. Review diff, stage intended files, execute `git commit`, and report the commit hash and summary. Do **not** ask for a redundant second confirmation.
  - If unsolicited: propose the commit message and staged diff, and await user direction.
- **PR workflow**: Use [pr-description-template.md](../examples/pr-description-template.md) and self-review against [code-review-guidelines.md](code-review-guidelines.md). Ensure CI checks pass.

---

## 5. Safety Boundaries & Authorization Gate

> [!CAUTION]
> **Operations requiring explicit user authorization:**
> - `git commit` / `git push`
> - `git reset` (soft, mixed, or hard)
> - `git rebase` / `git merge`
> - `git checkout` / `git switch` (switching branches or discarding files)
> - `git stash drop` / `git stash clear`
> - `git branch -d` / `git branch -D`
> - `git push --force` / `git push --force-with-lease`
> - `git clean`
> - Any history-rewriting operation

### Destructive Operations Protocol
- **Authorization gate**: Never perform destructive operations without explicit authorization for that specific action.
- **User requested a destructive operation**: An explicit user command (e.g., "Force push this branch to origin") satisfies authorization:
  1. Inspect current branch and remote status.
  2. Explain the operational impact (overwriting remote branch history).
  3. Use the safest variant: prefer `git push --force-with-lease` over `--force` to protect against overwriting upstream commits created by collaborators.
  4. Execute and report the outcome.
- **Ambiguous requests**: If the user asks to "push" without specifying force, attempt normal `git push`. If rejected, explain the conflict and request explicit authorization before considering any forced operation.
- **When uncertain**: Always pause and ask for clarification if an operation could result in data loss.
