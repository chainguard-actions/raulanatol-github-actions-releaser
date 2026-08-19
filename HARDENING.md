<!-- markdownlint-disable -->

# Hardening Report: raulanatol--github-actions-releaser/v2.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **raulanatol--github-actions-releaser/v2.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions using mutable tags/branches instead of pinned full-length SHA commits. In build.yml: `actions/checkout@v2` uses a tag. In release.yml: `actions/checkout@v2` uses a tag, and `raulanatol/github-actions-releaser@main` uses a branch name. These can be silently updated to point to malicious code.

Locations:

- `.github/workflows/build.yml:11`
- `.github/workflows/release.yml:11`
- `.github/workflows/release.yml:13`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no individual job declares its own `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/build.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all action references to full commit SHAs — actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e in both files, raulanatol/github-actions-releaser@main → @cd18dfc44baf26fd7c6b3a64369d8bf35794aabe in release.yml. (2) Added top-level `permissions: {}` to both workflow files to enforce least privilege. The release.yml deploy job additionally declares `permissions: contents: write` at the job level since the GitHub Actions Releaser needs to create/publish releases.

