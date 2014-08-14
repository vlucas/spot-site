title: Documentation
layout: docs_page
date: 2014-08-14 09:34:30
---

Welcome to the documentation for Spot. Hopefully, these documentation pages
will help you get setup and going with your project quickly.

If you run into any issues along the way, you can report a bug or open a
discussion on [GitHub Issues](https://github.com/vlucas/spot2/issues)

## What is Spot?

Spot is a simple and efficient DataMapper built on the [Doctrine
DBAL](http://www.doctrine-project.org/projects/dbal.html) (not to be confused
with [Doctrine ORM](http://www.doctrine-project.org/projects/orm.html), which
is also built on the DBAL).

Spot favors simplicity and explicitness over conventions, which makes it easy
to use with any database structure - especially legacy databases that may not
follow the same popular naming conventions that most other ORMs expect.

### No Magic

Spot has no "magical" behavior that will lead to confusion. Spot does not rely
on annotations, generated proxy objects, or other code generation. Spot also does
not make any assumptions about how your database is setup.

## Requirements

Spot requires a modern version of [PHP](http://php.net) (5.4+)

