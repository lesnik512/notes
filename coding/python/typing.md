## Mypy expressions: reveal_type() and reveal_locals()

```python
# reveal.py
import math
reveal_type(math.pi)

radius = 1
circumference = 2 * math.pi * radius
reveal_locals()
```

```bash
$ mypy reveal.py
reveal.py:4: error: Revealed type is 'builtins.float'
reveal.py:8: error: Revealed local types are:
reveal.py:8: error: circumference: builtins.float
reveal.py:8: error: radius: builtins.int
```

## Type Comments

```python
import math

def circumference(radius):
    # type: (float) -> float
    return 2 * math.pi * radius

# headlines.py
def headline(
    text,           # type: str
    width=80,       # type: int
    fill_char="-",  # type: str
):                  # type: (...) -> str
    return f" {text.title()} ".center(width, fill_char)

print(headline("type comments work", width=40))
```

## Subtypes

One important concept is that of subtypes. Formally, we say that a type T is a subtype of U if the following two conditions hold:
- Every value from T is also in the set of values of U type.
- Every function from U type is also in the set of functions of T type.

## Covariant, Contravariant, and Invariant

- Tuple is covariant. This means that it preserves the type hierarchy of its item types: Tuple[bool] is a subtype of Tuple[int] because bool is a subtype of int.

- List is invariant. Invariant types give no guarantee about subtypes. While all values of List[bool] are values of List[int], you can append an int to List[int] and not to List[bool]. In other words, the second condition for subtypes does not hold, and List[bool] is not a subtype of List[int].

- Callable is contravariant in its arguments. This means that it reverses the type hierarchy. You will see how Callable works later, but for now think of Callable[[T], ...] as a function with its only argument being of type T.

The type T is **consistent** with the type U if T is a subtype of U or either T or U is Any.

## Protocols

- `typing.Sized` - has \_\_len\_\_ method implemented

Custom protocol:

```python
from typing_extensions import Protocol

class Sized(Protocol):
    def __len__(self) -> int: ...

def len(obj: Sized) -> int:
    return obj.__len__()
```

## Sources:

- [Python Type Checking (Guide)](https://realpython.com/python-type-checking/)
