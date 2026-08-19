<!-- markdownlint-disable -->

# Hardening Report: raulanatol--github-actions-releaser/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raulanatol--github-actions-releaser/v2.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions using mutable tags or branch names instead of immutable full-length commit SHAs. This exposes the workflow to supply-chain attacks where a tag or branch can be silently updated to point to malicious code.

- `.github/workflows/build.yml`: `uses: actions/checkout@v2` (mutable tag `v2`)
- `.github/workflows/release.yml`: `uses: actions/checkout@v2` (mutable tag `v2`) and `uses: raulanatol/github-actions-releaser@main` (mutable branch `main`)

All `uses:` references should be pinned to a full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/build.yml:11`
- `.github/workflows/release.yml:10`
- `.github/workflows/release.yml:13`

### missing-permissions (severity: medium)

Neither `.github/workflows/build.yml` nor `.github/workflows/release.yml` declares a `permissions:` block at the top level or at the job level. Without explicit permissions, workflows inherit the default repository token permissions, which may be overly broad (e.g. `write` access to contents, pull requests, etc.). Each workflow should declare the minimal required permissions explicitly.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all mutable action references to full commit SHAs — actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e in both files, and raulanatol/github-actions-releaser@main → @cd18dfc44baf26fd7c6b3a64369d8bf35794aabe in release.yml. (2) Added permissions blocks — build.yml gets 'permissions: {}' (no token access needed), release.yml gets 'permissions: { contents: write }' (required for the releaser action to create GitHub releases).

