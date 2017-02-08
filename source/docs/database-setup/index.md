title: Database Setup
layout: docs_page
date: 2014-08-14 09:34:30
---

The `Spot\Locator` object is the main point of access to Spot that you will
have to be able to access from everywhere you need to run queries or work with
your entities. It is responsible for loading mappers and managing configuration.
To create a Locator, you will need a `Spot\Config` object.

The `Spot\Config` object stores and references database connections by name.
Create a new instance of `Spot\Config` and add database connections with
DSN strings so Spot can establish a database connection, then create your
locator object:

## Creating a Config Object

```php
$cfg = new \Spot\Config();

// MySQL
$cfg->addConnection('mysql', 'mysql://user:password@localhost/database_name');
// Sqlite
$cfg->addConnection('sqlite', 'sqlite://path/to/database.sqlite');

$spot = new \Spot\Locator($cfg);
```

You can also use [DBAL-compatible configuration
arrays](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/configuration.html)
instead of DSN strings if you prefer:

```php
$cfg->addConnection('mysql', [
    'dbname' => 'mydb',
    'user' => 'user',
    'password' => 'secret',
    'host' => 'localhost',
    'driver' => 'pdo_mysql',
]);
```
