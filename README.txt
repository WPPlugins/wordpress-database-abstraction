=== Plugin Name ===
Contributors: omniti
Tags: database abstraction, mssql, pdo, SQL Server, sqlsrv, pdo_mysql, pdo_sqlsrv, mysqli, database
Requires at least: 3.0
Tested up to: 3.3.1
Stable tag: 1.1.4

This plugin provides db access abstraction and SQL dialect abstraction for SQL Server.
It is an mu (Must-Use) plugin AND also a db.php drop-in.

== Description ==

WP Database Abstraction is a plugin to make it possible to run WP on top of MS
SQL Server or Azure and provides two features, database access abstraction and SQL
dialect abstraction. This plugin cannot be installed or used as a regular
plugin, it must be in the "mu-plugins" directory (must use plugins) and in
addition to the plugin it contains a "drop-in" to hook into WordPress Database
functionality.

Database Access Abstraction is the way you connect to the database through PHP.
This plugin allows mysql, mysqli, pdo, sqlsrv or mssql extensions to be used.  PDO has support
for mssql, dblib, sqlsrv and mysql drivers.  This allows
you to choose the way your WordPress installation connects to your database.  You
can use the plugin and still use a Mysql Database, which is perfect if your hosting
provider does not make the mysql extension available.  The flexible structure of the
plugin means that dropping in additional drivers is easy.

SQL dialect abstraction means translating from the dialect understood by Mysql
to other dialects.  Currently only translation layers for T-SQL (used by Azure and SQL Server)
are provided.  However this is an open source project and additional translation layers
could be added.

For help and support please see the sourceforge project
http://sourceforge.net/projects/wp-sqlsrv/

For information and tutorials please visit our blog
http://wordpress.visitmix.com/

== Installation ==

Before you begin you will need a properly configuration server and PHP installation.
For help with IIS you can visit http://php.iis.net/ and ask questions in the forums.
You will also need a working database extension.  Check your phpinfo page to verify:

To use mysql you will need one of the following php extensions enabled:

1. mysql
1. mysqli
1. pdo and pdo_mysql driver

To use sql server you will need one of the following php extensions enabled:

1. sqlsrv
1. pdo and pdo_sqlsrv driver
1. mssql (non-windows environments with freetds)
1. pdo and pdo_dblib (non-windows environments with freetds)

You can get more information and support for sql server and the sqlsrv extension
at http://social.technet.microsoft.com/Forums/en-US/sqldriverforphp/threads

For a new WordPress install - you cannot install WordPress using SQL Server until
the plugin and dropin are in place.

1. Download wordpress, unzip the package and put the file in place.
1. Download the plugin package.
1. Upload wp-db-abstraction.php and the wp-db-abstraction directory to wp-content/mu-plugins.  This should be parallel to your regular plugins directory.  If the mu-plugins directory does not exist, you must create it.
1. Put the db.php file from inside the wp-db-abstraction.php directory to wp-content/db.php
1. Visit $your_wordpress_url/wp-content/mu-plugins/wp-db-abstraction/setup-config.php to generate your wp-config.php file
1. Install WordPress normally

For a Wordpress install already using the plugin:

1. Download the plugin package.
1. Upload wp-db-abstraction.php and the wp-db-abstraction directory to wp-content/mu-plugins replacing the existing files
1. Put the db.php file from inside the wp-db-abstraction directory to wp-content/db.php

If you are using or intend to use plugins that also use a db.php dropin.
You MUST make sure the only db.php file in your wp-content directory is the one used by wp database abstraction.
Rename the db.php files from other plugins.  For example, if you are using a plugin named foobar, rename the
db.php file from foobar to db-foobar.php and put it in the wp-content directory parallel to the db.php file for
wp-db-abstraction.  DO NOT OVERWRITE the db.php file that is already present.  Then
add

include 'db-foobar.php';

to the bottom of the db.php file (there is an example in that file, uncomment it and change the name
to the name your renamed the other db.php file to)

For a Wordpress Install migrating from MySQL to SQL Server

1. Log into your current site and export your data as an xml file
1. Disable all your current plugins
1. Back up your existing config.php file
1. Download the plugin package.
1. Upload wp-db-abstraction.php and the wp-db-abstraction directory to wp-content/mu-plugins.  This should be parallel to your regular plugins directory.  If the mu-plugins directory does not exist, you must create it.
1. Put the db.php file from inside the wp-db-abstraction.php directory to wp-content/db.php
1. Visit $your_wordpress_url/wp-content/mu-plugins/wp-db-abstraction/setup-config.php to generate your wp-config.php file
1. Install WordPress
1. Log into the newly installed system and import your data
1. Reinstall and test your plugins

