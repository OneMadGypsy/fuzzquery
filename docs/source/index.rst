fuzzquery documentation
=======================

**fuzzquery** is a lightweight package for fuzzy word/phrase searches in a body of text. A token system is used to represent unknown/fuzzy data.

Installation
------------

To install ``fuzzquery`` and it's ``regex`` depencency, open a terminal and input:

``pip install fuzzquery``


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
 

Examples
--------

.. code-block:: console

  import fuzzquery as fq
  
  data = """ 
  I headed homeward to meet with the Wardens. 
  When I arrived, I was greeted by a homely man that told me the homestead was awarded 5 million dollars.
  We intend to use some of the homage to create a homeless ward. 
  The first piece of furniture will be my late-friend Homer's wardrobe.
  """
  queries = ('hom{5} {?} wa{!1}{5}', 
             'home{5}', 
             '{1}ward{!2}{2}', 
             'home{4} ward{4}')
  
  for query, span, match in fq.iterall(data, queries, ci=True):
      if query: print(f'\n{query.upper()}')
      print(f'  {match}')
