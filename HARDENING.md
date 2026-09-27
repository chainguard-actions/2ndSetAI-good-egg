<!-- markdownlint-disable -->

# Hardening Report: 2ndSetAI--good-egg/v0.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **2ndSetAI--good-egg/v0.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml references two actions using mutable version tags instead of pinned 40-character SHA commit hashes. This exposes the action to supply-chain attacks if the upstream tags are moved or compromised. Failing references: `actions/setup-python@v5` (line 43) and `astral-sh/setup-uv@v4` (line 47).

Locations:

- `action.yml:43`
- `action.yml:47`

### script-injection (severity: high)

Sub-rule (a): Two `run:` blocks in action.yml directly interpolate `${{ github.action_path }}` inside shell command strings. Any `${{ ... }}` expression directly inside a `run:` block is a script-injection risk because the value is substituted by the GitHub Actions template engine before the shell ever sees it, bypassing shell quoting. Offending lines: `cd ${{ github.action_path }}` in the 'Install good-egg' step and `cd ${{ github.action_path }}` in the 'Run Good Egg' step. These should be replaced with the environment variable `$GITHUB_ACTION_PATH` which is set automatically by the runner.

Locations:

- `action.yml:53`
- `action.yml:68`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed two unpinned action references by pinning to full commit SHAs (actions/setup-python@v5 → a26af69be951a213d495a4c3e4e4022e16d87065, astral-sh/setup-uv@v4 → 38f3f104447c67c051c4a08e39b64a148898af3a). Fixed two script-injection instances by replacing `${{ github.action_path }}` with the runner-provided `$GITHUB_ACTION_PATH` environment variable in both `run:` blocks, properly quoted.

