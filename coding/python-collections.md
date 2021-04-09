## `Counter` is:

* [Multiset](https://en.wikipedia.org/wiki/Multiset) realization
* Dict with a default value and smart `update()`,
* Supports set and arithmetic operations,
* Can return N or all elements sorted by value `.most_common(3)`,
* Can merge 2 or more Counters,
* Drops negative values.


## [`Chainmap`](https://docs.python.org/3/library/collections.html#collections.ChainMap) 

dict-like class for creating a single view of multiple mappings

```python
from collections import ChainMap

a = {'key': 'a'}
b = {'key': 'b'}
c = {'key': 'c'}
d = ChainMap(c, b, a)
d['key']
# 'c'
```

