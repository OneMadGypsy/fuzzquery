fuzzquery 24.5.28
=================

**fuzzquery** is a lightweight package for fuzzy word/phrase searches in a body of text. :doc:`tokens` are used to determine the number and type of approximations that can be made at a token's position in the ``query``. The :doc:`api` exposes 3 generator functions for performing searches - :ref:`finditer`, :ref:`findall`, and :ref:`iterall`.

Installation
------------

To install ``fuzzquery`` use the following command-line:

.. code-block:: console

  pip install fuzzquery
  
------------------

Definitions
-----------

``term``
  A group of consecutive, non-whitespace characters with word boundaries
``query``
  One or more ``terms`` separated by whitespace
``span``
  A tuple of slice coordinates for the current match
``match``
  The text that was matched
 
------------------

Contents
--------

.. toctree::

  tokens
  api
  examples

------------------

Links
-----

- `fuzzquery github <https://github.com/OneMadGypsy/fuzzquery/>`_
- `fuzzquery pypi <https://pypi.org/project/fuzzquery/>`_
