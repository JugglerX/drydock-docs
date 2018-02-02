+++
title = "Install With DrupalVM Locally"
description = ""
weight = 1
+++

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

Modify the `config.yml` file located inside the `vm` folder. You will need to change the `local_path` to point to the project files on your computer.

```
vagrant_synced_folders:
  - local_path: ~/work/web/drydock-business // enter the full path to your project directory
```

### Run Vagrant

From the `vm` folder you can now run Vagrant.

```bash
vagrant up
```
{{% notice note %}}
If for any reason the deployment fails, running `vagrant provision` will often fix the issue.
{{% /notice %}}


### Visit The Website

Visit _drydock-business.local_ in the browser! You should see the default Drupal 8 website.

___

## Import Data
**Choose between two methods** to complete the Drydock installation. This will enable the theme, but more importantly import content types, blocks, views and content.

### Method 1: SQL Import

Importing the SQL file is the fastest and easiest method to configure the site and achieve the exact look and feel as the demo. Note you need to run MySQL from within the Vagrant box.

From the `vm` directory ssh into Vagrant

``` 
vagrant ssh
```

Navigate to the folder which includes the drydock.sql file.

```
cd /var/www/drupalvm/drupal/sql
```

Import the database. DrupalVM has already created a database called `drupal`.

```
mysql -uroot -proot drupal < drydock.sql
```

Finally, clear the Drupal cache. Executing Drush commands must be done from the `/web` folder.


```
cd ..
cd web
drush cr all
```

Exit the SSH session.

```
exit
```

Visit `drydock-business.local` in the browser. The website should be fully provisioned and look like the demo website. This ends the installation process on your local machine.



### Method 2: Configuration Syncronization

Method 2 uses the new Drupal 8 configuration management system. Site configuration data that is stored in the database, can now be imported and exported as code. The potential is huge, but there are still many quirks to the process. We recommend you read the 
[Drupal configuration management docs](https://www.drupal.org/docs/8/configuration-management) for an overview.

For the Drydock theme, we've included the site configuration files. So instead of importing SQL you can import the configuration files to provision the fully completed site.


From the `vm` directory ssh into Vagrant

``` 
vagrant ssh
```

Drush commands must be executed from within the `/web` folder. 

```
cd /var/www/drupalvm/drupal/web
```

Install and enable the Drydock theme.

```
drush en drydock -y
drush config-set system.theme default drydock -y
```

Set the default homepage node. 

``` 
drush config-set system.site page.front /node/1 -y
```

Sync the configurations UUID.

One of the quirks of configuration management is that the UUID of your Drupal site must match the UUID in the configuration files. When you install a fresh copy of Drupal locally, and then recieve configuration files that have been exported from a different Drupal installation (different databases) you will need to sync the UUID.

```
drupal config:export:single --name=system.site --no-interaction
drupal config:export:single --name=shortcut.set.default --remove-uuid --remove-config-hash --no-interaction
```

Import the configuration

```
drush config-import -y
```

Visit `drydock-business.local` in the browser. The website should be fully provisioned and look like the demo website. This ends the installation process on your local machine.

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




