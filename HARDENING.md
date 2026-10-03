<!-- markdownlint-disable -->

# Hardening Report: 2ndSetAI--good-egg/v0.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **2ndSetAI--good-egg/v0.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two `uses:` references in action.yml use mutable tag refs instead of pinned 40-character SHA commit hashes, making the action vulnerable to supply-chain attacks if the upstream tag is moved or compromised:
- `uses: actions/setup-python@v5` (tag: v5)
- `uses: astral-sh/setup-uv@v4` (tag: v4)
These should be pinned to full SHA digests, e.g. `actions/setup-python@<40-hex-sha> # v5`.

Locations:

- `action.yml:43`
- `action.yml:47`

### script-injection (severity: high)

Two `run:` blocks in action.yml directly interpolate `${{ github.action_path }}` into shell command strings (sub-rule a). Any `${{ ... }}` expression interpolated directly inside a `run:` block is a script-injection risk because the value is substituted into the shell script before the shell parses it, bypassing shell quoting. Affected steps:
1. 'Install good-egg' step: `cd ${{ github.action_path }}`
2. 'Run Good Egg' step: `cd ${{ github.action_path }}`
Fix: use the environment variable `$GITHUB_ACTION_PATH` instead of the expression, e.g. `cd "$GITHUB_ACTION_PATH"`.

Locations:

- `action.yml:52`
- `action.yml:63`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed two unpinned action references by pinning to full SHA digests: actions/setup-python@v5 → @a26af69be951a213d495a4c3e4e4022e16d87065 # v5, and astral-sh/setup-uv@v4 → @38f3f104447c67c051c4a08e39b64a148898af3a # v4. Fixed two script-injection instances by replacing `${{ github.action_path }}` with the safe built-in environment variable `$GITHUB_ACTION_PATH` (quoted) in both the 'Install good-egg' and 'Run Good Egg' run blocks.

