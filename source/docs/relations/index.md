title: Relations
layout: docs_page
date: 2014-08-14 09:34:30
---

Relations are convenient ways to access related, parent, and child entities from
another loaded entity object. An example might be `$post->comments` to query for
all the comments related to the current `$post` object.

### Live Query Objects

All relations are returned as instances of relation classes that extend
`Spot\Relation\RelationAbstract`. This class holds a `Spot\Query` object
internally, and allows you to chain your own query modifications on it so you
can do custom things with relations, like ordering, adding more query
conditions, etc.

```php
$mapper->hasMany($entity, 'Entity\Comment', 'post_id')
    ->where(['status' => 'active'])
    ->order(['date_created' => 'ASC']);
```

All of these query modifications are held in a queue, and are run when the
relation is actually executed (on `count` or `foreach` iteration, or when
`execute` is explicitly called).

### Eager Loading

All relation types are lazy-loaded by default, and can be eager-loaded to
solve the N+1 query problem using the `with` method:

```php
$posts = $posts->all()->with('comments');
```

Multiple relations can be eager-loaded using an array:
```php
$posts = $posts->all()->with(['comments', 'tags']);
```

### Relation Types

Entity relation types are:

 * `HasOne`
 * `BelongsTo`
 * `HasMany`
 * `HasManyThrough`

### HasOne

HasOne is a relation where the *related object has a field which points to the
current object* - an example might be `User` has one `Profile`.

#### Method

```php
$mapper->hasOne(Entity $entity, $foreignEntity, $foreignKey)
```
 * `$entity` - The current entity instance
 * `$foreignEntity` - Name of the entity you want to load
 * `$foreignKey` - Field name on the `$foreignEntity` that matches up with the
   primary key of the current entity

#### Example

```php
namespace Entity;

class User extends \Spot\Entity
{
    protected static $table = 'users';

    public static function fields()
    {
        return [
            'id'           => ['type' => 'integer', 'autoincrement' => true, 'primary' => true],
            'username'     => ['type' => 'string', 'required' => true],
            'email'        => ['type' => 'string', 'required' => true],
            'status'       => ['type' => 'integer', 'default' => 0, 'index' => true],
            'date_created' => ['type' => 'datetime', 'value' => new \DateTime()]
        ];
    }

    public static function relations(Mapper $mapper, Entity $entity)
    {
        return [
            'profile' => $mapper->hasOne($entity, 'Entity\User\Profile', 'user_id')
        ];
    }
}
```

In this scenario, the `Entity\User\Profile` entity has a field named `user_id`
which the `Entity\User`'s `id` field as a value. Note that *no field exists on
this entity for this relation, but rather the related entity*.

### BelongsTo

BelongsTo is a relation where the *current object has a field which points to
the related object* - an example might be `Post` belongs to `User`.

#### Method

```php
$mapper->belongsTo(Entity $entity, $foreignEntity, $localKey)
```
 * `$entity` - The current entity instance
 * `$foreignEntity` - Name of the entity you want to load
 * `$localKey` - Field name on the current entity that matches up with the
   primary key of `$foreignEntity` (the one you want to load)

#### Example

```php
namespace Entity;

class Post extends \Spot\Entity
{
    protected static $table = 'posts';

    public static function fields()
    {
        return [
            'id'           => ['type' => 'integer', 'autoincrement' => true, 'primary' => true],
            'user_id'      => ['type' => 'integer', 'required' => true],
            'title'        => ['type' => 'string', 'required' => true],
            'body'         => ['type' => 'text', 'required' => true],
            'status'       => ['type' => 'integer', 'default' => 0, 'index' => true],
            'date_created' => ['type' => 'datetime', 'value' => new \DateTime()]
        ];
    }

    public static function relations(Mapper $mapper, Entity $entity)
    {
        return [
            'user' => $mapper->belongsTo($entity, 'Entity\User', 'user_id')
        ];
    }
}
```

