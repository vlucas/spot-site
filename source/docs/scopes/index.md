title: Scopes
layout: docs_page
date: 2014-08-14 09:34:30
---

While you can create custom query methods by using [custom
mappers](/docs/mappers/), it is often desirable to create small named
bite-sized conditions for queries that can be chained together to create more
complex queries.  Scopes are a convenient way to add custom methods to a
`Spot\Query` object that serve this purpose.

## Example Entity With Scopes Defined

```php
<?php
namespace Entity;

use Spot\Entity;
use Spot\Mapper;

class Event extends \Spot\Entity
{
    protected static $table = 'test_events';

    public static function fields()
    {
        return [
            'id'           => ['type' => 'integer', 'primary' => true, 'autoincrement' => true],
            'title'        => ['type' => 'string', 'required' => true],
            'description'  => ['type' => 'text', 'required' => true],
            'type'         => ['type' => 'string', 'required' => true, 'options' => [
                'free' => 'Free',
                'private' => 'Private (Ticket Required)',
                'vip' => 'VIPs only'
            ]],
            'date_start'   => ['type' => 'datetime', 'required' => true, 'validation' => [
                'dateAfter' => new \DateTime('-1 second')
            ]],
            'status'       => ['type' => 'string', 'default' => 1, 'options' => [
                0 => 'Inactive',
                1 => 'Active',
                2 => 'Archived'
            ]],
            'date_created' => ['type' => 'datetime']
        ];
    }

    public static function scopes()
    {
        return [
            'free' => function ($query) {
                return $query->where(['type' => 'free']);
            },
            'active' => function ($query) {
                return $query->where(['status' => 1]);
            }
        ];
    }
}
```

<div class="callout info">
  The scopes you define in the static `scopes` method will be accessible from the
  `Spot\Query` object.
</div>

## Using Scopes in Queries

You can now use the `free` and `active` scopes as methods as soon as you have a
`Spot\Query` object.

```php
$posts = $mapper->all()->free()->active();
```

This will achieve the same result as chaining the `where` conditions directly,
but using scopes have much greater readability and clarity:

```php
$posts = $mapper->where(['type' => 'free'])->where(['status' => 1]);
```