If you are using or intend to use plugins that also use a db.php dropin.
You MUST make sure the only db.php file in your wp-content directory is the one used by wp database abstraction.
Rename the db.php files from other plugins.  For example, if you are using a plugin named foobar, rename the
db.php file from foobar to db-foobar.php and put it in the wp-content directory parallel to the db.php file for
wp-db-abstraction.  DO NOT OVERWRITE the db.php file that is already present.  Then
add

include 'db-foobar.php';

to the bottom of the db.php file (there is an example in that file, uncomment it and change the name
to the name your renamed the other db.php file to)

== Frequently Asked Questions ==

= How do I create a wp-config.php file? =

For new installs - we package our own wp-config.php creator.  The creation url will be at
$your_wordpress_url/wp-content/mu-plugins/wp-db-abstraction/setup-config.php  The original
setup-config.php WILL be redirected after the second step if db.php is in the right place

= My themes and images don't show up when using Multisite with IIS =
The rewrite rules supplied by wordpress for networking are incorrect for IIS7 and Url Rewrite

change your web.config file and replace the rewrite rule for #5 with the one below
`<rule name="WordPress Rule 5" stopProcessing="true">
  <match url="^[_0-9a-zA-Z-]+/(wp-(content|admin|includes).*)" ignoreCase="false" />
  <action type="Rewrite" url="{R:1}" />
</rule>`


= Why is collation important when using SQL Server? How do I change the SQL Server Collation used? =

By default, the SQL Server collation used by this plugin is database_default.  This means
that the default collation defined by the SQL Server install will dictate the collation
used when creating the sql server tables.

If you need to use a different collation for your installation, you may define DB_COLLATE
in your wp-config.php file.  For example, defining it to Cyrillic_General_BIN.  Note that this
will only affect new installations of the plugin.

If you want to change the way an individual table is collated you may use the following example syntax:

`ALTER TABLE wp_posts ALTER COLUMN post_content varchar(max)COLLATE Latin1_General_CI_AS`

== Changelog ==

= 1.1.4 =
* add all reserved words for sql server - plugins might use them
* fixed limit regex to catch queries with ; at the end
* fix for inserting NULL into identity columns

= 1.1.3 =
* fixed ording for items in archives
* added extra space before N prefix in strings to fix bad concatenation issues

= 1.1.2 =
* packaged correct example fields_map file updated to a base 3.2.1 install
* blocked install as anything but mu-plugin
* fixed error messages for attempted regular plugin install
* fixed html issues with error page
* field maps parsed_types information cache can have a location defined in wp-config.php
* on multisite installs, each site has it's own database metadata cache file
* added error if PHP database extension desired is not loaded to avoid confusing db can't connect errors

= 1.1.1 =
* additional help for installation, upgrading, and migrating
* packaged example fields_map file updated to a base 3.2.1 install
* translation uses N prefix for unicode data
* DB_COLLATE obeyed properly for table creation
* better error handling and descriptions for plugin, installation and upgrade issues

= 1.1.0 =
* New method of stripping out strings before translation
* drivers extend sql translation that extends wpdb
* mysqli driver added
* Drivers cleaned up and code simplified
* Issues with sqlsrv and datetime objects, and with integrated windows auth are now fixed
* Some translation fixes for various plugins
* API ping is now cached
* Error if fields_map file cannot be written to
* Packaged example fields_map file for installations that failed

= 1.0.1 =
* Fix for USING join for categories
* mysql driver now simply extends wpdb class
* pdo driver now provides connection exception information when WP_DEBUG is on
* Added warning for windows users with ntwdblib and mssql to config setup

= 1.0.0 =
 * Changed from patch to plugin architecture.
 * Incorporated several fixes from sourceforge forums in T-SQL dialect translations

== Notes ==

= Known Limitations =
 * This plugin must be in the "mu-plugins" directory.  The db.php file "drop-in" must be in the wp-content directory.  Normal plugin installation will not work.
 * Auto-update does not work for must-use plugins
 * Some plugins and themes that do not use the WP abstraction layer will break.  This is contrary to WordPress API guidelines, contact the theme or plugin author.
 * Some plugins and themes may use queries that need additional translations added.  Please report any you find in the sourceforge forums so the translation files can be updated. Please turn on query logging in wp-config.php and include your logs with the translation fix request.
 * Other plugins may also use a db.php drop-in.  You can use both plugins, but it will requiring renaming files and adding lines
   to db.php from the plugin.  See the Installation section for more details.
 * Other plugins that use db.php and extend the db class with custom behavior will break.

= To Do =
 * Add additional translations for PostgreSQL and Sqlite
 * Add additional drivers - pdo_sqlite, sqlite, sqlite3, pgsql, pdo_pgsql, odbc, pdo_odbc
 * Plugin specific autoupdater to mimic upgrade behavior of regular plugins
 * Administration area with the ability to view, alter, edit configuration and help with debugging issues
 * Make db.php smart enough to pick up any files prefixed with db- in the wp-content directory to help with other plugins with "drop-ins"
 * Proper error handling (no more @symbols) in database driver classes
