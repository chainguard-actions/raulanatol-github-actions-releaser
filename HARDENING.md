<!-- markdownlint-disable -->

# Hardening Report: raulanatol--github-actions-releaser/v2.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raulanatol--github-actions-releaser/v2.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference actions by mutable tag or branch names instead of pinned full-length commit SHAs, making the workflow vulnerable to supply-chain attacks if those tags/branches are moved or compromised.

- `.github/workflows/build.yml`: `uses: actions/checkout@v2` (tag `v2`)
- `.github/workflows/release.yml`: `uses: actions/checkout@v2` (tag `v2`)
- `.github/workflows/release.yml`: `uses: raulanatol/github-actions-releaser@main` (branch `main`)

All three should be pinned to a full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`).

Locations:

- `.github/workflows/build.yml:10`
- `.github/workflows/release.yml:10`
- `.github/workflows/release.yml:12`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no job within either file declares its own `permissions:` block. Without explicit permissions, GitHub Actions grants the default (often broad) token permissions, which violates the principle of least privilege.

- `.github/workflows/build.yml`: no `permissions:` at top level or job level
- `.github/workflows/release.yml`: no `permissions:` at top level or job level

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:
- build.yml: Pinned actions/checkout@v2 to full SHA (0717577d45739eb3c851188b29f50ed6c0b2194e) and added top-level `permissions: {}`.
- release.yml: Pinned actions/checkout@v2 to full SHA (0717577d45739eb3c851188b29f50ed6c0b2194e), pinned raulanatol/github-actions-releaser@main to full SHA (cd18dfc44baf26fd7c6b3a64369d8bf35794aabe), and added top-level `permissions: {}`.

