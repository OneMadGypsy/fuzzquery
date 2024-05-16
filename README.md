# fuzzquery
A lightwieght package for doing fuzzy matches with a simple query language


## functions

#### `finditer(text:str, query:str, skip:list|tuple, flags:str)`

| arg      | description             |
| -------- | ----------------------- |
| `text`   | the text to search      |
| `query`  | the query to search for |
| `skip`   | iterable of terms and/or characters that trigger a skip when found in results |
| `flags`  | str of regex flags    |

--------

#### `findany(text:str, queries:list|tuple, skip:list|tuple, flags:str)`

| arg      | description             |
| -------- | ----------------------- |
| `text`   | the text to search      |
| `queries`  | iterable of queries to search for |
| `skip`   | iterable of terms and/or characters that trigger a skip when found in results |
| `flags`  | str of regex flags    |

--------

#### `iterall(text:str, queries:list|tuple, skip:list|tuple, flags:str)`

| arg      | description             |
| -------- | ----------------------- |
| `text`   | the text to search      |
| `queries`  | iterable of queries to search for |
| `skip`   | iterable of terms and/or characters that trigger a skip when found in results |
| `flags`  | str of regex flags    |

## queries

A token system is used to represent unknown/fuzzy data. The 3 types of tokens are:

| token    | type    | description                           | example           | result-like                         |
| -------- | ------- | ------------------------------------- | ----------------- | ------------------------------- |
| `"{x}"`  | allowed | from 0 to x non-whitespace characters | `"home{5}"`       | `homeless, homestead, homeward` |
| `"{!x}"` | strict  | exactly x non-whitespace characters   | `"{1}ward{!2}"`   | `warden, awarded`               |
| `"{?}"`  | unknown | 0 or more unknown words               | `"thou {?} kill"` | `thou shalt not kill`           |
