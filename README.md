# fuzzquery
A lightwieght package for doing fuzzy matches with a simple query language


## Docs:

**notes:**

1) `Iter` is an alias of `list|tuple|set`. It only exists in this documentation as a means to illustrate types in a less complex way.
2) `skip` (if used) should be an `Iter` of words and/or characters that trigger a skip when found in results to be yielded.

--------

### finditer
> yield all matches of a single query.

**finditer(`text`:str, `query`:str, `skip`:Iter|None=None, `ci`:bool=False) -> Iterator**
| arg      | description                                                                   |
| -------- | ----------------------------------------------------------------------------- |
| `text`   | the text to search                                                            |
| `query`  | the query to search for                                                       |
| `skip`   | iterable of terms and/or characters that trigger a skip when found in results |
| `ci`     | case-insensitive matching                                                     |

--------

### findany
> `OR` queries together and yield whatever matched.

**findany(`text`:str, `queries`:Iter, `skip`:Iter|None=None, `ci`:bool=False) -> Iterator**
| arg       | description                                                                   |
| --------- | ----------------------------------------------------------------------------- |
| `text`    | the text to search                                                            |
| `queries` | iterable of queries to search for                                             |
| `skip`    | iterable of terms and/or characters that trigger a skip when found in results |
| `ci`      | case-insensitive matching                                                     |

--------

### iterall
> yield all matches of multiple queries.

**iterall(`text`:str, `queries`:Iter, `skip`:Iter|None=None, `ci`:bool=False) -> Iterator**
| arg       | description                                                                   |
| --------- | ----------------------------------------------------------------------------- |
| `text`    | the text to search                                                            |
| `queries` | iterable of queries to search for                                             |
| `skip`    | iterable of terms and/or characters that trigger a skip when found in results |
| `ci`      | case-insensitive matching                                                     |


## Queries:

A token system is used to represent unknown/fuzzy data. The 3 types of tokens are:

| token    | type    | description                           | example           | result-like                     |
| -------- | ------- | ------------------------------------- | ----------------- | ------------------------------- |
| `"{x}"`  | allowed | 0 to `x` non-whitespace characters    | `"home{5}"`       | `home, homestead, homeward`     |
| `"{!x}"` | strict  | exactly `x` non-whitespace characters | `"{1}ward{!2}"`   | `warden, awarded`               |
| `"{?}"`  | unknown | 0 or more unknown words               | `"thou {?} kill"` | `thou shalt not kill`           |
