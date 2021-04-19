## Check if string is palindrome

```python
def is_pal(w):
    l = 0
    r = len(w) - 1
    while True:
        if l >= r:
            return True
        if w[l] == ' ':
            l += 1
            continue
        if w[r] == ' ':
            r -= 1
            continue
        if w[l] != w[r]:
            return False
        l += 1
        r -= 1
    
is_pal('m    ad am')
# True
```

## Sum of numbers as linked arrays

```python
class Node:
    def __init__(self, value, next_=None):
        self.value = value
        self.next = next_

    @classmethod
    def from_int(cls, n):
        value = n % 10
        return cls(value, cls.from_int(n // 10) if n // 10 > 0 else None)
        
    def __str__(self):
        return f'{str(self.next or "")}{self.value}'

    def __add__(self, b):
        return sum_lists(self, b)
    
def sum_lists(a: Node, b: Node, over = 0) -> Node:
    value = a.value + b.value + over
    if a.next is None and b.next is None:
        return Node(value % 10, Node(1) if value > 9 else None)

    next_ = sum_lists(a.next or Node(0), b.next or Node(0), value // 10)
    return Node(value % 10, next_)

a = Node.from_int(999991)
b = Node.from_int(9)
print(a,'+', b, '=', a + b)
# 999991 + 9 = 1000000
```
