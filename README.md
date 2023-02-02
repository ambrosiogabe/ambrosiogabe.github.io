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
xcopy ..\tmp .\ /s /e /y /q
git add .
git commit -m "Update built site"
git push -u origin gh-pages
git checkout master
git pull origin master
rm -r -fo ..\tmp
```
