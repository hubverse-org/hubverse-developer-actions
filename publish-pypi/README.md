# `publish-pypi`

This workflow is triggered by adding a release tag to a repository and does the
following:

- Builds a Python package
- Publishes it to PyPI
- Signs the Python distribution
- Creates a GitHub release

This workflow uses a trusted publisher when writing to PyPI, so there are a
few one-time setup steps to complete before using it.

See the Hubverse
[Python release process documentation](https://hubverse.io/en/latest/developer/python.html#pypi-setup)
for more information.
