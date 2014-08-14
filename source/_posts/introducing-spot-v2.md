title: Introducing Spot ORM v2
date: 2014-08-14 13:46:35
tags:
---

Welcome to the new website for the completely re-coded Spot v2. The new version
of Spot has been completely re-coded and based on a solid foundation &ndash;
[Doctrine DBAL](http://www.doctrine-project.org/projects/dbal.html). The DBAL
powers all of Spot's queries and auto-migrations using schema reflection, and
removes the need for Spot to have its own database drivers.

Building on top of the DBAL allows me to focus on making Spot the best ORM it
can possibly be instead of worrying about database support and poorly designed
database adapters that were in the previous version of Spot. This has already
lead to some noticeable feature improvements, like support for eager-loading
_HasManyThrough_ relations &ndash; a weak point of poor architecture in the
previous version of Spot [[v1
issue](https://github.com/vlucas/Spot/issues/68)]. Users of Spot v2 can also
enjoy more robust type support thanks to the DBAL, like native `json` columns
in PostgreSQL, among many other benefits.

### History of Spot

The Spot ORM project [started in
2010](https://github.com/vlucas/Spot/commits/master?page=7), as a completely
re-imagined, re-coded, and re-named version of _phpDataMapper_ - a project that
started [in 2008](http://vancelucas.com/blog/introducing-php-datamapper/). The
name was changed from _phpDataMapper_ to _Spot_ after reviewing the [PHP
License](http://php.net/license/) that asks projects not to use _PHP_ in their
names.

### Keeping The Good Parts

The first version of Spot, and its predecessor, phpDataMapper had some really
good syntax building queries and defining fields. These features have been kept
in Spot v2, so porting over your code from Spot v1 should be fairly easy.

Spot's general philosophy favoring simplicity and clarity, and against "magic"
behavior and excessive configuration (like XML, INI, and YAML) has also been
preserved.

### Looking Forward

I have learned a lot over the years, and have incorporated all of it into the
new Spot v2. Perhaps the most important (and beneficial) lesson is to stand on
the shoulders of giants. By using Doctrine DBAL, I now I have an entire team of
dedicated people to worrying about database adapters and portability, which not
only removes a significant amount of work from my plate, but allows me to focus
on making Spot better. It also gives me a much greater degree of confidence
knowing the foundation is solid, and allows me to more seriously consider Spot
as a good alternative to larger projects like [Doctrine
ORM](http://www.doctrine-project.org/projects/orm.html) and
[Propel](http://propelorm.org/). Spot's future is bright!

