Tokens
======

Tokens are used to represent unknown/fuzzy data. A token's placement determines where a substitution, substitution & deletion or joining ``terms`` may appear in the ``query``.

Overview
--------
  
+--------+---------+---------------------------------------+------------------+--------------------------------+
| token  | type    | description                           | example          | result-like                    |
+========+=========+=======================================+==================+================================+
| "{x}"  | range   | 0 to x non-whitespace characters      | "home{5}"        | home, homestead, homeward      |
+--------+---------+---------------------------------------+------------------+--------------------------------+
| "{!x}" | strict  | exactly x non-whitespace characters   | "{2}ward{!2}"    | warden, awarded, rewarded      |
+--------+---------+---------------------------------------+------------------+--------------------------------+
| "{?}"  | joining | 0 or more unknown joining ``terms``   | "thou {?} kill"  | thou shalt not kill            |
+--------+---------+---------------------------------------+------------------+--------------------------------+

Limiting Tokens
---------------

The **range** & **strict** tokens are considered "limiting tokens". They capture `non-whitespace` characters, and can be placed anywhere in a query.

.. note::

  **Using a limiting token implies:**

    - create a string ``x`` replacement characters long, at this token's position
    - require or allow ``x`` approximations on the replacement characters

The ``regex`` package has a few `approximation <https://github.com/mrabarnett/mrab-regex#approximate-fuzzy-matching-hg-issue-12-hg-issue-41-hg-issue-109>`_ expressions.
The approximations are made by `insertion`, `substitution` and `deletion`. All 3 of those are managable in 2 ways:

  :range: assign a score to each and provide a ``limit`` for their combined ``total``
  :strict: explicitly state which are allowed and provide ``limits`` for their individual ``total(s)``

Since ``insertion`` is the only behavior that allows characters to be injected, ``insertions`` are explicitly and implicitly forbidden. 
This is how ``insertion`` is managed in **fuzzquery**:

  :range: given a score higher than the ``limit``
  :strict: never explicitly stated as being allowed

This leaves us with ``substitution`` and ``deletion``. This is how both are managed in **fuzzquery**:

  :range: allows for ``substitution`` and ``deletion`` if ``total <= limit``
  :strict: allows for ``substitutions`` only, and ``total`` must equal ``limit``

Joining Token
-------------

The **joining** token doesn't use any approximations. It gets all unknown ``term`` instances between 2 known ``terms``. It does not currently accept a ``limit``.
