+++
title = "Install With MAMP Free Edition"
description = ""
weight = 2
+++

{{% notice note %}}
Tested with MAMP (Basic) 4.3 on Mac OSX Sierra 10.12
{{% /notice %}}

### Unzip Project Files

Unzip the Drydock project on your computer into a folder called `drydock-business`. You should end up with a folder structure like below.  

```
/drydock-business
 - /drupal
 	- /web
 - /vm
```

### Set MAMP Document Root

In MAMP the document root must be the set to the Drupal `web` folder.

![](/images/mamp-basic-docroot.png?classes=border,shadow)


### Install Drupal

Visit your local MAMP website `localhost:8888` and you should be presented with the Drupal Install screen.

Follow the prompts and install the site. 

The theme comes with a `settings.php` file which has the database settings preconfigured.

```php
// settings.local.php

  $databases['default']['default'] = array (
    'database' => 'drupal',
    'username' => 'root',
    'password' => 'root',
    'prefix' => '',
    'host' => 'localhost',
    'port' => '',
    'driver' => 'mysql',
  );
```

### Import SQL Database

Import the drydock.sql file that comes with the Drydock project. It is located inside the `/drupal/sql` folder.

![](/images/mamp-basic-import-sql-phpmyadmin.png?classes=border,shadow)

### Visit the site and clear cache

Visit `localhost:8888` - make sure to login to Drupal admin and clear the cache. After clearing cache you should the final site.


