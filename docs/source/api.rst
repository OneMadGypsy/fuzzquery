API
========

.. note::

  - A version of this documentation can also be viewed via python's built-in ``help`` function.
  - To simplify this documentation, ``Iter`` is being used as an alias of ``list|tuple|set``. There is no explicit reference to ``Iter`` in the ``fuzzquery`` package.

**fuzzquery** consists of 3 generator functions for performing searches.

finditer -> Iterator
  yields all matches of 1 ``query``
findall -> Iterator
  joins an ``Iter`` of queries by `OR`, and yields consecutive ``matches``
iterall -> Iterator
  loops over an  ``Iter`` of ``queries``, calling ``finditer`` on them, and yielding all results

------------------

.. _finditer:

finditer
--------

  Yield all (``span``, ``match``) of a single query.

+----------+-----------------------------------------------------------------------+--------------+
| arg      | description                                                           | type         |
+==========+=======================================================================+==============+
|*text*    | the text to search                                                    | `str`        |
+----------+-----------------------------------------------------------------------+--------------+
|*query*   | the ``query`` to search for                                           | `str`        |
+----------+-----------------------------------------------------------------------+--------------+
|*skip*    | ``terms`` and/or characters that trigger a skip when found in results | `Iter|None`  |
+----------+-----------------------------------------------------------------------+--------------+
|*ci*      | case-insensitive matching  (default: False)                           | `bool`       |
+----------+-----------------------------------------------------------------------+--------------+

.. code-block:: python

  import fuzzquery as fq

  ...

  # skip and ci are shown as their default value
  for span, match in fq.finditer(text, query, skip=None, ci=False):
      ...

------------------

.. _findall:

findall
-------

  Join ``queries`` by `OR`, and yield all (``span``, ``match``) of "whatever-is-next".

+-----------+-----------------------------------------------------------------------+--------------+
| arg       | description                                                           | type         |
+===========+=======================================================================+==============+
|*text*     | the text to search                                                    | `str`        |
+-----------+-----------------------------------------------------------------------+--------------+
|*queries*  | queries to combine for "whatever-is-next" search                      | `Iter`       |
+-----------+-----------------------------------------------------------------------+--------------+
|*skip*     | ``terms`` and/or characters that trigger a skip when found in results | `Iter|None`  |
+-----------+-----------------------------------------------------------------------+--------------+
|*ci*       | case-insensitive matching  (default: False)                           | `bool`       |
+-----------+-----------------------------------------------------------------------+--------------+

.. code-block:: python

  import fuzzquery as fq

  ...

  # skip and ci are shown as their default value
  for span, match in fq.findall(text, queries, skip=None, ci=False):
      ...

---------------

.. _iterall:

iterall
-------

  Yield all (``query``, ``span``, ``match``) of 1 or more ``queries``. 
  
+-----------+-----------------------------------------------------------------------+--------------+
| arg       | description                                                           | type         |
+===========+=======================================================================+==============+
|*text*     | the text to search                                                    | `str`        |
+-----------+-----------------------------------------------------------------------+--------------+
|*queries*  | queries to combine for "whatever-is-next" search                      | `Iter`       |
+-----------+-----------------------------------------------------------------------+--------------+
|*skip*     | ``terms`` and/or characters that trigger a skip when found in results | `Iter|None`  |
+-----------+-----------------------------------------------------------------------+--------------+
|*ci*       | case-insensitive matching  (default: False)                           | `bool`       |
+-----------+-----------------------------------------------------------------------+--------------+

.. code-block:: python

  import fuzzquery as fq

  ...

  # skip and ci are shown as their default value
  for query, span, match in fq.iterall(text, queries, skip=None, ci=False):
      if query:
          print(query)
		  
.. note::

  It could take numerous iterations to complete a single ``query``. To indicate that ``query`` has changed, every new ``query`` only has a value on it's first match.
		  
		  
		  
