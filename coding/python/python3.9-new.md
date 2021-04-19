# New in Python 3.9

## New way to merge dicts

[PEP-584](https://www.python.org/dev/peps/pep-0584/) introduced the `|` operator for `dict`!

```python
merged = d1 | d2
```

## String-method `removeprefix`

[PEP-616](https://www.python.org/dev/peps/pep-0616/) introduced `str.removeprefix` and `str.removesuffix` methods:

```python
'abcd'.removeprefix('ab')
# 'cd'

'abcd'.removeprefix('fg')
# 'abcd'
```

## `ast.unparse`

Python 3.9 introduces a new function [ast.unparse](https://docs.python.org/3.9/library/ast.html#ast.unparse). It accepts a parsed AST (Abstract Syntax Tree) and produces a Python code.

```python
import ast
tree = ast.parse('a=(1+2)+3 # example')
ast.unparse(tree)
# '\na = 1 + 2 + 3'
```

