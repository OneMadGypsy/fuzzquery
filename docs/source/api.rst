API
========

**fuzzquery** consists of 3 generator functions for performing searches.

finditer -> Iterator
  yields all matches of 1 query
findall -> Iterator
  joins an ``Iter`` of queries by `OR`, and yields consecutive matches
iterall -> Iterator
  loops over an  ``Iter`` of queries, calling ``finditer`` on them, and yielding all results

.. note::

  To simplify this documentation, ``Iter`` is being used as an alias of ``list|tuple|set``. There is no explicit reference to ``Iter`` in the ``fuzzquery`` package.


Definitions
-----------

query
  a singular query string
span
  a tuple of slice coordinates for the current match
match
  the text that was matched

----------

finditer
--------

  yield all (``span``, ``match``) of a single query.

+----------+-------------------------------------------------------------------+--------------+
| arg      | description                                                       | type         |
+==========+===================================================================+==============+
|*text*    | the text to search                                                | `str`        |
+----------+-------------------------------------------------------------------+--------------+
|*query*   | the query to search for                                           | `str`        |
+----------+-------------------------------------------------------------------+--------------+
|*skip*    | terms and/or characters that trigger a skip when found in results | `Iter|None`  |
+----------+-------------------------------------------------------------------+--------------+
|*ci*      | case-insensitive matching  (default: False)                       | `bool`       |
+----------+-------------------------------------------------------------------+--------------+

.. code-block:: python

  import fuzzquery as fq

  for span, match in fq.finditer(text, query, skip=None, ci=False):
      ...

------------------

findall
-------

  `OR` queries together and yield all (``span``, ``match``) of "whatever-is-next".

+-----------+-------------------------------------------------------------------+--------------+
| arg       | description                                                       | type         |
+===========+===================================================================+==============+
|*text*     | the text to search                                                | `str`        |
+-----------+-------------------------------------------------------------------+--------------+
|*queries*  | queries to combine for "whatever-is-next" search                  | `Iter`       |
+-----------+-------------------------------------------------------------------+--------------+
|*skip*     | terms and/or characters that trigger a skip when found in results | `Iter|None`  |
+-----------+-------------------------------------------------------------------+--------------+
|*ci*       | case-insensitive matching  (default: False)                       | `bool`       |
+-----------+-------------------------------------------------------------------+--------------+

.. code-block:: python

  import fuzzquery as fq

  for span, match in fq.findall(text, queries, skip=None, ci=False):
      ...

---------------

iterall
-------

  yield all (``query``, ``span``, ``match``) of 1 or more queries. ``query`` is always ``None`` except on it's first match. This is your indicator that a new query is being processed.
  
+-----------+-------------------------------------------------------------------+--------------+
| arg       | description                                                       | type         |
+===========+===================================================================+==============+
|*text*     | the text to search                                                | `str`        |
+-----------+-------------------------------------------------------------------+--------------+
|*queries*  | queries to iterate over                                           | `Iter`       |
+-----------+-------------------------------------------------------------------+--------------+
|*skip*     | terms and/or characters that trigger a skip when found in results | `Iter|None`  |
+-----------+-------------------------------------------------------------------+--------------+
|*ci*       | case-insensitive matching  (default: False)                       | `bool`       |
+-----------+-------------------------------------------------------------------+--------------+

.. code-block:: python

  import fuzzquery as fq

  for query, span, match in fq.iterall(text, queries, skip=None, ci=False):
      if query:
          # first match for this query, else None
          print(query)
