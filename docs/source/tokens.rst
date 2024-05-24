Tokens
======

Overview
--------

Tokens are used to represent unknown/fuzzy data. A token's placement determines where a substitution, substitution & deletion or joining words may appear in the query.
The ``range`` & ``strict`` tokens capture `non-whitespace` characters, and can be placed anywhere. 
The ``joining`` token captures 0 or more unknown terms between known terms, and must be placed in the space between any 2 terms.
  
+--------+---------+---------------------------------------+------------------+--------------------------------+
| token  | type    | description                           | example          | result-like                    |
+========+=========+=======================================+==================+================================+
| "{x}"  | range   | 0 to x non-whitespace characters      | "home{5}"        | home, homestead, homeward      |
+--------+---------+---------------------------------------+------------------+--------------------------------+
| "{!x}" | strict  | exactly x non-whitespace characters   | "{1}ward{!2}"    | warden, awarded                |
+--------+---------+---------------------------------------+------------------+--------------------------------+
| "{?}"  | joining | 0 or more unknown joining terms       | "thou {?} kill"  | thou shalt not kill            |
+--------+---------+---------------------------------------+------------------+--------------------------------+
