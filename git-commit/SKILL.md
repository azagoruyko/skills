---
name: git-commit
description: Groups and commits working-directory changes in logical order by analyzing diffs, splitting into self-contained groups, and committing each with repository-style messages. Use when the user asks to commit changes, group commits, clean up git history, split changes into logical commits, or stage and commit in order.
---

# Group and commit changes in logical order

Analyze all unstaged and staged changes, split them into logical groups, and commit each group separately so history is clear and each commit is self-contained.

## Workflow

1. **Analyze** – Run `git status` and inspect diffs (`git diff`, `git diff --cached`) to understand what changed.
2. **Group** – Group related changes. Prefer:
   - Independent changes in separate commits (e.g. refactor in one, feature in another).
   - If changes depend on each other, commit the least-dependent (foundational) changes first so rollbacks leave the project in a working state.
3. **Commit** – For each group:
   - **Whole-file staging** – When all changes in a file belong to this group: `git add <path>`.
   - **Chunk staging** – When a file has changes for different groups, stage only the hunks for this group:
     - Inspect hunks with `git diff <path>` (unstaged) or `git diff --cached <path>` (staged).
     - Create a patch file with only the hunks for this group, then: `git apply --cached <patchfile>`.
   - Write a clear commit message for that group.
   - Run `git commit`.
4. **Repeat** – Continue until all changes are committed.

## Commit message format

Use the format adopted in the repository. Check existing commits: `git log --oneline -20`.
