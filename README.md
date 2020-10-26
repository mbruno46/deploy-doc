# deploy-sphinx-doc
Action for automatic deployment of Python documentation generated with Sphinx.

[![Test](https://github.com/mbruno46/deploy-sphinx-doc/workflows/Test/badge.svg)](https://github.com/mbruno46/deploy-sphinx-doc/actions?query=workflow%3ATest)
[![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)

## Inputs

- `path`

  the relative path of the folder containing the documentation source files, including 
  `conf.py` and a `Makefile` invoking sphinx. 
  
- `doc-branch` (optional, default = `gh-pages`)

  the target branch used to publish the documentation. Make sure to turn on github pages
  in your repository settings, and use this branch as host. 

- `pypackages` (optional, default = `''`)
  
  additional python packages useful to build the documentation, such as `nbsphinx`.

**Note:** the `Makefile` generated from `sphinx-quickstart` can be modified except for 
the following three lines

```
SPHINXBUILD   ?= sphinx-build
SOURCEDIR     = .
BUILDDIR      = _build
```

## Example 

Set up your documentation using `sphinx-quickstart`
which creates a project directory with a `conf.py` file together with
an empty master file `index.rst`

```bash
$ cd repository
$ mkdir repo-doc
$ cd repo-doc
$ sphinx-quickstart --quiet --project=repository --author=myself -v v1
$ ls 
Makefile   _build     _static    _templates conf.py    index.rst  make.bat
```

You can then add the newly created directory (in our example above `repo-doc`) 
to your git repository and commit.

To use this action to automatically publish your documentation on a given
branch of your repository, use the following commands in your workflow

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

**Note:** make sure to checkout your repository and setup the python environment.

**Note:** if you use the field `path` within the checkout action you should make sure that
the field `path` inside deploy action is matched accordingly.
