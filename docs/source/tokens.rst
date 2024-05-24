Tokens
======

Tokens are used to represent unknown/fuzzy data. A token's placement determines where a substitution, substitution & deletion or joining words may appear in the query.
The ``range`` & ``strict`` tokens capture `non-whitespace` characters, and can be placed anywhere. 
The ``joining`` token captures 0 or more unknown terms between known terms, and must be placed in the space between any 2 terms.

Definitions
-----------

insertion
  Placing a character between existing adjacent characters
substitution
  Replacing a character with a different character
deletion
  Removing a character
total
  The number of times an approximation was made or the combined score of those approximations
limit
  The maximum number of allowed approximations. Also referred to as ``x``

Overview
--------
  
+--------+---------+---------------------------------------+------------------+--------------------------------+
| token  | type    | description                           | example          | result-like                    |
+========+=========+=======================================+==================+================================+
| "{x}"  | range   | 0 to x non-whitespace characters      | "home{5}"        | home, homestead, homeward      |
+--------+---------+---------------------------------------+------------------+--------------------------------+
| "{!x}" | strict  | exactly x non-whitespace characters   | "{2}ward{!2}"    | warden, awarded, rewarded      |
+--------+---------+---------------------------------------+------------------+--------------------------------+
| "{?}"  | joining | 0 or more unknown joining terms       | "thou {?} kill"  | thou shalt not kill            |
+--------+---------+---------------------------------------+------------------+--------------------------------+

Limiting Tokens
---------------

The ``regex`` package has a few `approximation <https://github.com/mrabarnett/mrab-regex#approximate-fuzzy-matching-hg-issue-12-hg-issue-41-hg-issue-109>`_ expressions.
The approximations are made by `insertion`, `substitution` and `deletion`. All 3 of those are managed in 2 ways:

  :range: you assign a score to each and provide a ``limit`` for their combined ``total``
  :strict: you explicitly state which are allowed and provide ``limits`` for their individual ``total``

Since ``insertion`` is the only behavior that allows characters to be injected, ``insertions`` are explicitly and implicitly forbidden. 
This is how ``insertion`` is managed:

  :range: given a score higher than the ``limit``
  :strict: never explicitly stated as being allowed

This leaves us with ``substitution`` and ``deletion``:

  :range: allows for ``substitution`` and ``deletion`` if ``0 <= total <= limit``
  :strict: allows for ``substitutions`` only, and ``total`` must equal ``limit``

.. note::

  Using a limiting token is to imply:
    "Create a string ``x`` replacement characters long, and require or allow ``x`` approximations."

Joining Token
-------------

The ``joining`` token doesn't use any approximations. It gets everything between 2 terms, if there is anything to get. It does not currently accept a ``limit``, but I intend to change that in the next build.
