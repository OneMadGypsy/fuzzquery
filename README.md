# fuzzquery
A lightwieght package for doing fuzzy matches with a simple query language. This works by creating regular expressions for you that utilize the fuzzy matching features of the `regex` package, based on your query tokens. 


## Installation:
Open a terminal and input the following command: **`pip install fuzzquery`**. This will install `fuzzquery` and it's `regex` dependency.

## Theory:
The regex package has 3 fuzzy matching features:

| type       | dsecription                                             |
| ---------- | ------------------------------------------------------- |
| insert     | 0 or more characters appear **before** your term        |
| substitute | 0 or more characters are substituted in your term       |
| delete     | 0 or more characters are removed from your term         |

This system completely ignores the native insertion model and creates it's own with substitution and deletion. This is because insertion is wholly uncontrollable. It is the only feature that allows characters where there never were any. My system reformats queries with substitution/deletion characters wherever a token is present. This means if you put a token before your term, it is essentially a controllable insertion. The back-end system will view it as a substitution and/or deletion, but it is a sub/del before your term so, it is functionally an insertion. 

Substitutions and deletions can only happen where sub/del characters are present. This is because the sub/del score limit is identical to the amount of sub/del characters. If a non-sub/del character was to be subbed or deleted, it would leave behind an original sub/del character (backtick) and it is unlikely that character will be a part of your term so, the match would fail. Basically, you can reliably search for anything that does **not** have a backtick in it. Technically, you can even reliably search for things that do have a backtick in them, but you are playing with the possibility of hitting an edge case that may yield some incorrect results.

## Documentation:

**notes:**

1) `Iter` is an alias of `list|tuple|set`. It only exists in this documentation as a means to illustrate types in a less complex way.
2) `skip` (if used) should be an `Iter` of words and/or characters that trigger a skip when found in results to be yielded.

--------

### `finditer`
> yield all matches of a single query.

**finditer(`text`:str, `query`:str, `skip`:Iter|None=None, `ci`:bool=False) -> Iterator**
| arg      | description                                                                   |
| -------- | ----------------------------------------------------------------------------- |
| `text`   | the text to search                                                            |
| `query`  | the query to search for                                                       |
| `skip`   | iterable of terms and/or characters that trigger a skip when found in results |
| `ci`     | case-insensitive matching                                                     |

--------

### `findany`
> `OR` queries together and yield whatever matched.

**findany(`text`:str, `queries`:Iter, `skip`:Iter|None=None, `ci`:bool=False) -> Iterator**
| arg       | description                                                                   |
| --------- | ----------------------------------------------------------------------------- |
| `text`    | the text to search                                                            |
| `queries` | iterable of queries to search for                                             |
| `skip`    | iterable of terms and/or characters that trigger a skip when found in results |
| `ci`      | case-insensitive matching                                                     |

--------

### `iterall`
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

| token  | type    | description                           | example           | result-like                     |
| ------ | ------- | ------------------------------------- | ----------------- | ------------------------------- |
| `{x}`  | allowed | 0 to `x` non-whitespace characters    | `"home{5}"`       | `home, homestead, homeward`     |
| `{!x}` | strict  | exactly `x` non-whitespace characters | `"{1}ward{!2}"`   | `warden, awarded`               |
| `{?}`  | unknown | 0 or more unknown words               | `"thou {?} kill"` | `thou shalt not kill`           |
