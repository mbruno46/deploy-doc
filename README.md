# deploy-sphinx-doc
Action for automatic deployment of Python documentation generated with Sphinx.

[![Test](https://github.com/mbruno46/deploy-sphinx-doc/workflows/Test/badge.svg)](https://github.com/mbruno46/deploy-sphinx-doc/actions?query=workflow%3ATest)
[![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)

## Usage 

In your python code repository (master or develop branch), 
set up your documentation folder using `sphinx-quickstart`
which creates in the project directory a `conf.py` file together with
an empty master file `index.rst`. You can then add the newly
created documentation source directory to your git repository
and commit.

```bash
cd repository
mkdir repo-doc
cd repo-doc
sphinx-quickstart --quiet --project=repository --author=myself -v v1
```

Add the appropriate commands to your current workflow.

```yml
name: Deploy Doc

on: [push]

jobs:
  deploy:

    runs-on: ubuntu-latest

    steps:

      - name: Setup python
        uses: actions/setup-python@v2
        with:
          python-version: 3.6
          architecture: x64

      - name: Clone repository
        uses: actions/checkout@v2

      - name: Deploy
        uses: mbruno46/deploy-sphinx-doc@v0.1
        with:
          path: doc
          doc-branch: 'gh-pages'
```

