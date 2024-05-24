fuzzquery documentation
=======================

**fuzzquery** is a lightweight package for fuzzy word/phrase searches in a body of text. A token_system_. is used to represent unknown/fuzzy data.


.. _token_system:

Tokens
------

Tokens are used to represent unknown/fuzzy data. The 3 types of tokens are:
  
+--------+---------+---------------------------------------+------------------+--------------------------------+
| token  | type    | description                           | example          | result-like                    |
+========+=========+=======================================+==================+================================+
| "{x}"  | range   | 0 to x non-whitespace characters      | "home{5}"        | home, homestead, homeward      |
+--------+---------+---------------------------------------+------------------+--------------------------------+
| "{!x}" | strict  | exactly x non-whitespace characters   | "{1}ward{!2}"    | warden, awarded                |
+--------+---------+---------------------------------------+------------------+--------------------------------+
| "{?}"  | unknown | 0 or more unknown **words**           | "thou {?} kill"  | thou shalt not kill            |
+--------+---------+---------------------------------------+------------------+--------------------------------+

.. _interface:

Interface
---------
  **note:**
    ``list|tuple|set`` is aliased as ``Iter`` to simplify documentation. There is no ``Iter`` type in the ``fuzzquery`` package.

|  

**finditer(text, query, skip=None, ci=False) -> Iterator**
  
  yield all (``span``, ``match``) of a single query.
  
+----------+-------------------------------------------------------------------+----------------+
| arg      | description                                                       | type           |
+==========+===================================================================+================+
|*text*    | the text to search                                                | ``str``        |
+----------+-------------------------------------------------------------------+----------------+
|*query*   | the query to search for                                           | ``str``        |
+----------+-------------------------------------------------------------------+----------------+
|*skip*    | terms and/or characters that trigger a skip when found in results | ``Iter|None``  |
+----------+-------------------------------------------------------------------+----------------+
|*ci*      | case-insensitive matching                                         | ``bool``       |
+----------+-------------------------------------------------------------------+----------------+

|  

**findall(text, queries, skip=None, ci=False) -> Iterator**
  
  ``OR`` queries together and yield all (``span``, ``match``) of "whatever-is-next".
  
+-----------+-------------------------------------------------------------------+----------------+
| arg       | description                                                       | type           |
+===========+===================================================================+================+
|*text*     | the text to search                                                | ``str``        |
+-----------+-------------------------------------------------------------------+----------------+
|*queries*  | queries to combine for "whatever-is-next" search                  | ``Iter``       |
+-----------+-------------------------------------------------------------------+----------------+
|*skip*     | terms and/or characters that trigger a skip when found in results | ``Iter|None``  |
+-----------+-------------------------------------------------------------------+----------------+
|*ci*       | case-insensitive matching                                         | ``bool``       |
+-----------+-------------------------------------------------------------------+----------------+
	  
|  

**iterall(text, queries, skip=None, ci=False) -> Iterator**
  
  yield all (``query``, ``span``, ``match``) of multiple queries.
  
+-----------+-------------------------------------------------------------------+----------------+
| arg       | description                                                       | type           |
+===========+===================================================================+================+
|*text*     | the text to search                                                | ``str``        |
+-----------+-------------------------------------------------------------------+----------------+
|*queries*  | queries to iterate over                                           | ``Iter``       |
+-----------+-------------------------------------------------------------------+----------------+
|*skip*     | terms and/or characters that trigger a skip when found in results | ``Iter|None``  |
+-----------+-------------------------------------------------------------------+----------------+
|*ci*       | case-insensitive matching                                         | ``bool``       |
+-----------+-------------------------------------------------------------------+----------------+
 
