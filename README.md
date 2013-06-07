SphinxQL
========

A library for work with [Spinx](http://sphinxsearch.com/) QL Api ( uses mysqli php extension

##### Example

```php

$sphinxql = new SphinxQL();
$query = $sphinxql->newQuery();
$query
    ->addIndex('my_index')
    ->addField('field_name', 'alias')
    ->addField('another_field')

    ->addFields(
        array(
            array('field' => 'title', 'alias' => 'title_alias'),
            array('field' => 'user_id')
        )
    )

    ->search('some words to search for')

    // string (is given directly to sphinx, so can contain @field directives)
    ->where('time', time()-3600, '>', FALSE)


    // field, value, operator='=', quote=TRUE
    ->whereIn('tags_i_need', array(1, 2, 3), 'all')
    ->whereIn('tags_i_do_not_want', array(4, 5, 6), 'none')
    ->whereIn('tags_i_would_like_one_of', array(7, 8, 9), 'any')


    // field, array values, type='any'
    ->order('@weight', 'desc')


    // field, sort='desc'
    ->offset(10)->limit(50)


    // defaults are 0 and 20, same as the sphinx defaults
    ->option('max_query_time', '100')


    // option name, option value
    ->groupBy('field')
    ->in_group_order_by('another_field', 'desc');


    // sphinx-specific, check their docs
    $result = $query->execute();


    // get stats
    $stats = $sphinx->stats();
```

##### Example with RT Index

```php
$sphinxql = new SphinxQL();

$result = $sphinxql->query('
    INSERT INTO realtime_index
        (id, title, content)
    VALUES
        ( 1, "title news", "content news" )
    '
);
```