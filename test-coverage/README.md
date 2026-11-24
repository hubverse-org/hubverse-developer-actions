# `test-coverage`

Computes test coverage of an R package with covr and uploads it to [Codecov](https://about.codecov.io/).

Set it up in a package with:

```r
hubDevs::use_hubdev_github_action("test-coverage")
```

The workflow runs on pushes to `main` (or `master`) and on pull requests.

## Authentication

The upload authenticates with [OIDC](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect), so no Codecov token is stored in the repository or the organization. This follows the [hubverse GitHub Actions security RFC](https://github.com/hubverse-org/decisions/blob/main/decisions/2025-02-13-rfc-github-action-security.md), which prefers OIDC for authenticating to external services. The job needs `id-token: write` for this, and the workflow sets it.

The repository must be activated on Codecov.

GitHub issues no OIDC token to two kinds of pull request:

- **Dependabot PRs.** The workflow skips the upload. Coverage is still computed and the tests still run.
- **PRs from forks.** The workflow uploads without a token. Codecov accepts tokenless uploads from forks of public repositories. If Codecov refuses the upload, the job does not fail.

On every other run, a refused upload fails the job.
