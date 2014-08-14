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
            'id'           => ['type' => 'integer', 'autoincrement' => true, 'primary' => true],
            'title'        => ['type' => 'string', 'required' => true],
            'body'         => ['type' => 'text', 'required' => true],
            'status'       => ['type' => 'integer', 'default' => 0, 'index' => true],
            'author_id'    => ['type' => 'integer', 'required' => true],
            'date_created' => ['type' => 'datetime', 'value' => new \DateTime()]
        ];
    }
}
```
