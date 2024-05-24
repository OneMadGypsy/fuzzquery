Tokens
======

Tokens are used to represent unknown/fuzzy data. A token's placement determines where a substitution, substitution & deletion or joining words may appear in the query.
The ``range`` & ``strict`` tokens capture `non-whitespace` characters, and can be placed anywhere. 
The ``joining`` token captures 0 or more unknown terms between known terms, and must be placed in the space between any 2 terms.

Definitions
-----------

insertion
  placing a character between existing adjacent characters
substitution
  replacing a character with a different character
deletion
  removing a character
total
  the number of times an approximation was made
limit
  the maximum number of allowed approximations

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


Theory
------

The ``regex`` package has a few `approximation <https://github.com/mrabarnett/mrab-regex#approximate-fuzzy-matching-hg-issue-12-hg-issue-41-hg-issue-109>`_ expressions.
The approximations are made by `insertion`, `substitution` and `deletion`. All 3 of those are managed in 2 ways:

  1. you assign a score to each and provide a ``limit`` for their combined ``total``
  2. you explicitly state which ones are allowed and provide a ``limit`` for their individual ``total``

Since ``insertion`` is the only behavior that allows characters to be injected, ``insertions`` are explicitly and implicitly forbidden. 
From the list above, this is what happens to ``insertion``:

  1. given a score higher than the ``limit``
  2. never explicitly stated as being allowed

This leaves us with ``substitution`` and ``deletion``. Matching the items in the uppermost list - this is how tokens are managed:

  1. **range** : allows for ``substitution`` and ``deletion`` if ``0 <= total <= limit``
  2. **strict** : allows for ``substitutions`` only, and ``total`` must equal ``limit``

.. note::

  Using a strict or range token is to imply:
    "Create a string of replacement characters with a length of ``x``, and allow/require a ``total`` of ``x`` approximations."
