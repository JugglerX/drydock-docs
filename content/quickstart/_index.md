+++
title = "Quickstart"
description = ""
+++

# Drydock Business - Drupal 8 Theme

### Install Drupal Locally
- [Install DrupalVM](docs/install-drupalvm.md)
- [Install MAMP](docs/install-mamp.md)

### Import Theme Configuration & Content
- [Import SQL](docs/import-sql.md)
- [Import Configuration](docs/import-configuration.md)

### Install Theme
- [Install Theme](docs/install-theme.md)

### Deployment
- [Setup Github](docs/install-github.md)
- [Deploy to Pantheon](docs/deploy-pantheon.md)



# Install with DrupalVM

## Local Prerequisites

* Install Composer Globally https://getcomposer.org/doc/00-intro.md#globally
* Install Vagrant https://www.vagrantup.com/downloads.html
* Install Virtual Box https://www.virtualbox.org/wiki/Downloads

## Install Drupal

### Unzip The Project

Unzip the Drydock project on your computer into a folder called `drydock-business`. You should end up with a folder structure like below.  

```
/drydock-business
 - /drupal
 - /vm
```

### Configure DrupalVM

Modify the `config.yml` file located inside the `vm` folder. 

```
vagrant_synced_folders:
  - local_path: ~/work/web/drydock-business // enter the full path to your project directory
```

### Run Vagrant
Deploy DrupalVM using Vagrant. Note you must be inside the `vm` folder.

```
vagrant up
```

If for any reason the deployment fails, running `vagrant provision` will often fix the issue.

### Visit The Website

Visit _drydock-business.local_ in the browser! You should see the default Drupal 8 website.




## Import Data
**Choose between two methods** to complete the Drydock installation. This will enable the theme, but more importantly import content types, blocks, views and content.

### Method 1: SQL Import

``` 
vagrant ssh
cd /var/www/drupalvm/drupal/sql
mysql -uroot -proot drupal < drydock.sql
cd ..
cd web
drush cr all
exit
```

### Method 2: Configuration Syncronization

```
vagrant ssh
cd /var/www/drupalvm/drupal/sql
drush en drydock -y
cp modules/custom/drydock_engage_setup/content/file/enis-yavuz-365453.jpg sites/default/files/2017-11/enis-yavuz-365453.jpg
drush config-set system.site page.front /node/1 -y
drush config-set system.theme default drydock -y
drupal config:export:single --name=system.site --no-interaction
drupal config:export:single --name=shortcut.set.default --remove-uuid --remove-config-hash --no-interaction
drush config-import -y
```

#### Generate Dummy Content

When the theme is first installed, it automatically creates default content for key pages including 'homepage, services, work, team, testimonials, about us, contact us'

To generate dummy content for each content type use `devel_generate` in the Drupal admin.

Or use drush on the command line.

```
drush generate-content 20 --types=service
drush generate-content 20 --types=work
drush generate-content 20 --types=team
drush generate-content 20 --types=testimonial
```

## Setup Github

Create a new Github repo on Github and then initalise a new Github repo in the /drydock-business folder.

```
cd drydock-business
git init
git add -A .
git commit -m "first install"
git remote add origin https://github.com/username/your-github-repo.git
git push origin master
```

## Deploy to Pantheon

Create a Pantheon website. You can use the dashboard at the Pantheon website or use the Terminus CLI.

```
terminus site:create my-project "My Project" "Drupal 8"
terminus connection:set my-project.dev git
```

Push the code to the Pantheon git repo. 

Make sure Pantheon is set to git transfer mode. 

```
// you can get the ssh url from the pantheon dashboard > connection info
git remote add pantheon ssh://ID@ID.drush.in:2222/~/repository.git

// This will overwrite any existing code on your Pantheon website
git push --force pantheon master
```

Upload the MySQL database to Pantheon

You can log into the Pantheon dashboard an upload the .sql file or via the command line.

```
vagrant ssh
cd /var/www/drupalvm/drupal
mkdir sql
cd sql
mysqldump -uroot -proot drupal > drupal.sql;
exit
```

Navigate to the sql folder and sync the database to Pantheon.

```
// you can get the Pantheon MySQL details from the pantheon dashboard > connection info > database:command line
mysql -u pantheon -p{random_password} 7 -h {pantheon_hostname} -P 27595 pantheon < drupal.sql
```

Upload files to Pantheon

1. Set Pantheon to SFTP mode and then using an FTP program copy the contents of the web/sites/default/files directory.

2. Zip the web/sites/default/files directory and upload this .zip file using the Pantheon dashboard.


Pantheon Extras

You may need to create a /private folder in web/sites/default/files



