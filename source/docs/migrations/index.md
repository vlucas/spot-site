title: Migrations
layout: docs_page
date: 2014-08-14 09:34:30
---

Spot comes with a method for running migrations on Entities that will
automatically CREATE and ALTER tables based on the current Entity's `fields`
definition.

```php
$mapper = $spot->mapper('Entity\Post');
$mapper->migrate();
```

Your database should now have the `posts` table in it, with all the fields you
described in your `Post` entity.

<div class="callout danger">
  Please note that **re-naming columns is not supported** in migrations because
  there is no way for spot to know which column you renamed to what - Spot will
  see a new column that needs to be created, and a column that no longer exists
  and needs to be dropped. This could result in data loss during an
  auto-migration.
</div>

