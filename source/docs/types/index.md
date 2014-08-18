title: Field Types
layout: docs_page
date: 2014-08-14 09:34:30
---

Since Spot v2.x is built on top of DBAL, all the [DBAL
types](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/types.html)
are used and fully supported in Spot:

## Built-In Field Types

### Integer Types
 * `smallint`
 * `integer`
 * `bigint`

### Decimal Types
 * `decimal`
 * `float`

### String Types
 * `string`
 * `text`
 * `guid`

### Binary String Types
 * `binary`
 * `blob`

### Boolean/Bit Types
 * `boolean`

### Date and Time Types
 * `date`
 * `datetime`
 * `datetimez`
 * `time`

### Array Types
 * `array` - PHP serialize/deserialze
 * `simple_array` - PHP implode/explode
 * `json_array` - json_encode/json_decode

### Object Types
 * `object` - PHP serialize/deserialze

## Further Reading

Please read the [Doctrine DBAL Types Reference
Page](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/types.html)
thoroughly for more information and types and cross-database support. Some
types may be stored differently on different databases, depending on database
vendor support and other factors.

## Registering Custom Field Types

If you want to register your own custom field type with custom
functionality on get/set, have a look at the [Custom Mapping Types on the DBAL
reference page](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/types.html#custom-mapping-types).

Since Spot uses the DBAL internally, there are no additional changes you have
to make for your custom type to work with Spot.

