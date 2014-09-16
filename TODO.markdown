* Auto-publish from travis

  Poor man's version:

      $ git checkout gh-pages
      $ cp _site/* .
      $ git add --all
      $ git commit -m "Update site"
      $ git push
      $ git checkout master

* In the Rakefile, read the work products from the wordlist.json just like we do in the template and create separate rules for html and txt products
