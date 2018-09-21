# UNLcms 2
Drupal 8 installation at the University of Nebraska–Lincoln

# Installation

1. Install Composer (https://getcomposer.org/download/) and Drush version 8 (http://docs.drush.org/en/master/install/). Latest Drush tested and verified is 8.1.11.

  -  Production uses http://www.imagemagick.org/script/download.php. You can optionally skip installing this and switch to the core GD2 toolkit at _admin/config/media/image-toolkit_.

2. Clone this project and run from this project's root:
  ```
  cp .htaccess.sample .htaccess
  cp sites/default/default.settings.php sites/default/settings.php
  mkdir sites/default/files
  sudo chown _www sites/default/files
  sudo chmod 777 sites/default/settings.php
  sudo chmod 777 sites/default/files
  ```
  
3. If installing in a directory such as http://localhost/workspace/UNL-CMS-2 edit .htaccess and change
  
  `# RewriteBase /` to `RewriteBase /workspace/UNL-CMS-2`

4. Run
  ```
  composer install
  ```

5. Do standard Drupal install:
  * Navigate to /index.php in the browser
  * Choose "Configuration installer" for the "installation profile"
  * On "Configure site" set "Username" to "admin" and set "Email address" to a personal address so it doesn't conflict with your UNL email

6. Run:
  ```
  drush entity-updates
  sudo chmod 755 sites/default/settings.php
  sudo chmod 755 sites/default/files/
  ```

7. Install the UNLedu Framework (https://github.com/unl/wdntemplates) separately and create a symlink to its 'wdn' directory.
  * For example, if you installed the wdntemplates project in /Library/WebServer/Documents then you would run this from your UNL-CMS-2 project root:

  ```
  ln -s /Library/WebServer/Documents/wdntemplates/wdn wdn
  ```

8. Run:
  ```
  drush user-create jdoe2
  drush user-add-role "administrator" jdoe2
  ```
  The reason we create an admin user first, then create a second account is that the first user in a Drupal installation has special permisisons. We want to operate without that complexity. See https://www.drupal.org/node/540008

9. Enter the domain of your dev machine at admin/config/system/group_subdomain

10. That's it for installation. Instructions below are for additional site maintenance and updating tasks.

# Update core

  * Run `composer update drupal/core --with-dependencies`
  * Replace files (such as update.php) except for
    - .htaccess.sample (and your site's .htaccess)
    - index.php
    - composer.json
    - composer.lock
    - robots.txt
    - sites/ directory including sites/default/default.settings.php
  * Manually update the above files with the latest changes
  * Run `drush updatedb`

# Adding a module

  * Example for adding the IMCE module: `composer require drupal/imce`
  * Enable the module in the UI and configure
  * Export the configuration and commit using "Managing configuration" below
  
# Updating a module

  * Run command to update a module `composer update vendor/module`. For example: `composer update unlcms/unl_cas` as this will only update the specific module and nothing else.
  * Make edits to the configuration in the Drupal UI if needed
  * Follow "Managing configuration" below
  * Once the module update is pushed to production, run update.php there

# Managing configuration

  * Requires Drush 8+: http://docs.drush.org/en/master/install/
  * `git pull` so you have the latest /config files
  * Make sure your local dev site is using the latest config by running this from the site root `drush config-import`
  * Make configuration changes on a local dev site and run `drush config-export`
  * Commit changes to /config dir
  * Do a pull request
  
# Production deployment

  * `composer install --no-dev`
  * `drush updb`
  * `drush config-import --partial`  IMPORTANT: Must use the '--partial' flag so that existing group menus and webforms are not deleted.
  * `drush cr`

# Local devlopment

  * Disable Drupal 8 caching during development: https://www.drupal.org/node/2598914
  * Enable Twig debugging and disable caching: https://www.drupal.org/node/1903374

# Useful drush commands

  * Cache rebuild: `drush cr`
