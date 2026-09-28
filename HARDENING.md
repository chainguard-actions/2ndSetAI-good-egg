<!-- markdownlint-disable -->

# Hardening Report: 2ndSetAI--good-egg/v0.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **2ndSetAI--good-egg/v0.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Two `run:` blocks in action.yml directly interpolate `${{ github.action_path }}` inside shell commands (sub-rule a: direct expression interpolation). Any `${{ ... }}` expression inside a `run:` script is a script-injection risk because YAML template substitution occurs before the shell ever sees the value. Offending lines: `cd ${{ github.action_path }}` in both the 'Install good-egg' step and the 'Run Good Egg' step. These should be replaced with the environment variable `$GITHUB_ACTION_PATH` which is already set by the runner.

Locations:

- `action.yml:44`
- `action.yml:55`

### unpinned-uses (severity: high)

Two `uses:` references in action.yml are pinned to mutable version tags rather than immutable 40-character SHA digests, making the action vulnerable to supply-chain attacks if the upstream tag is moved or compromised. Failing references: `actions/setup-python@v5` and `astral-sh/setup-uv@v4`. Each should be replaced with the full commit SHA of the intended version, e.g. `actions/setup-python@<40-char-sha> # v5`.

Locations:

- `action.yml:40`
- `action.yml:43`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed all three findings in hardened/action/action.yml: (1) Replaced both `${{ github.action_path }}` expressions in `run:` blocks with `"$GITHUB_ACTION_PATH"` (the runner-provided env var, quoted) to eliminate script-injection risk. (2) Pinned `actions/setup-python@v5` to its full SHA `a26af69be951a213d495a4c3e4e4022e16d87065` and `astral-sh/setup-uv@v4` to its full SHA `38f3f104447c67c051c4a08e39b64a148898af3a`, with the original tags preserved as inline comments.

