<!-- markdownlint-disable -->

# Hardening Report: 2ndSetAI--good-egg/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **2ndSetAI--good-egg/v2.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action uses `actions/setup-python@v5`, which is pinned to a mutable version tag (`@v5`) rather than an immutable 40-character commit SHA. This means the action could be silently updated to a different (potentially malicious) version without any change to this file, creating a supply-chain risk.

Locations:

- `action.yml:52`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `actions/setup-python@v5` to its full commit SHA `a26af69be951a213d495a4c3e4e4022e16d87065` in action.yml line 52, preserving the `# v5` tag as a comment for readability. This eliminates the supply-chain risk from the mutable version tag.

