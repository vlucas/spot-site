title: Queries With Spot
layout: docs_page
date: 2014-08-14 09:34:30
---

Finders (Mapper)
----------------

All these finders require a <a href="/docs/mappers/">Mapper</a> instance. Mappers
are responsible for finding and updating individual entities.

The main finders used most are `all` to return a collection of entities,
and `first` or `get` to return a single entity matching the conditions.

### get([$primaryKey])

The `get` method accepts a primary key to load a record with:

```php
// Get Post with 'id' = 58
$mapper = $spot->mapper('Entity\Post');
$post = $mapper->get(58);
```

If there is no `Entity\Post` with the primary key provided, boolean `false`
will be returned.


### all()

Find all entities and return a `Spot\Entity\Collection` of loaded `Entity\Post`
objects.

```php
// Get ALL posts
$mapper = $spot->mapper('Entity\Post');
$posts = $mapper->all();
```

### where([conditions])

Find all entities that match the given conditions and return a
`Spot\Entity\Collection` of loaded `Entity\Post` objects.

```php
$mapper = $spot->mapper('Entity\Post');

// Where can be called directly from the mapper
$posts = $mapper->where(['status' => 1]);

// Or chained using the returned `Spot\Query` object - results identical to above
$posts = $mapper->all()->where(['status' => 1]);

// Or more explicitly using using `select`, which always returns a `Spot\Query` object
$posts = $mapper->select()->where(['status' => 1]);
```

<div class="callout info">
  A `Spot\Query` object is returned with all queries, which means additional
  conditions and other statements can be chained in any way or order you want.
  The query will be lazy-executed on interation or `count`, or manually by ending
  the chain with a call to `execute()`.
</div>

#### Conditional Variations

```php
# All posts with a 'published' status, descending by date_created
$posts = $mapper->all()
    ->where(['status' => 'published'])
    ->order(['date_created' => 'DESC']);

# All posts created before 3 days ago
$posts = $mapper->all()
    ->where(['date_created <' => new \DateTime('-3 days')]);

# Posts with 'id' of 1, 2, 5, 12, or 15 - Array value = automatic "IN" clause
$posts = $mapper->all()
    ->where(['id' => [1, 2, 5, 12, 15]]);
```

### first([conditions])

Find and return a single `Spot\Entity` object that matches the criteria.

```php
$post = $mapper->first(['title' => "Test Post"]);
```

Or `first` can be used on a previous query with `where` to fetch only the first
matching record.

```php
$post = $mapper->where(['title' => "Test Post"])->first();
```

A call to `first` will always execute the query immediately, and return either
a single loaded entity object, or boolean `false`.

### select()

To get an instance of the query builder (`Spot\Query`) with no conditions set
on it, use `select`.

```php
// Get instance of the query builder directly
$query = $mapper->select();
```

This is effectively the same thing as `all()`, but without any semantics attached.

