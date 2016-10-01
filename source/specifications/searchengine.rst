Search engine
=============


Introduction
------------

The search engine need to be updated, there are the points need to be updated:

* delete META because it's too complex to maintain
* need to be able group queries
* queries too complex (too many joins) and result query too long or block database for a time.


Display choice
--------------

Use _jquerybuiler

We remove META. For example in computer, we have criteria of computer and linked (networkport, infocom...) + software criteria and linked 
+ monitor criteria and linked... All in same dropdown, but with grouped differently (optgroup).


Internal
--------

All queries use simple queries to be fast.

Search class
~~~~~~~~~~~~

function show
_____________

This function is used to display query builder form


function showList
_________________

This function manage searh result.

We call in this function the functions:

* ``prepareDatasForSearch``: traduct query-builder params
* ``search_get_all_ids``: get all ids of the item (like in computer search, all id) of each jquery-builder groups
* ``search_ids_operations``: do operations (AND, OR) of groups of id
* ``prepareDisplayIdName``: with the array IDs found, get the id + name (+ comment) of each column
* ``prepareColumns``: convert each column to the field / html code
* ``displayDatas``: display the data


getSearchOptions function
~~~~~~~~~~~~~~~~~~~~~~~~~

With the end of META, each property will receive a unique number for all GLPI (now we can have same number in computer and in software).


We remove key ``joinparams`` and replace by ``path``. This key will specify the table + foreignkey to use to go to the value we want.
Example in computer search to have software name:

.. code:: php

      $tab[801]['path']           = [
          array(
              'table' => Computer_SoftwareVersion::getTable(),
              'field' => 'computer_id'
          ),
          array(
              'table' => SoftwareVersion::getTable(),
              'field' => 'softwareversion_id'
          ),
          array(
              'table' => Software::getTable(),
              'field' => 'software_id'
          ),
      );


This that, the code in search engine will be easier to create queries to get information.



.. _jquerybuilder: http://querybuilder.js.org/
