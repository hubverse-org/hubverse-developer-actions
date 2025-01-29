# `publish-test-pypi`

This workflow builds a Python package and publishes it to
test.pypi.org whenever changes are pushed to the `main` branch.

This workflow uses a trusted publisher when writing to TestPyPI, so there are a
few one-time setup steps to complete before using it.

See the Hubverse
[Python release process documentation](https://hubverse.io/en/latest/developer/python.html#testpypi-setup)
for more information.
