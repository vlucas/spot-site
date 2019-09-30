title: Basic CRUD Operations
layout: docs_page
date: 2014-08-19 09:20:14
---

There are four main save methods on Spot's mapper: `insert`, `create`, `save`,
and `update`. There are some basic differences between them, so this page
will address each one.

## build([array $data])

Build an entity object with the provided array data.

```php
// Returns a new loaded Entity\User object
$entity = $mapper->build([
    'name' => 'Chester Tester',
    'email' => 'chester@example.com'
]);
```
**Build will create the Entity object for you, but it will not save it**. If
you also wish to save the entity object, you must pass it to one of the save
methods:

```php
$mapper->save($entity);
```

## create([array $data])

Create will both **build** and **insert** the entity. The result will be a
fully-loaded entity object that has already been saved.

```php
// Returns a successfully saved, loaded Entity\User object... or throws an exception
$entity = $mapper->create([
    'name' => 'Chester Tester',
    'email' => 'chester@example.com'
]);
```

<div class="callout info">
  <h4>Expect an Entity... or Else!</h4>
  Unlike `insert` and `save`, the `create` method **will throw an exception if
  a record cannot be inserted**. This is because since it only accepts a data
  array, a boolean false on failure would not allow the user to see any
  validation errors if present. So instead, the `create` method will always
  return either a successfully inserted Entity object, or will throw an
  exception. This makes the `create` method a great choice for database seed
  files and other setup scripts.
</div>

## insert([$data])

The `insert` method will create a new row in the database with the provided
data (either an array of data, or a `Spot\Entity` object), and will return
either the inserted record's primary key, or boolean false.

### Data Array

Using a data array is perhaps the easiest way to use `insert`:

```php
// Insert and return record primary key, or boolean false
$result = $mapper->insert([
    'name' => 'Chester Tester',
    'email' => 'chester@example.com'
]);

// Fetch inserted record by ID
if ($result) {
    $entity = $mapper->get($result);
}
```

### Entity Object

You can also pass a `Spot\Entity` object to `insert`:

```php
// Insert and return record primary key, or boolean false

$entity = $mapper->build([
    'name' => 'Chester Tester',
    'email' => 'chester@example.com'
]);
$result = $mapper->insert($entity);

// Fetch inserted record by ID
if ($result) {
    // Do something with $entity
}
```

## save($entity)

The `save` method is a convenience method that handles both inserting and
updating an entity object. Save will call either `insert` or `update`,
depending on whether the Entity has already been saved (it will check
`$entity->isNew()` internally).

```php
// Save and return record primary key, or boolean false

$entity = $mapper->build([
    'name' => 'Chester Tester',
    'email' => 'chester@example.com'
]);
$result = $mapper->save($entity);

// Fetch inserted record by ID
if ($result) {
    // Update with another call to save
    $entity->name = 'Lester Tester';
    $mapper->save($entity);
}
```

## update($entity)

The `update` method will update an existing entity.

```php
// Find and update an entity
$entity = $mapper->first(['email' => 'chester@example.com']);
if ($entity) {
    $entity->name = 'Lester Tester';
    $mapper->update($entity);
}
```

## Saving Relations

In order to save relations on the entity, you need to call the `save` method (or `update` or `insert`) with an additional `$options` parameter, an array with the `relations` key as `true`:

```php
$mapper->save($entity, ["relations" => true]);
```

**Currently, Spot does not automatically save any related entities on save**. For
instance, if you have a `Book` entity that hasOne `Author`, and you make edits
to the author object, you must save the author object itself directly with the
author's mapper. Saving the book object will not automatically detect and save
changes on any related objects, even if the related objects are already loaded.

