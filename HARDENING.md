<!-- markdownlint-disable -->

# Hardening Report: 2ndSetAI--good-egg/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **2ndSetAI--good-egg/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references across action.yml and workflow files use mutable tags instead of full 40-character SHA commit digests, making the action vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.

Failing references:
- action.yml: `actions/setup-python@v5`
- .github/workflows/ci.yml: `actions/checkout@v4`, `actions/setup-python@v5`, `astral-sh/setup-uv@v4`
- .github/workflows/good-egg.yml: `2ndSetAI/good-egg@v2`
- .github/workflows/release.yml: `actions/checkout@v4`, `actions/setup-python@v5`, `astral-sh/setup-uv@v4`, `pypa/gh-action-pypi-publish@release/v1`, `softprops/action-gh-release@v2`

Locations:

- `action.yml:50`
- `.github/workflows/ci.yml:10`
- `.github/workflows/ci.yml:14`
- `.github/workflows/ci.yml:18`
- `.github/workflows/good-egg.yml:11`
- `.github/workflows/release.yml:11`
- `.github/workflows/release.yml:17`
- `.github/workflows/release.yml:21`
- `.github/workflows/release.yml:44`
- `.github/workflows/release.yml:52`
- `.github/workflows/release.yml:60`

### missing-permissions (severity: medium)

The workflow file ci.yml has no top-level `permissions:` key and its only job (`test`) also has no job-level `permissions:` key. Without explicit permissions, the job inherits the default repository token permissions, which may be overly broad (write access to contents, etc.).

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all unpinned action references to full 40-character commit SHAs:
- actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262
- actions/setup-python@v5 → @a26af69be951a213d495a4c3e4e4022e16d87065
- astral-sh/setup-uv@v4 → @38f3f104447c67c051c4a08e39b64a148898af3a
- 2ndSetAI/good-egg@v2 → @9d3e310471fa5ea76f5f47ec6d917ed41cdc4649
- pypa/gh-action-pypi-publish@release/v1 → @ba38be9e461d3875417946c167d0b5f3d385a247
- softprops/action-gh-release@v2 → @3bb12739c298aeb8a4eeaf626c5b8d85266b0e65

Original tags preserved as inline comments. Added top-level `permissions: {}` and job-level `permissions: { contents: read }` to ci.yml to fix the missing-permissions finding.

