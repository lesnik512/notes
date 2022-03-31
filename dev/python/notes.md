# 1. `collections` package

## `Counter`:
* [Multiset](https://en.wikipedia.org/wiki/Multiset) realization
* Supports set and arithmetic operations
* Return elements sorted by value: `a.most_common(3)` or `a.most_common()`
* Counters can be merged: `a + b` or `a.update(b)`
* Negative values are dropped


## `Chainmap`
* dict-like class for creating a single view of multiple mappings

```python
from collections import ChainMap
a = {'key': 'a'}
b = {'key': 'b'}
d = ChainMap(b, a)
d['key']
# 'b'
```


# 2. "dunder"-methods

## class attribute
`__class_getitem__` - `__getitem__` for class: `MyList[int]`.


## data and non-data descriptors
If an object defines `__set__` or `__delete__`, it is considered a data descriptor. Descriptors that only define `__get__` are called non-data descriptors. The difference is that non-data descriptors are called only if the attribute isn't presented in `__dict__` of the instance.

## hashable
To make a class hashable, it has to implement both the `__hash__` and  `__eq__`
- The hash of an object must never change during its lifetime
- Hashable objects which compare equal must have the same hash value (and vice versa)

# class constructor
`__init_subclass__`. It is called on subclass creation and accepts the class and keyword arguments passed next to base classes. Let's see an example:

```python
speakers = {}
class Speaker:
  # `name` is a custom argument
  def __init_subclass__(cls, name=None):
    if name is None:
      name = cls.__name__
    speakers[name] = cls

class Guido(Speaker): pass
class Beazley(Speaker, name='David Beazley'): pass
speakers
# {'Guido': __main__.Guido, 'David Beazley': __main__.Beazley}
```

# types.DynamicClassAttribute

[types.DynamicClassAttribute](https://docs.python.org/3/library/types.html#types.DynamicClassAttribute) is a decorator that allows having a `@property` that behaves differently when it's called from the class and when from the instance.

```python
from types import DynamicClassAttribute

class Meta(type):
    @property
    def hello(cls):
        return f'hello from Meta ({cls})'

class C(metaclass=Meta):
    @DynamicClassAttribute
    def hello(self):
        return f'hello from C ({self})'

C.hello
# "hello from Meta (<class '__main__.C'>)"

C().hello
# 'hello from C (<__main__.C object ...)'
```

# `__prepare__`

Magic method `__prepare__` on metaclass is called on class creation. It must return a dict instance that then will be used as `__dict__` of the class. For example, it can be used to inject variables into the function scope:

```python
class Meta(type):
    def __prepare__(_name, _bases, **kwargs):
        d = {}
        for k, v in kwargs.items():
            d[k] = __import__(v)
        return d

class Base(metaclass=Meta):
    def __init_subclass__(cls, **kwargs):
      pass

class C(Base, m='math'):
    mypi = m.pi

C.mypi
# 3.141592653589793

C.m.pi
# 3.141592653589793
```

# dict as `__slots__`

`__slots__` can be used to save memory. You can use any iterable as `__slots__` value, including `dict`. And starting from Python 3.8, you can use `dict` to specify docstrings for slotted attributes `__slots__`:

```python
class Channel:
  "Telegram channel"
  __slots__ = {
    'slug': 'short name, without @',
    'name': 'user-friendly name',
  }
  def __init__(self, slug, name):
    self.slug = slug
    self.name = name

inspect.getdoc(Channel.name)
# 'user-friendly name'
```

# Snippets

## Call a function until a sentinel value
```python
from functools import partial
blocks = []
for block in iter(partial(input), ''):
    blocks.append(block)
blocks
```

## Ignore exception
```python
import os
from contextlib import contextmanager

@contextmanager
def ignored(*exceptions):
    try:
        yield
    except exceptions:
        pass

with ignored(OSError):
    os.remove('somefile.tmp')
```


## Slice vars

```python
xs = list(range(10))
every_second = slice(None, None, 2)
xs[every_second]
# [0, 2, 4, 6, 8]
```

## Grep with generator

```python
def grep(pattern):
    print("Looking for {!r}".format(pattern))
    while True:
        line = yield
        if pattern in line:
            print(line)

gen = grep("Gotcha!")
next(gen)
gen.send("This line doesn't have ... what we're looking for")
gen.send("This one does not!")
gen.send("This one does. Gotcha!")
# Looking for 'Gotcha!'
# This one does. Gotcha!
```

## Method resolution order

```python
class D:
    def get_some(self):
        print('C')

class A(D):
    def get_some(self):
        print('A')

class B:
    def get_some(self):
        print('B')

class C(A, B):
    pass

c = C()
C.mro()
#[__main__.C, __main__.A, __main__.D, __main__.B, object]
```

## Access item in multilevel `dict`

```python
from functools import reduce
from operator import getitem

d = {'a': {'b': {'c': 13}}}
path = 'a.b.c'
reduce(getitem, path.split('.'), d)
# 13
```

## Links
- About string type [[ru](https://habr.com/ru/post/480324/)]
- About `dict` [[ru](https://habr.com/ru/post/432996/)]
- About `int` [[ru](https://habr.com/ru/post/455114/)]