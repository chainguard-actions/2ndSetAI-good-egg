<!-- markdownlint-disable -->

# Hardening Report: 2ndSetAI--good-egg/v0.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **2ndSetAI--good-egg/v0.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml references two GitHub Actions using mutable version tags instead of pinned full-length SHA commit hashes. This exposes the action to supply-chain attacks if the upstream tag is moved or compromised. Failing references: `actions/setup-python@v5` and `astral-sh/setup-uv@v4`. These should be pinned to their full 40-character commit SHAs (e.g. `actions/setup-python@<sha> # v5`).

Locations:

- `action.yml:40`
- `action.yml:44`

### script-injection (severity: high)

Sub-rule (a): `${{ github.action_path }}` is interpolated directly inside `run:` shell command strings in two steps. Any `${{ ... }}` expression embedded directly in a `run:` block is a script-injection risk because the value is substituted into the shell script before the shell parses it. The affected lines are:
- Step 'Install good-egg': `cd ${{ github.action_path }}`
- Step 'Run Good Egg': `cd ${{ github.action_path }}`
The safe pattern is to pass the value through an `env:` variable and reference it as `"$ACTION_PATH"` in the shell script.

Locations:

- `action.yml:51`
- `action.yml:64`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

1. Pinned actions/setup-python@v5 to full SHA a26af69be951a213d495a4c3e4e4022e16d87065 and astral-sh/setup-uv@v4 to full SHA 38f3f104447c67c051c4a08e39b64a148898af3a, preserving the version tags as comments. 2. Moved ${{ github.action_path }} out of both run: blocks (Install good-egg and Run Good Egg steps) into env: variables named ACTION_PATH, then referenced them as "$ACTION_PATH" in the shell scripts to prevent script injection.

