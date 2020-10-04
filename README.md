# deploy-sphinx-doc
Action for automatic deployment of Python documentation generated with Sphinx

## First usage

```bash
cd repository
git checkout --orphan gh-pages # or any other name
git rm -rf .
touch index.html
git add index.html
git commit -m 'gh-pages created'
git push --set-upstream origin gh-pages
```
