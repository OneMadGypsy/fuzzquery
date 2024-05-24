API
========
.. note::

  ``list|tuple|set`` is aliased as ``Iter`` to simplify documentation. There is no ``Iter`` type in the ``fuzzquery`` package.

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

  ``OR`` queries together and yield all (``span``, ``match``) of "whatever-is-next".

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

  yield all (``query``, ``span``, ``match``) of multiple queries.
  
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
