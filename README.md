# fuzzquery 24.5.26
A lightweight package for fuzzy word/phrase searches in a body of text, using a very simple token system.

--------

## Installation:
To install `fuzzquery` and it's `regex` dependency, use the folloqing command-line: 

``` console
pip install fuzzquery
```

--------

## Queries:

Tokens are used to represent unknown/fuzzy data. The 3 types of tokens are:

| token  | type    | description                           | example           | result-like                     |
| ------ | ------- | ------------------------------------- | ----------------- | ------------------------------- |
| `{x}`  | range   | 0 to `x` non-whitespace characters    | `"home{5}"`       | `home, homestead, homeward`     |
| `{!x}` | strict  | exactly `x` non-whitespace characters | `"{1}ward{!2}"`   | `warden, awarded`               |
| `{?}`  | unknown | 0 or more unknown **words**           | `"thou {?} kill"` | `thou shalt not kill`           |

__The `unknown` token must be segregated in the space between any 2 terms, exactly as illustrated in the above example.__

--------

## Documentation:

**note:**
`list|tuple|set` is aliased as `Iter` to simplify documentation. There is no `Iter` type in the `fuzzquery` package.

--------

### finditer(text, query, skip, ci)
> yield all (`span`, `match`) of a single query.

| arg      | description                                                       | type       |
| -------- | ----------------------------------------------------------------- | ---------- |
| `text`   | the text to search                                                | str        |
| `query`  | the query to search for                                           | str        |
| `skip`   | terms and/or characters that trigger a skip when found in results | Iter\|None |
| `ci`     | case-insensitive matching                                         | bool       |

--------

### findany(text, queries, skip, ci)
> `OR` queries together and yield all (`span`, `match`) of "whatever-is-next".

| arg       | description                                                       | type       |
| --------- | ----------------------------------------------------------------- | ---------- |
| `text`    | the text to search                                                | str        |
| `queries` | queries to combine for "whatever-is-next" search                  | Iter       |
| `skip`    | terms and/or characters that trigger a skip when found in results | Iter\|None |
| `ci`      | case-insensitive matching                                         | bool       |

--------

### iterall(text, queries, skip, ci)
> yield all (`query`, `span`, `match`) of multiple queries.

| arg       | description                                                       | type       |
| --------- | ----------------------------------------------------------------- | ---------- |
| `text`    | the text to search                                                | str        |
| `queries` | queries to search for                                             | Iter       |
| `skip`    | terms and/or characters that trigger a skip when found in results | Iter\|None |
| `ci`      | case-insensitive matching                                         | bool       |

--------

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
