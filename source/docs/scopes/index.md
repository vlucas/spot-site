title: Scopes
layout: docs_page
date: 2014-08-14 09:34:30
---

While you can create custom query methods by using [custom
mappers](/docs/mappers/), it is often desirable to create small named
bite-sized conditions for queries that can be chained together to create more
complex queries.  Scopes are a convenient way to add custom methods to a
`Spot\Query` object that serve this purpose.

## Example Mapper With Scopes Defined

```php
<?php
namespace Entity\Mapper;

use Spot\Mapper;

class Post extends Mapper
{
    public function scopes()
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
  The scopes you define in the `scopes` method on your custom mapper will be
  accessible from the `Spot\Query` object.
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


