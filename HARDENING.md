<!-- markdownlint-disable -->

# Hardening Report: 2ndSetAI--good-egg/v0.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **2ndSetAI--good-egg/v0.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two `uses:` references in action.yml use mutable version tags instead of pinned 40-character SHA digests, making the action vulnerable to supply-chain attacks if the referenced tags are moved or overwritten:
- `actions/setup-python@v5` (line 41)
- `astral-sh/setup-uv@v4` (line 46)
These should be pinned to their full commit SHAs, e.g. `actions/setup-python@<40-char-sha> # v5`.

Locations:

- `action.yml:41`
- `action.yml:46`

### script-injection (severity: high)

Two `run:` blocks directly interpolate a `${{ ... }}` GitHub Actions expression inside the shell command string (sub-rule a). Specifically, `${{ github.action_path }}` is substituted into the shell script before the shell ever sees it, meaning any unexpected characters in the value would be parsed by the shell before quoting can protect them.

Offending lines:
- Line 53: `cd ${{ github.action_path }}` (in the 'Install good-egg' step)
- Line 66: `cd ${{ github.action_path }}` (in the 'Run Good Egg' step)

The safe pattern is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead, which is already available as a shell variable and does not require expression interpolation: `cd "$GITHUB_ACTION_PATH"`

Locations:

- `action.yml:53`
- `action.yml:66`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed two unpinned action references by pinning to full commit SHAs: actions/setup-python@v5 → @a26af69be951a213d495a4c3e4e4022e16d87065 # v5, astral-sh/setup-uv@v4 → @38f3f104447c67c051c4a08e39b64a148898af3a # v4. Fixed two script-injection instances by replacing `${{ github.action_path }}` with the pre-set shell environment variable `"$GITHUB_ACTION_PATH"` in both the 'Install good-egg' and 'Run Good Egg' steps.

