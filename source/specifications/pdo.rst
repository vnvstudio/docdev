PDO
===


Introduction
------------

Several database engines
------------------------

We want be able to use GLPI with following databases:

* MySQL/MariaDB
* Postgresql
* SQLite

To achieve that, we will use PDO. We have try some ORM but not have good performances.

Performances
------------

To have better performances, we will do only simple queries (never use joins).

Main changes in database
------------------------

The database structure must be changed:

* table names from *plural* to *singular* (more simple to manage)
* foreignkey from *plural* to *singular* (more simple to manage)
* all boolean like ``is_deleted`` must be in type *boolean* instead of *integer*
* be sure we use integer and no string for numerical values
* foreignkeys with action:

  * *update* => ``NO_ACTION``
  * *delete* => ``SET NULL``, do not use ``CASCADE`` because we will not be able to manage logs of delete items

Use abstraction
---------------

In case we want use another ORM later and to be more simple in GLPI/plugin code, we will define an abstraction layer::

     glpi code <-> abstraction class <-> use LessQL functions

Create a class: ``CommonDb``

with functions:

* ``table``: define the table to do query, so create the query
* ``select``: list of fields to select, so add select to the query (if this not called, will get all fields), id is always implicit, so not need ad it
* ``where``: first argument is the field, second is what to search (if string is *your string*, this will query for ``IN ('your', 'string')``), third is ``''`` or ``'not'``
* ``orderby``: first argument is the field name, second is the order, ``ASC`` or ``DESC``
* ``limit``: first argument is count, second the offset
* ``count``: get number of elements found
* ``fetch``: get only first element
* ``fetchall``: get all elements

For example query *all computers with id and name having is_deleted equals to true*:

.. code-block:: php

   <?php
   $cDB = new CommonDB();
   $cDB->table('glpi_computer');
   $cDB->select('name');
   $cDB->where('is_deleted', true);
   $my_computers = $cDB->fetchall();
