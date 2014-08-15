title: Complex Queries
layout: docs_page
date: 2014-08-15 16:25:14
---

There are a number of conditional operations in Spot that can be used to build
more complex queries without obtuse query builder syntax or long namespaced
constants.

## A Query Example

```php
# All posts with a 'published' or 'draft' status, descending by date_created, limit 10
$posts = $mapper->where(['status' => 'published'])
    ->orWhere(['status' => 'draft'])
    ->order(['date_created' => 'DESC'])
    ->group(['id'])
    ->having(['id > 20'])
    ->limit(10, 20);
```
```sql
SELECT * FROM posts
WHERE status = 'published' OR WHERE status = 'draft'
GROUP BY id
ORDER BY date_created DESC
HAVING id > 20
LIMIT 10 OFFSET 20
```

## Accepted Query Values

Spot can use many different types of values in the query builder, and will
automatically do the right thing with them.

### Scalar Values
Scalar values like normal strings, integers, and floats will be handled normally:

```php
$posts = $mapper->all()->where(['status >=' => 3]);
```
```sql
SELECT * FROM posts WHERE status >= 3
```

### DateTime Objects

PHP's `DateTime` objects can be used as query values, and will be automatically
converted to the native database driver's required format.

```php
# All posts created before 3 days ago
$posts = $mapper->where(['date_created <' => new DateTime('-3 days')]);
```
```sql
SELECT * FROM posts WHERE date_created < '2014-08-12'
```

### Arrays
Array values passed to Spot's query builder will result in automatic "IN" clause.

```php
// Posts with 'id' of 1, 2, 5, 12, or 15
$posts = $mapper->all()->where(['id' => [1, 2, 5, 12, 15]]);
```
```sql
SELECT * FROM posts WHERE id IN(1, 2, 5, 12, 15)
```

### Null
If a value is `null`, Spot will use the proper SQL syntax:

```php
$posts = $mapper->all()->where(['status !=' => null]);
```
```sql
SELECT * FROM posts WHERE status IS NOT NULL
```

### Booleans
If a value is boolean `true` or `false`, Spot will use the proper SQL syntax
according to the database adapter you are using:

```php
$posts = $mapper->all()->where(['is_active' => true]);
```
```sql
-- PostgreSQL
SELECT * FROM posts WHERE is_active = 't';

-- MySQL
SELECT * FROM posts WHERE is_active = 1;
```

## Custom Queries

**There is no substitute for the power and expressiveness of SQL**. While ORMs
like Spot are very nice to use, if you need to do complex queries, it's best to
just use custom queries with the SQL you know and love. This is why Spot's
query builder is fairly simple compared to other ORMs, and doesn't support
things like subqueries, etc.

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

### Joins

Joins are currently not enabled by Spot's query builder. The Doctine DBAL query
builder does provide full support for them, so they may be enabled in the
future.