In this scenario, the `Entity\Post` entity has a field named `user_id` which is
the `Entity\User`'s `id` field's value. Note that *the field exists on this
entity for this relation, but not on the related entity*.

### HasMany

HasMany is used where a single record relates to multiple other records - an
example might be `Post` has many `Comments`.

#### Method

```php
$mapper->hasMany(Entity $entity, $entityName, $foreignKey, $localValue = null)
```
 * `$entity` - The current entity instance
 * `$entityName` - Name of the entity you want to load a collection of
 * `$foreignKey` - Field name on the `$entityName` that matches up with the
   current entity's primary key

#### Example

We start by adding a `comments` relation to our `Post` object:
```php
namespace Entity;

class Post extends Spot\Entity
{
    protected static $table = 'posts';

    public static function fields()
    {
        return [
            'id'           => ['type' => 'integer', 'autoincrement' => true, 'primary' => true],
            'title'        => ['type' => 'string', 'required' => true],
            'body'         => ['type' => 'text', 'required' => true],
            'status'       => ['type' => 'integer', 'default' => 0, 'index' => true],
            'date_created' => ['type' => 'datetime', 'value' => new \DateTime()]
        ];
    }

    public static function relations(Mapper $mapper, Entity $entity)
    {
        return [
            'comments' => $mapper->hasMany($entity, 'Entity\Comment', 'post_id')->order(['date_created' => 'ASC']),
        ];
    }
}
```

And add a `Entity\Post\Comment` object with a 'belongsTo' relation back to the post:

```php
namespace Entity;

class Comment extends \Spot\Entity
{
    // ... snip ...

    public static function relations(\Spot\MapperInterface $mapper, \Spot\EntityInterface $entity)
    {
        return [
            'post' => $mapper->belongsTo($entity, 'Entity\Post', 'post_id')
        ];
    }
}
```

### HasManyThrough

HasManyThrough is used for many-to-many relationships. An good example is
tagging. A post has many tags, and a tag has many posts. This relation is
a bit more complex than the others, because a HasManyThrough requires a
join table and mapper.

#### Method
```php
$mapper->hasManyThrough(Entity $entity, string $hasManyEntity, string $throughEntity, string $selectField, string $whereField)
```
 * `$entity` - The current entity instance
 * `$hasManyEntity` - This is the target entity you want a collection of. In this case, we want a collection of `Entity\Tag` objects.
 * `$throughEntity` - Name of the entity we are going through to get what we want - In this case, `Entity\PostTag`.
 * `$selectField` - Name of the field on the `$throughEntity` that will select records by the primary key of `$hasManyEntity`.
 * `$whereField` - Name of the field on the `$throughEntity` to select records by the current entities' primary key (we have a post, so this will be the `Entity\PostTag->post_id` field).

#### Example

We need to add the `tags` relation to our `Post` entity, specifying query
conditions for both sides of the relation.

```php
namespace Entity;

class Post extends Spot\Entity
{
    protected static $table = 'posts';

    public static function fields()
    {
        return [
            'id'           => ['type' => 'integer', 'autoincrement' => true, 'primary' => true],
            'title'        => ['type' => 'string', 'required' => true],
            'body'         => ['type' => 'text', 'required' => true],
            'status'       => ['type' => 'integer', 'default' => 0, 'index' => true],
            'date_created' => ['type' => 'datetime', 'value' => new \DateTime()]
        ];
    }

    public static function relations(\Spot\MapperInterface $mapper, \Spot\EntityInterface $entity)
    {
        return [
            'tags' => $mapper->hasManyThrough($entity, 'Entity\Tag', 'Entity\PostTag', 'tag_id', 'post_id'),
        ];
    }
```

#### Explanation

The result we want is a collection of `Entity\Tag` objects where the id equals
the `post_tags.tag_id` column. We get this by going through the
`Entity\PostTags` entity, using the current loaded post id matching
`post_tags.post_id`.

