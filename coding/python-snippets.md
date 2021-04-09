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

