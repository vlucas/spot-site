title: Working With Entities
layout: docs_page
date: 2014-08-14 09:34:30
---

Entity classes can be named and namespaced however you want to set them
up within your project structure. For the following examples, the
Entities will just be prefixed with an `Entity` namespace for easy psr-0
compliant autoloading.

## Defining An Entity

The minimal required definition of an entity defines the `$table` and the `fields`, and extends from `Spot\Entity`:

```php
<?php
namespace Entity;

class Post extends \Spot\Entity
{
    protected static $table = 'posts';

    public static function fields()
    {
        return [
            'id'           => ['type' => 'integer', 'primary' => true, 'autoincrement' => true],
            'title'        => ['type' => 'string', 'required' => true],
            'body'         => ['type' => 'text', 'required' => true],
            'status'       => ['type' => 'integer', 'default' => 0, 'index' => true],
            'author_id'    => ['type' => 'integer', 'required' => true],
            'date_created' => ['type' => 'datetime', 'value' => new \DateTime()]
        ];
    }
}
```

<div class="callout info">
  <h4>Field Options</h4>
  
  <p>All of the <strong><a
  href="http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/schema-representation.html#column">DBAL
  schema column options</a></strong> can be used for field attributes, and will be passed to DBAL when
  creating the table structure through <a
  href="/docs/migrations/">migrations</a>. Spot adds some additional field attributes like `required` and `value` as well.</p>
</div>

### Primary Keys

In the above "Post" entity, the primary key is defined on the `id` field
with a `'primary' => true` attribute on the field definition. If the primary
key is also an auto-increment column, you must also add a `'autoincrement' =>
true` attribute.

The full required definition looks like this:
```php
'id' => ['type' => 'integer', 'primary' => true, 'autoincrement' => true]
```

### Indexes

An index can be defined on an entity field by adding the `'index' => true`
attribute.

```php
'status' => ['type' => 'integer', 'index' => true]
```

Many times, it is desirable to create a **compound index** with more than one
column. In Spot, simply give the `index` attribute a name that is shared with
another column, and a compound index will be created using all columns with the
same index name.

```php
'lat' => ['type' => 'decimal', 'index' => 'geolocation']
'lng' => ['type' => 'decimal', 'index' => 'geolocation']
```

### Unique Columns

Similar to defining an index, you must add a `'unique' => true` to specify a
single field as requiring a unique value:

```php
'email' => ['type' => 'string', 'required' => true, 'unique' => true]
```

And also just like normal indexes, **compound unique indexes** can be created
by using a shared name:

```php
'email'    => ['type' => 'string', 'required' => true, 'unique' => 'user_email']
'username' => ['type' => 'string', 'required' => true, 'unique' => 'user_email']
```

<div class="callout info">
  To detect duplicate values and return a friendly error message instead of an
  exception from the database engine, Spot will automatically attempt to find
  existing entities with duplicate values you are passing when a field is marked
  as `unique`. This happens in the `validate` method of `Spot\Mapper` that is
  run before every insert and update operation. This results in _one extra query
  on insert or update_ for each unique column or compound column group.
</div>

### Required Fields

A required field can be enforced on save, create, and update using a
`'required' => true` attribute.

```php
'email' => ['type' => 'string', 'required' => true]
```
