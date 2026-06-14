<!-- markdownlint-disable -->

# Hardening Report: 2ndSetAI--good-egg/v0.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **2ndSetAI--good-egg/v0.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two `uses:` references in action.yml use mutable version tags instead of full 40-character commit SHA digests, making the action vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.
- `actions/setup-python@v5` (line 42)
- `astral-sh/setup-uv@v4` (line 46)

Locations:

- `action.yml:42`
- `action.yml:46`

### script-injection (severity: high)

Sub-rule (a): The expression `${{ github.action_path }}` is interpolated directly inside `run:` shell command strings in two steps. Although `github.action_path` is typically not attacker-controlled, any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell sees it, making it a script-injection risk per the check rules.

'Install good-egg' step (line 52): `cd ${{ github.action_path }}`
'Run Good Egg' step (line 65): `cd ${{ github.action_path }}`

Fix: use the environment variable `$GITHUB_ACTION_PATH` instead of the expression `${{ github.action_path }}` inside `run:` blocks.

Locations:

- `action.yml:52`
- `action.yml:65`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all findings in action.yml: (1) Pinned actions/setup-python@v5 to full SHA a26af69be951a213d495a4c3e4e4022e16d87065 and astral-sh/setup-uv@v4 to full SHA 38f3f104447c67c051c4a08e39b64a148898af3a, preserving the original tags as comments. (2) Replaced both occurrences of `${{ github.action_path }}` in run: blocks with the built-in `$GITHUB_ACTION_PATH` environment variable (also added proper quoting), eliminating the script-injection risk.

