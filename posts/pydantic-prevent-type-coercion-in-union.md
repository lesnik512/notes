I faced with the problem with type coercion in `Union` when resulting type depends on the order of types in annotation:

```python
from pydantic import BaseModel
from typing import Union

class TestModel(BaseModel):
    value: Union[bool, float, int, str]

print(TestModel(value=True).value)
print(TestModel(value=1).value)
print(TestModel(value=1.).value)
print(TestModel(value="no").value)

# True
# True
# True
# False
```

And this problem can be solved by [Strict Types](https://pydantic-docs.helpmanual.io/usage/types/#strict-types):

```python
from pydantic import (
    BaseModel,
    StrictStr,
    StrictBool,
    StrictInt,
    StrictFloat
)
from typing import Union, List

class TestModel(BaseModel):
    value: Union[StrictBool, StrictFloat, StrictInt, StrictStr, List[StrictBool], List[StrictFloat], List[StrictInt], List[StrictStr]]

print(TestModel(value=True).value)
print(TestModel(value=1.).value)
print(TestModel(value=1).value)
print(TestModel(value="no").value)

# True
# 1.0
# 1
# no

print(TestModel(value=[True, False]).value)
print(TestModel(value=[1, 2]).value)
print(TestModel(value=[1., 2.]).value)

# [True, False]
# [1, 2]
# [1.0, 2.0]
```
