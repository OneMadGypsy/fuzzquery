# fuzzquery
A lightweight package for fuzzy matching.


## Installation:
To install `fuzzquery` and it's `regex` dependency, open a terminal and input the following command: 
### `pip install fuzzquery`


## Theory:
The `regex` package has 3 fuzzy matching features:

| type       | description                                       |
| ---------- | ------------------------------------------------- |
| insert     | 0 or more characters appear **before** your term  |
| substitute | 0 or more characters are substituted in your term |
| delete     | 0 or more characters are removed from your term   |

This system completely ignores the native insertion model and creates it's own with substitution and deletion. This is because insertion is wholly uncontrollable. It is the only feature that allows characters where there never were any. My system reformats queries with substitution/deletion characters wherever a token is present. This means if you put a token before your term, it is functionally an insertion.


## Documentation:

**notes:**

1) `Iter` is an alias of `list|tuple|set`. It only exists in this documentation as a means to illustrate types in a less complex way.
2) `skip` (if used) should be an `Iter` of words and/or characters that cannot be `in` a result.

--------

### finditer
> yield all (`span`, `match`) of a single query.

**finditer(`text`:str, `query`:str, `skip`:Iter|None=None, `ci`:bool=False) -> Iterator**
| arg      | description                                                               |
| -------- | ------------------------------------------------------------------------- |
| `text`   | the text to search                                                        |
| `query`  | the query to search for                                                   |
| `skip`   | Iter of terms and/or characters that trigger a skip when found in results |
| `ci`     | case-insensitive matching                                                 |

--------

### findany
> `OR` queries together and yield all (`span`, `match`) of whatever matched.

**findany(`text`:str, `queries`:Iter, `skip`:Iter|None=None, `ci`:bool=False) -> Iterator**
| arg       | description                                                               |
| --------- | ------------------------------------------------------------------------- |
| `text`    | the text to search                                                        |
| `queries` | Iter of queries to search for                                             |
| `skip`    | Iter of terms and/or characters that trigger a skip when found in results |
| `ci`      | case-insensitive matching                                                 |

--------

### iterall
> yield all (`query`, `span`, `match`) of multiple queries.

**iterall(`text`:str, `queries`:Iter, `skip`:Iter|None=None, `ci`:bool=False) -> Iterator**
| arg       | description                                                               |
| --------- | ------------------------------------------------------------------------- |
| `text`    | the text to search                                                        |
| `queries` | Iter of queries to search for                                             |
| `skip`    | Iter of terms and/or characters that trigger a skip when found in results |
| `ci`      | case-insensitive matching                                                 |


## Queries:

A token system is used to represent unknown/fuzzy data. The 3 types of tokens are:

| token  | type    | description                           | example           | result-like                     |
| ------ | ------- | ------------------------------------- | ----------------- | ------------------------------- |
| `{x}`  | allowed | 0 to `x` non-whitespace characters    | `"home{5}"`       | `home, homestead, homeward`     |
| `{!x}` | strict  | exactly `x` non-whitespace characters | `"{1}ward{!2}"`   | `warden, awarded`               |
| `{?}`  | unknown | 0 or more unknown words               | `"thou {?} kill"` | `thou shalt not kill`           |


## Examples:

```python3
import fuzzquery as fq

data = """ 
I headed homeward to meet with the Wardens. 
When I arrived, I was greeted by a homely man that told me the homestead was awarded 5 million dollars.
We intend to use some of the homage to create a homeless ward. 
The first piece of furniture will be my late-friend Homer's wardrobe.
"""
queries = ('hom{5} {?} wa{!1}{5}', 
           'home{5}', 
           '{1}ward{!2}{2}', 
           'home{4} ward{4}')

for query, span, match in fq.iterall(data, queries, ci=True):
    if query: print(f'\n{query.upper()}')
    print(f'  {match}')
```

#### output

```none
HOM{5} {?} WA{!1}{5}
  homeward to meet with the Wardens
  homely man that told me the homestead was
  homage to create a homeless ward
  Homer's wardrobe

HOME{5}
  homeward
  homely
  homestead
  homeless
  Homer's

{1}WARD{!2}{2}
  Wardens
  awarded
  wardrobe

HOME{4} WARD{4}
  homeless ward
  Homer's wardrobe
```

--------

```python3
import fuzzquery as fq

data = """ 
I would classify music as one of my favorite hobbies. 
I love classical music played by classy musicians for a classic musical. 
Beethoven can not be out-classed, music-wise - a man of class, musically gifted.
"""
query = 'class{4} music{4}'

print(f'\n{query.upper()} with skip')
for span, match in fq.finditer(data, query, skip=('classify', ','), ci=True):
    print(f'  {match}')
    
print(f'\n{query.upper()} no skip')
for span, match in fq.finditer(data, query, ci=True):
    print(f'  {match}')
```

#### output

```none
CLASS{4} MUSIC{4} with skip
  classical music
  classy musicians
  classic musical

CLASS{4} MUSIC{4} no skip
  classify music
  classical music
  classy musicians
  classic musical
  classed, music
  class, musically
```
