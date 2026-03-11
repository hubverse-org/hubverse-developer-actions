# hubverse-developer-actions <img src="https://github.com/hubverse-org/hubDocs/blob/main/docs/_static/LOGO-hubverse.png?raw=true" align="right" width="50px"/>

<!-- badges: start -->
<!-- badges: end -->

> GitHub Actions for common hubverse developer CI tasks

This repository stores [GitHub Actions](https://github.com/features/actions) for hubverse developers and packages, which can be used to do a variety of common hubverse developer CI tasks.

## Workflows

| Workflow | Language | Description |
|----------|----------|-------------|
| [`pkgdown-netlify-preview`](pkgdown-netlify-preview/) | R | Build pkgdown site, deploy production to GitHub Pages, deploy PR previews to Netlify |
| [`publish-pypi`](publish-pypi/) | Python | Build, publish to PyPI via trusted publishing, sign with Sigstore, create GitHub release |
| [`publish-pypi-test`](publish-pypi-test/) | Python | Build and publish to TestPyPI via trusted publishing |

## Action pinning policy

We use a tiered approach to pinning third-party GitHub Actions, balancing supply-chain security with maintenance burden. For the full security rationale, see the [hubverse security docs](https://docs.hubverse.io/en/latest/developer/security.html).

### Tier 1 -- GitHub-official actions

**Pin to: major version tag** (e.g., `@v5`)

Actions under `actions/*` and `github/*`. Maintained by GitHub, verified publisher, extremely low supply-chain risk.

Examples: `actions/checkout`, `actions/setup-python`, `actions/upload-artifact`, `actions/download-artifact`

### Tier 2 -- Trusted ecosystem actions

**Pin to: major version tag** (e.g., `@v2`) **or latest exact version tag** if the project doesn't maintain rolling major tags.

Actions from major, trusted organisations that are de facto standards in their ecosystem.

| Action | Maintainer | Pin style |
|--------|-----------|-----------|
| `r-lib/actions/*` | Posit/tidyverse | `@v2` (rolling major tag) |
| `astral-sh/setup-uv` | Astral | `@v7` (rolling major tag) |
| `pypa/gh-action-pypi-publish` | Python Packaging Authority | `@v1.13.0` (exact version tag) |
| `sigstore/gh-action-sigstore-python` | Sigstore / OpenSSF | `@v3.2.0` (exact version tag) |

**Why some use exact version tags:** Not all trusted projects maintain rolling major version tags. `pypa/gh-action-pypi-publish` and `sigstore/gh-action-sigstore-python` only publish exact semver tags (e.g., `v1.13.0`, `v3.2.0`), so we pin to the latest version. Dependabot handles minor and major update PRs; patch updates are ignored (see below).

### Tier 3 -- Other third-party actions

**Pin to: full SHA** with a version comment (e.g., `@9d877eea...  #v4.7.6`)

Actions from individual maintainers or smaller organisations where a compromised tag is a real risk.

Examples: `JamesIves/github-pages-deploy-action`, `nwtgck/actions-netlify`

### Dependabot configuration

The dependabot config complements this policy:
- **Patch updates ignored** for all actions (reduces noise for SHA-pinned and exact-version-pinned actions)
- **Minor updates grouped** into single PRs
- **Security alerts unaffected** -- dependabot security alerts are a separate system and will still fire for known CVEs regardless of version update settings

### CodeQL trusted owners

The `security-extended` CodeQL suite flags actions not pinned to SHA. To prevent false positives for Tier 2 actions, we maintain an org-level CodeQL model pack ([`codeql-model-pack/`](codeql-model-pack/)) that lists trusted action owners: `r-lib`, `astral-sh`, `pypa`, `sigstore`.

The pack is automatically published to GitHub Container Registry as `hubverse-org/trusted-actions` whenever files in `codeql-model-pack/` are changed on `main` (via the [`publish-codeql-pack`](.github/workflows/publish-codeql-pack.yaml) workflow). It is referenced in the hubverse-org org-level CodeQL configuration so it applies to all repos.

If a new action is promoted to Tier 2, add its owner to [`codeql-model-pack/models/trusted-actions-owners.yml`](codeql-model-pack/models/trusted-actions-owners.yml) and the pack will be republished automatically.

### Adding a new third-party action

For the process of evaluating and adding new third-party actions to hubverse workflows (including tier assessment, pinning method, and CodeQL/allowlist updates), see the [hubverse security docs](https://docs.hubverse.io/en/latest/developer/security.html).

See [#18](https://github.com/hubverse-org/hubverse-developer-actions/issues/18) for the full discussion.
