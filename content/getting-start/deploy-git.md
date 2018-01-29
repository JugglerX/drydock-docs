+++
title = "Setup Github"
description = ""
weight = 3
+++

# Setup Github

Create a new Github repo on Github and then initalise a new Github repo in the /drupal folder.

```
cd drupal
git init
git add -A .
git commit -m "web and vendor directory from composer install"
git remote add origin https://github.com/JugglerX/your-github-repo.git
git push origin master
```
