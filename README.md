# Installation

  * Install Composer: https://getcomposer.org/download/
  
  * Clone this project and run from this project's root:
  ```
  cp .htaccess.drupal-default .htaccess
  cp sites/default/default.settings.php sites/default/settings.php
  mkdir sites/default/files
  sudo chown _www sites/default/files
  sudo chmod 777 sites/default/settings.php
  sudo chmod 777 sites/default/files
  ```

  * Edit sites/default/settings.php and add:

  $config_directories = array(
    CONFIG_SYNC_DIRECTORY => 'config/sync',
  );

  * Edit .htaccess and change
  ``` 
  # RewriteBase /
  ```
  to
  ``` 
  RewriteBase /workspace/UNL-CMS-2
  ```

  * Run
  ```
  composer update drupal/core --with-dependencies
  ```

  * Do standard Drupal install
    - Navigate to /index.php
    - Choose "Configuration installer" for the "installation profile"
    - On "Configure site" set "Username" to "admin" and set "Email address" to a personal address so it doesn't conflict with your UNL email

  * Run:
```
    sudo chmod 755 sites/default/settings.php
    sudo chmod 755 sites/default/files/
```

  * Install the UNLedu Framework (https://github.com/unl/wdntemplates) separately and create a symlink to its 'wdn' directory.
  ```
  ln -s /Library/WebServer/Documents/wdn wdn
  ```

  * Enable the unl_cas module and create a user with your My.UNL uid (such as jdoe2) and make yourself an administrator. Logout and log back in with your UNL credentials.  The reason we create an admin user first, then create a second account is that the first user in a Drupal installation has special permisisons. We want to operate without that complexity. See https://www.drupal.org/node/540008

  * That's it for installation. Instructions below are for additional site maintenance and updating tasks.

# Update core

  * Download latest version from drupal.org

  * Delete /core and /vendor

  * Replace all files except for
    - .htaccess
    - composer.json
    - composer.lock
    - robots.txt
    - /sites

  * Manually update the above files with the latest changes in the latest core version
    - For example, update "drupal/core" in composer.json with the new version

  * Run 
  ```
  composer update drupal/core --with-dependencies
  ```
  
  * Run update.php


# Managing configuration

  * Requires Drush 8+: http://docs.drush.org/en/master/install/

  * **git pull** so you have the latest /config version

  * Make sure your local dev site is using the latest config by running this from the site root $> drush config-import

  * Make configuration changes on a local dev site and run $> drush config-export

  * Commit changes to config dir
  
  * Do a pull request

  * On production run $> drush config-import

# Local devlopment

  * Enable Twig debugging and disable caching: https://www.drupal.org/node/1903374

# Useful drush commands

  * Cache rebuild: drush cr
