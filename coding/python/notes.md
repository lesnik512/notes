# `__class_getitem__`

[PEP-560](https://www.python.org/dev/peps/pep-0560/) (landed in Python 3.7) introduced a new magic method `__class_getitem__`. it is the same as `__getitem__` but for not instancinated class. The main motivation is easier type annotation support for generic collections like `List[int]` or `Type[Dict[str, int]]`:

```python
class MyList:
  def __getitem__(self, index):
    return index + 1

  def __class_getitem__(cls, item):
    return f"{cls.__name__}[{item.__name__}]"

MyList()[1]
# 2

MyList[int]
# 'MyList[int]'
```

# data and non-data descriptors

[Descriptors](https://docs.python.org/3/howto/descriptor.html) are special class attributes with a custom behavior on attribute get, set, or delete. If an object defines `__set__` or `__delete__`, it is considered a data descriptor. Descriptors that only define `__get__` are called non-data descriptors. The difference is that non-data descriptors are called only if the attribute isn't presented in `__dict__` of the instance.

### Non-data descriptor:

```python
class D:
  def __get__(self, obj, owner):
    print('get', obj, owner)

class C:
    d = D()

c = C()
c.d
# get <C object at ...> <class 'C'>

# updating __dict__ shadows the descriptor
c.__dict__['d'] = 1
c.d
# 1
```

### Data descriptor:

```python
class D:
  def __get__(self, obj, owner):
    print('get', obj, owner)

  def __set__(self, obj, owner):
    print('set', obj, owner)

class C:
    d = D()

c = C()
c.d
# get <C object at ...> <class 'C'>

# updating __dict__ doesn't shadow the descriptor
c.__dict__['d'] = 1
c.d
# get <C object at ...> <class 'C'>
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

# `__init_subclass__` (PEP-487)


Python 3.6 introduced a few hooks to simplify things that could be done before only with metaclasses. Thanks to [PEP-487](https://www.python.org/dev/peps/pep-0487/). The most useful such hook is `__init_subclass__`. It is called on subclass creation and accepts the class and keyword arguments passed next to base classes. Let's see an example:

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

