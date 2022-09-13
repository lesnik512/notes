# typing.Protocol

```python
from typing import Protocol


class Flyer(Protocol):
    def fly(self) -> None:
        """A Flyer can fly"""


class FlyingHero:
    """This hero can fly, which is BEAST."""
    def fly(self):
        # Do some flying...


class RunningHero:
    """This hero can run. Better than nothing!"""
    def run(self):
        # Run for your life!


class Board:
    """An imaginary game board that doesn't do anything."""
    def make_fly(self, obj: Flyer) -> None:   # <- Here's the magic
        """Make an object fly."""
        return obj.fly()


board = Board()
board.make_fly(FlyingHero())
board.make_fly(RunningHero())  # <- Fails mypy type-checking!
```

## Recursive protocols

Protocols can be recursive (self-referential) and mutually recursive. This is useful for declaring abstract recursive collections such as trees and linked lists:

```python
from typing import TypeVar, Optional
from typing_extensions import Protocol

class TreeLike(Protocol):
    value: int

    @property
    def left(self) -> Optional['TreeLike']: ...

    @property
    def right(self) -> Optional['TreeLike']: ...

class SimpleTree:
    def __init__(self, value: int) -> None:
        self.value = value
        self.left: Optional['SimpleTree'] = None
        self.right: Optional['SimpleTree'] = None

root: TreeLike = SimpleTree(0)  # OK
```

