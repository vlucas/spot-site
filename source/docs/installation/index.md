title: Installation
layout: docs_page
date: 2014-08-14 09:34:30
---

Spot uses [Composer](http://getcomposer.org) to install and update:

```shell
php composer.phar require vlucas/spot2 ~2.0
```

The Spot source code is available [on GitHub](https://github.com/vlucas/spot2),
and can be cloned into your project via git, or [downloaded directly as a ZIP
archive](https://github.com/vlucas/spot2/archive/master.zip).

### Installing Composer

If you don't have composer installed, you can do so with the command:

```shell
curl -s http://getcomposer.org/installer | php
```

### Manual Installation

If you would rather install Spot manually, just open up your `composer.json`
file, and add `vlucas/spot2` to the `require` section:

```shell
{
    "require": {
        "vlucas/spot2": "~2.0"
    }
}
```

### Autoloading

If you are already using Composer to manage your other dependencies, you have
nothing more to do - Spot uses Composer's autoloader, so all of Spot's classes
are available to you now.

If you haven't already setup Composer's autoloader, require it in your project:

```
require 'vendor/autoload.php';
```

[Composer's basic usage guide](https://getcomposer.org/doc/01-basic-usage.md)
is very useful if you need further help with composer.
