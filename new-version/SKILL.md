---
name: new-version
description: Bumps project version using semantic versioning from commits since last tag, updates the canonical version in code, commits and tags the release, then generates release notes. Uses a 7-day cooldown (last commit date) before proceeding. Use when the user asks for a new version, release, version bump, or to tag a release.
---

# New version (release workflow)

When the user asks for a new version, follow this workflow.

## 1. Cooldown check

- Get the last commit's date (e.g. `git log -1 --format="%cs"`).
- If that date is **more than 7 days before today** → cooled down, proceed.
- If the last commit is **within 7 days** → not cooled down. Explain and stop (or ask the user if they want to proceed anyway). Do not bump, commit, or tag without explicit confirmation.

## 2. Clean working tree

- Ensure the working tree is clean (`git status`). If not, list modified files and either abort or ask the user whether to include uncommitted changes in the release commit. Default to abort unless they confirm.

## 3. Discover current version and its location

Find the **canonical version** and where it is defined. Prefer, in order:

1. **Language metadata**: `pyproject.toml` (`[project] version`), `setup.cfg` (`[metadata] version`), `package.json` (`"version"`), `Cargo.toml` (`[package] version`).
2. **Version modules**: Root-level `__init__.py` / `version.py` with `__version__` or `VERSION`; root-level `VERSION` file.
3. **Project override**: If `.cursor/skills/new-version/` exists with extra instructions, follow those for locating and updating the version.

If multiple candidates exist, prefer language-standard files over ad-hoc files; if still ambiguous, ask the user. Parse the current version as semantic `X.Y.Z` (ignore or preserve pre-release/build suffixes if present).

## 4. Latest tag and commits

- Get the latest tag: e.g. `git describe --tags --abbrev=0` or tags sorted by date.
- Collect commits since that tag: `git log <last_tag>..HEAD --oneline` (or full history if no tags).
- Use this range to decide bump type (see below).

## 5. Bump type (semantic versioning)

- **major**: Any commit with `BREAKING CHANGE` or breaking markers (e.g. `feat!:`, `fix!:`).
- **minor**: Any commit starting with `feat` (`feat:`, `feat(scope):`, etc.).
- **patch/bug**: Otherwise.

If the user explicitly passed `major`, `minor`, or `bug`/`patch`, use that instead. Compute next version: major → `(X+1).0.0`, minor → `X.(Y+1).0`, patch → `X.Y.(Z+1)`.

## 6. Update version in code

- Edit only the version field/line in the canonical file (TOML/JSON/Python/VERSION). Preserve formatting and do not change unrelated fields. Re-read the file to confirm.

## 7. Commit and tag

- Stage only the version-changed file(s).
- Commit: message like `chore(release): bump version to X.Y.Z`.
- Tag: use the same version as the new tag (follow existing tag pattern, e.g. `vX.Y.Z` or `X.Y.Z`). Create an **annotated** tag (e.g. message `Release vX.Y.Z`). Do not overwrite an existing tag; if it exists, inform the user and ask. Do not push unless the user asks.

## 8. Release notes

- Generate release notes in Markdown for the range from the **previous** tag to the **new** tag (the one just created). Use the same structure and output rules as the **release-notes** skill (sections: Breaking Changes, Features, Fixes, Refactors, Docs, Chore; raw Markdown in a fenced block, emoji headers, no empty sections, title `## Release Notes <newTag>`).
- **Print** the release notes to the user (e.g. in the chat). Do **not** create or write a release-notes file unless the user explicitly asks for one.

## 9. Report back

- Summarise: old version, new version, bump type, commit hash, tag name.
- Remind how to push: `git push && git push --tags`.

If any step fails (e.g. cannot find version, git error), stop, explain, and ask the user how to proceed.
