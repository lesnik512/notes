1. Using `itertools.product` instead of nested loop

```python
for x in listA:
    for y in listB:
        r.append((x, y))

for x, y in itertools.product(listA, listB):     
    r.append((x, y))
```
