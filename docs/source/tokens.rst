Tokens
======

Tokens are used to determine the number and type of approximations that can be made at a token's position in the ``query``.

Symbols
-------

+---------+--------------------------------------------+
| symbol  | description                                |
+=========+============================================+
| num     | allow a range ``0`` to ``num``             |
+---------+--------------------------------------------+
| "="     | exactly ``num``, instead of a range        |
+---------+--------------------------------------------+
| "w"     | signifies the token should match words     |
+---------+--------------------------------------------+

Examples
--------

+---------+----------------+-----------------------------+-------------------+--------------------------------+
| token   | type           | description                 | query             | result-like                    |
+=========+================+=============================+===================+================================+
| "{5}"   | range chars    | 0 to ``5`` characters       | "home{5}"         | home, homestead, homeward      |
+---------+----------------+-----------------------------+-------------------+--------------------------------+
| "{=2}"  | strict chars   | exactly ``2`` characters    | "{2}ward{=2}"     | warden, awarded, rewarded      |
+---------+----------------+-----------------------------+-------------------+--------------------------------+
| "{4w}"  | range words    | 0 to ``4`` words            | "thou {4w} kill"  | thou shalt not kill            |
+---------+----------------+-----------------------------+-------------------+--------------------------------+
| "{=2w}" | strict words   | exactly ``2`` words         | "bring {=2w}"     | bring it on, bring the pain    |
+---------+----------------+-----------------------------+-------------------+--------------------------------+


.. note::

  - word tokens must be surrounded by whitespace or query line ends
  - you can use "c"*hars* to contrast "w"*ords* (ex: "{=2c}")
  - tokens never match any form of whitespace