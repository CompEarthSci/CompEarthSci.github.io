---
layout: post
title:  "Publishing to PyPi"
date:   2025-12-11 14:40:00 +0000
categories: meetings
---
This contains some brief notes from our session on automated publishing to package indices.

We used [https://test.pypi.org/] as our target repository. This works in the same way as PyPi but is intended for
tests - material is frequently deleted to save space and resources. To publish here (or on PyPi) you need to 
create an account, link that to an email address and create a two factor authentication method.

To publish a package you need to build it and then publish it. Following on from our discussion of `uv` last week
we used `uv build` and `uv publish` for these steps. It is also possible to use python's `build` module with `twine`.
The publish step needs an API token. Create this and set the environment variable `UV_PUBLISH_TOKEN` to the token, and `UV_PUBLISH_URL` to 
`https://test.pypi.org/legacy/` (the default is the main PyPi instance) before running `uv publish`.  At this stage you could (and should)
remove the global API token on test.pypi and create a project specific token. We don't need this as we now look at setting up publishing
via github actions.

One way to approach publishing via github is to create a workflow that looks something like:

```
name: Publish to PyPI
# Put a version on PyPI when a GitHub release is created
on:
 release:
   types: [published]

permissions: {}

jobs:
  build:
    name: Build distribution
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v6
      with:
        persist-credentials: false
    - name: Install uv
      uses: astral-sh/setup-uv@1e862dfacbd1d6d858c55d9b792c756523627244
      with:
        # Install a specific version of uv.
        version: "0.9.13"
        python-version: "3.14"
        enable-cache: false
    - name: Build a binary wheel and a source tarball
      run: uv build
    - name: Store the distribution packages
      uses: actions/upload-artifact@v5
      with:
        name: python-package-distributions
        path: dist/

  publish-to-pypi:
    name: >-
      Publish distribution
    needs:
    - build
    runs-on: ubuntu-latest
    environment:
      name: test-pypi
      url: https://test.pypi.org/p/prem4derg  
    permissions:
      id-token: write  

    steps:
    - name: Download all the dists
      uses: actions/download-artifact@v6
      with:
        name: python-package-distributions
        path: dist/
    - name: Publish distribution 
      uses: pypa/gh-action-pypi-publish@ed0c53931b1dc9bd32cbe73a98c7f6766f8a527e
      with:
        repository-url: https://test.pypi.org/legacy/
```

Useful links: 
* [https://docs.astral.sh/uv/guides/package/#publishing-your-package]
* [https://packaging.python.org/en/latest/tutorials/packaging-projects/]
* [https://www.pyopensci.org/blog/python-packaging-security-publish-pypi.html]
* [https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/]

Note that you need to enable a trusted publishing source on PyPi. Please use an environment to constrain permissions.
