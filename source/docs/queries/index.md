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

Since a `Spot\Query` object is returned, conditions and other statements
can be chained in any way or order you want. The query will be
lazy-executed on interation or `count`, or manually by ending the chain with a
call to `execute()`.

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

### Joins

Joins are currently not enabled by Spot's query builder. The Doctine DBAL query
builder does provide full support for them, so they may be enabled in the
future.

### Custom Queries

While ORMs like Spot are very nice to use, if you need to do complex queries,
it's best to just use custom queries with the SQL you know and love.

Spot provides a `query` method that allows you to run custom SQL, and load the
results into a normal collection of entity objects. This way, you can easily run
custom SQL queries with all the same ease of use and convenience as the
built-in finder methods and you won't have to do any special handling.

#### Using Custom SQL

```php
$posts = $mapper->query("SELECT * FROM posts WHERE id = 1");
```

#### Using Query Parameters

```php
$posts = $mapper->query("SELECT * FROM posts WHERE id = ?", [1]);
```

#### Using Named Placeholders

```php
$posts = $mapper->query("SELECT * FROM posts WHERE id = :id", ['id' => 1]);
```

<div class="callout info">
  Spot will load ALL returned columns on the target entity from the query
  you run. So if you perform a JOIN or get more data than the target entity
  normally has, it will just be loaded on the target entity, and no attempt will
  be made to map the data to other entities or to filter it based on only the
  defined fields.
</div>

