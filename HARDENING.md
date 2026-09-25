<!-- markdownlint-disable -->

# Hardening Report: 2ndSetAI--good-egg/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **2ndSetAI--good-egg/v2.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step 'Set up Python' uses `actions/setup-python@v5`, which is pinned to a mutable version tag rather than an immutable 40-character SHA commit hash. This means the action could be silently updated or compromised without the workflow noticing, enabling supply-chain attacks. It should be pinned to a full SHA, e.g. `actions/setup-python@a26af69be951a213d495a4c3e4e4022e16d87065 # v5`.

Locations:

- `action.yml:51`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `actions/setup-python@v5` to its full commit SHA `a26af69be951a213d495a4c3e4e4022e16d87065` in hardened/action/action.yml (line 51). The `# v5` comment is retained for human readability.

