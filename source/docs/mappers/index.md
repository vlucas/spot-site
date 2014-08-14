title: Working With Mappers
layout: docs_page
date: 2014-08-14 09:34:30
---

Since Spot follows the [DataMapper design
pattern](http://martinfowler.com/eaaCatalog/dataMapper.html), you will need a
mapper instance for working with object Entities and database tables. You can
get a mapper instance from the `Spot\Locator` object's `mapper` method by
providing the fully qualified entity namespace + class name:

```php
$postMapper = $spot->mapper('Entity\Post');
```

Mappers only work with one entity type, so you will need one mapper per entity
class you work with (i.e. to save an Entity\Post, you will need the appropriate
mapper, and to save an Entity\Comment, you will need a comment mapper, not the
same post mapper. Relations will automatically be loaded and handled by their
corresponding mapper by Spot.

<div class="callout info">
  **You do NOT have to create a mapper for each entity** unless you need
  custom finder methods or other custom logic. If there is no entity-specific
  mapper for the entity you want, Spot will load the generic mapper for you and
  return it.
</div>

Using Custom Mappers
--------------------

Although you do not have to create a mapper for each entity, sometimes it is
nice to create one if you have a lot of custom finder methods, or want a better
place to contain the logic of building all the queries you need.

Just specify the full mapper class name in your entity:
```php
namespace Entity;

class Post extends \Spot\Entity
{
    protected static $mapper = 'Entity\Mapper\Post';

    // ... snip ...
}
```

And then create your mapper:
```php
namespace Entity\Mapper;

use Spot\Mapper;

class Post extends Mapper
{
    /**
     * Get 10 most recent posts for display on the sidebar
     *
     * @return \Spot\Query
     */
    public function mostRecentPostsForSidebar()
    {
        return $this->where(['status' => 'active'])
            ->order(['date_created' => 'DESC'])
            ->limit(10);
    }
}
```

Then when you load the mapper like normal, Spot will see the custom
`Entity\Post::$mapper` you defined, and load that instead of the generic one,
allowing you to call your custom method:

```php
$mapper = $spot->mapper('Entity\Post');
$sidebarPosts = $mapper->mostRecentPostsForSidebar();
```
