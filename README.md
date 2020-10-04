# deploy-sphinx-doc
Action for automatic deployment of Python documentation generated with Sphinx.

### Usage 

### Step 1. doc-branch

The first step consists in adding to your repository an empty
branch which will host the documentation website files.
To do so, we suggest to create an empty `orphan` branch
to be pushed upstream

```bash
cd repository
git checkout --orphan gh-pages # or any other name
git rm -rf .
touch index.html
git add index.html
git commit -m 'gh-pages created'
git push --set-upstream origin gh-pages
```

### Step 2. Source files

In your python code repository (master or develop branch), 
set up your documentation folder using `sphinx-quickstart`
which creates in the source directory a `conf.py` file together with
an empty master file `index.rst`. You can then add the newly
created documentation source directory to your git repository
and commit.

### Step 3. Actions

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
        with:
          path: your-repository-name

      - name: Deploy
        uses: mbruno46/deploy-sphinx-doc@v0.1
        with:
          path: doc
          doc-branch: 'gh-pages'
```

