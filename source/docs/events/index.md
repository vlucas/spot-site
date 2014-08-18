title: Events
layout: docs_page
date: 2014-08-14 09:34:30
---

Events in Spot are powered by the [Sabre
EventEmitter](https://github.com/fruux/sabre-event), and are exposed to your
entity via the `events` static method. This allows spot to load your custom
events when your entity's mapper is first loaded.


## Example Entity
```php
<?php
namespace Entity;

use Spot\Entity;
use Spot\Mapper;
use Spot\EventEmitter;

class Post extends \Spot\Entity
{
    protected static $table = 'posts';

    public static function fields()
    {
        return [
            'id'           => ['type' => 'integer', 'autoincrement' => true, 'primary' => true],
            'title'        => ['type' => 'string', 'required' => true],
            'body'         => ['type' => 'text', 'required' => true],
            'status'       => ['type' => 'integer', 'default' => 0, 'index' => true],
            'author_id'    => ['type' => 'integer', 'required' => true],
            'is_active'    => ['type' => 'boolean', 'index' => true],
            'date_created' => ['type' => 'datetime', 'value' => new \DateTime()]
        ];
    }

    public static function events(EventEmitter $eventEmitter)
    {
        $eventEmitter->on('beforeSave', function (Entity $entity, Mapper $mapper) {
            // Ensure certain statuses are not active
            if ($entity->status === 9) {
                $entity->is_active = false;
            }
        });
    }
}
```

<div class="callout info">
  In addition to the `on` method used here, the EventEmitter also has a `once`
  method that will ensure your callback is only executed one time.
</div>

## Events

### beforeSave
Called before _any_ save event through any method that saves an entity.

Returning `false` from your event callback will not save the entity.

### afterSave
Called after _any_ save event through any method that saves an entity.

### beforeInsert
Called _before_ a new entity is inserted via the `save`, `insert` or `create`
method.

Returning `false` from your event callback will not insert the entity.

### afterInsert
Called _after_ a new entity is inserted via the `save`, `insert` or `create` method.

### beforeUpdate
Called _before_ a new entity is updated via the `save` or `update` method.

Returning `false` from your event callback will not update the entity.

### afterUpdate
Called _after_ a new entity is updated via the `save` or `update` method.

### beforeValidate
Called _before_ validation is run. Triggered by _every save event_ through any
method that saves an entity.

Returning `false` from your event callback will trigger an immediate validation
failure, will not run any other validations, and will not save the entity.

### afterValidate
Called _after_ validation is run. Triggered by _every save event_ through any
method that saves an entity.

Returning `false` from your event callback will trigger a validation failure,
and will not save the entity.

