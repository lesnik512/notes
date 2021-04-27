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
