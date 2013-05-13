Danpu - MySQL dump tool for PHP
=========

This branch contains a PHP 5.2 compatible legacy version of the library. See 2.0.0+ and the [master](https://github.com/gocom/danpu) branch for the up to date codebase.

[![Build Status](https://travis-ci.org/gocom/danpu.png?branch=1.0)](https://travis-ci.org/gocom/danpu)

Danpu is a dependency-free, cross-platform, portable PHP library for backing up MySQL databases. It has no hard dependencies, and is fit for use in restricted, shared-hosting environments. It requires nothing more than access to your database, PDO and a directory it can write the backup to. The script is optimized and has low memory-footprint, allowing it to handle even larger databases.

Installing
---------

Using [Composer](http://getcomposer.org):

    $ composer.phar require "rah/danpu 1.*"

v.1.x.x are PHP 5.2 compatible.

Usage
---------

To create a new backup and or import one, create a ```Dump``` instance containing your configuration options and feed it to one of the worker classes. The library is PHP 5.2.0 compatible, and uses PSR-0 compatible PEAR-style naming convention. All classes throw exceptions on failures.

### Taking a backup

Backups are created by the Rah_Danpu_Export class. The class exports the database to a SQL file. The backup is compressed if the target filename ends to .gz extension.

```php
$dump = new Rah_Danpu_Dump;
$dump
    ->file('/path/to/target/dump/file.sql.gz')
    ->dsn('mysql:dbname=database;host=localhost')
    ->user('username')
    ->pass('password')
    ->tmp('/tmp');

new Rah_Danpu_Export($dump);
```

### Importing a backup

Danpu can also import its own backups it has created. While the importer works with the backups it has made, it doesn't accept freely formatted SQL. The importer is pretty strict about formatting, and expects exactly the same format as generated by Danpu. It requires that values in statements are escaped properly, including newlines, queries should end to a semicolon and statements preferably should not wrap to multiple lines.

Backups are imported by the Rah_Danpu_Import class. It uncompresses any .gz files before importing.

```php
new Rah_Danpu_Import($dump);
```

Where the ```$dump``` would be an instance of Rah_Danpu_Dump.

Changelog
---------

### Version 1.3.0 - 2013/05/13

* Fixed: Errors in the given examples. The Rah_Danpu_Dump::temp() should be Rah_Danpu_Dump::tmp().
* Added: Rah_Danpu_Config class. Allows creating configs by extending.
* Added: Rah_Danpu_Dump now validates the given configuration option names.

### Version 1.2.0 - 2013/05/13

* Added: Database connection details are set using a DSN string. Deprecates Rah_Danpu_Dump::$db and Rah_Danpu_Dump::$host.

### Version 1.1.0 - 2013/05/13

* Added: Option to ignore database tables.

### Version 1.0.0 - 2013/05/11

* The initial legacy release.