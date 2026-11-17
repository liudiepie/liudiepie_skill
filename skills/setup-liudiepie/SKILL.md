---
name: setup-liudiepie
description: One-time project setup for the liudiepie workflow. Detects the project's typecheck, lint, test, and run commands, confirms them with the user, and records them in the project's CLAUDE.md so Phase 4 (Verify with Evidence) always knows exactly what to run. Use when the user runs /setup-liudiepie or asks to set up or configure liudiepie for a project.
disable-model-invocation: true
argument-hint: "[optional notes, e.g. 'monorepo, only packages/api']"
---

# liudiepie setup

Prepare the current project so the liudiepie workflow can verify work with real commands instead of guesses.

## Step 1: Detect the verification commands

Look only at files that already exist in the project; never invent a command that nothing in the repo defines.

1. Read the project's manifests and task runners, such as `package.json` scripts, `pyproject.toml`, `Makefile`, `justfile`, `Cargo.toml`, `go.mod`, `composer.json`, `Gemfile`, `build.gradle`, `pom.xml`, and `.github/workflows/*.yml`.
2. Note the package manager from lockfiles (`pnpm-lock.yaml`, `yarn.lock`, `bun.lockb`, `package-lock.json`, `uv.lock`, `poetry.lock`).
3. For each category, pick the most specific command the project defines:
   - **Typecheck**: for example `pnpm typecheck`, `tsc --noEmit`, `mypy .`, `cargo check`
   - **Lint**: for example `pnpm lint`, `ruff check .`, `golangci-lint run`
   - **Test**: for example `pnpm test`, `pytest`, `go test ./...`, `cargo test`
   - **Run the real path**: how to start the app or call it, for example `pnpm dev` on port 3000, `python -m app`, or a CLI entry point
4. If CI workflows run commands, prefer the ones CI uses, since those are what must pass.
5. Take the user's notes in `$ARGUMENTS` into account (for example, a monorepo package to focus on).

## Step 2: Confirm with the user

Show what you found as a short numbered list, one line per category, marking anything you could not find as "none found". Ask the user to confirm or correct it in one reply, offering your detected values as the defaults. Do not write anything until they answer.

## Step 3: Record the commands in CLAUDE.md

1. Open `CLAUDE.md` at the project root, creating it if it does not exist.
2. Add or replace a single section with exactly this heading, leaving the rest of the file untouched:

   ```markdown
   ## liudiepie verification

   - Typecheck: `<command or "none">`
   - Lint: `<command or "none">`
   - Test: `<command or "none">`
   - Run the real path: `<command and how to check it, or "none">`
   ```

3. If the section already exists, update it in place rather than adding a second copy.

## Step 4: Prove the commands work

Run each confirmed command once (skip the long-running "run" command unless it exits on its own). Report which ones passed, which failed, and the first lines of any error, so the user knows the baseline before starting real work. Do not fix failures during setup; just report them.

## Step 5: Finish

Tell the user setup is done and that they can now start work with `/liudiepie <describe the task>` (or `/liudiepie:liudiepie <describe the task>` if they installed liudiepie as a Claude Code plugin).
