.. _specifications-lessql:

Introduction
------------

Many databases
==============

We want be able to use GLPI with databases:

* MySQL
* MariaDB
* Postgresql
* sqlite

To do that, we will use a lightweight ORM: LessQL (http://lessql.net/)


Performances
============

To have better performances, we will do only simple queries (never use joins).


Main changes in database
========================

The database structure must be changed:

* table names from *plurial* to *singular*
* foreignkey from *plurial* to *singular*
* all boolean like is_deleted must be in type *boolean* instead *integer*
* foreignkeys with action:
    * *update* => *NO_ACTION*
    * *delete* => *SET NULL* , not use *CASCADE* because we will not be able to manage logs of delete items




Use abstraction
===============

In case we want use another ORM later, we will define an abstraction layer::

     glpi code <-> abstraction class <-> use LessQL functions


Create a class: *CommonDB*

with functions: 

* *table*: define the table to do query, so create the query
* *select*: list of fields to select, so add select to the query (if this not called, will get all fields), id is always implicit, so not need ad it
* *where*: first arg is the field, second is what to search (is string it's = 'your string', if array it's IN ('str', 'str')), thirt is '' or 'not
* *orderby*: first arg is the field name, second 'ASC' or 'DESC'
* *limit*: first arg is count, second the offset
* *count*: get number of elements found
* *fetch*: get only first element
* *fetchall*: get all elements

For example query all computers with id + name have is_deleted = false::

    $cDB = new CommonDB();
    $cDB->table('glpi_computer');
    $cDB->select('name');
    $cDB->where('is_deleted', true);
    $my_computers = $cDB->fetchall();


