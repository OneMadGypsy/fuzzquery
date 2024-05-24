fuzzquery 24.5.26
=================

**fuzzquery** is a lightweight package for fuzzy word/phrase searches in a body of text. Tokens are used to determine the number and type of approximations that can be made at your query's token position.

Installation
------------

To install ``fuzzquery`` and it's ``regex`` depencency, use hte following command-line:

.. code-block:: console

  pip install fuzzquery

Definitions
-----------

insertion
  Injecting a character between existing adjacent characters
substitution
  Replacing a character with a different character
deletion
  Removing a character
total
  The number of times an approximation was made or the combined score of those approximations
limit
  The maximum number of allowed approximations. Also referred to as ``x``.
term
  A group of consecutive, non-whitespace characters with word boundaries

Contents
--------

.. toctree::

   tokens
   api
   examples

Links
-----

- `fuzzquery github <https://github.com/OneMadGypsy/fuzzquery/>`_
- `fuzzquery pypi <https://pypi.org/project/fuzzquery/>`_
- `regex pypi <https://pypi.org/project/regex/>`_
