# "dunder"-methods

## data and non-data descriptors
If an object defines `__set__` or `__delete__`, it is considered a data descriptor. Descriptors that only define `__get__` are called non-data descriptors. The difference is that non-data descriptors are called only if the attribute isn't presented in `__dict__` of the instance.

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