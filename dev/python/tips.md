1. Using `itertools.product` instead of nested loop

```python
for x in listA:
    for y in listB:
        r.append((x, y))
```

Using itertools not only gives a performance boost but is also flat and clean. Just look how clean the above code would look with itertools.product()

```python
for x, y in itertools.product(listA, listB):     
    r.append((x, y))
```
