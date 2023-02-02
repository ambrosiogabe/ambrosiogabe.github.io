# A Blog

This is derived from the Hyde blog template found [here](https://github.com/poole/hyde).

## Deployment

Run:

```powershell
$env:JEKYLL_ENV="production"
jekyll serve
# Now the site should be built in _site
mkdir ..\tmp
xcopy _site ..\tmp /s /e /y /q
git checkout gh-pages
```
