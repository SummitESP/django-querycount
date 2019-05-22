Django Querycount
=================

Inspired by **[this post by David Szotten](http://goo.gl/UUKN0r)**, this project
gives you a middleware that prints DB query counts in Django's runserver
console output.

![django-querycount in action](screenshot.png)


Installation
------------

The SummitESP version of django-querycount is already integrated into the SK Integration development environment.

To enable it, add **ENABLE_QUERYCOUNT=True** to .dockerenv.

**Note:** django-querycount is hard coded to work only with **DEBUG=True** in settings.

Settings
--------

The default settings for the app are:

```python
{
    'THRESHOLDS': {
        'MEDIUM': 50,
        'HIGH': 200,
        'MIN_TIME_TO_LOG':0,
        'MIN_QUERY_COUNT_TO_LOG':0
    },
    'IGNORE_REQUEST_PATTERNS': [],
    'IGNORE_SQL_PATTERNS': [],
    'DISPLAY_DUPLICATES': None,
    'RESPONSE_HEADER': 'X-DjangoQueryCount-Count'
}
```

The **THRESHOLDS** settings will determine how many queries are
interpreted as high or medium (and the color-coded output).

The **IGNORE_REQUEST_PATTERNS** setting allows you to define a list of
regexp patterns that get applied to each request's path. If there is a match,
the middleware will not be applied to that request. For example, the following
setting would bypass the querycount middleware for all requests to the admin::

```python
{ ..., 'IGNORE_REQUEST_PATTERNS': [r'^/admin/'], ... }
```

The **IGNORE_SQL_PATTERNS** setting allows you to define a list of
regexp patterns that ignored to statistic sql query count. For example, the following
setting would bypass the querycount middleware for django-silk sql query::

```python
{ ... 'IGNORE_SQL_PATTERNS': [r'silk_'], ... }
```

The **DISPLAY_DUPLICATES** setting allows you
to control how the most common duplicate queries are displayed. If the setting
is **None** (the default), duplicate queries are not displayed. Otherwise, this
should be an integer. For example, the following setting would always print the
5 most duplicated queries::

```python
{ ... 'DISPLAY_DUPLICATES': 5, ... }
```

The **RESPONSE_HEADER** setting allows you to define a custom response
header that contains the total number of queries executed. To disable this header, 
supply **None** as the value::

```python
{ ... 'RESPONSE_HEADER': None, ... }
```

To provide completely custom settings, set **QUERYCOUNT_SETTINGS=\<settings dict all on one line\>** in .dockerenv.

To use the default settings and set ***ONLY*** the **DISPLAY_DUPLICATES** setting, set **QUERYCOUNT_DISPLAY_DUPLICATES=\<some number\>** in .dockerenv.

License
-------

This code is distributed under the terms of the MIT license.

Testing
-------

Run `python manage.py test querycount` to run the tests. Note that this will
modify your settings so that your project is in DEBUG mode for the duration
of the `querycount` tests.

(side-note: this project needs better tests; for the moment, there are only
smoke tests that set up the middleware and call two simple test views).

Contributing
------------

Bug fixes and new features are welcome! Fork this project and send a Pull Request
to have your work included. Be sure to add yourself to ``AUTHORS.rst``.
