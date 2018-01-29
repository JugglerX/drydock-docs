+++
title = "Deploy to Pantheon"
description = ""
weight = 4
+++

Create a Pantheon website. You can use the dashboard or create it from the CLI using Terminus.

```
terminus site:create my-project "My Project" "Drupal 8"
terminus connection:set my-project.dev git
```

Push the code to the Pantheon git repo.

```
// you can get the ssh url from the pantheon dashboard > connection info
git remote add pantheon ssh://ID@ID.drush.in:2222/~/repository.git
git push --force pantheon master
```

Upload the MySQL database to Pantheon

```
vagrant ssh
cd /var/www/drupalvm/drupal
mkdir sql
cd sql
mysqldump -uroot -proot drupal > drupal.sql;
exit
cd sql
mysql -u pantheon -p{random_password} 7 -h {pantheon_hostname} -P 27595 pantheon < drupal.sql
```


