## Creating models without validation

```python
class User(BaseModel):
    id: int
    age: int
    name: str = 'John Doe'

new_user = User.construct(**{"id": 123, "age": 30})
```

## Generic response

```python
from typing import Generic, TypeVar, List

from pydantic import BaseModel
from pydantic.generics import GenericModel


DataT = TypeVar('DataT')

class Response(GenericModel, Generic[DataT]):
    items: List[DataT]

print(Response[int](data=List[1]))
#> data=[1]
```
## Dynamic model creation

Here `StaticFoobarModel` and `DynamicFoobarModel` are identical.

```python
from pydantic import BaseModel, create_model

DynamicFoobarModel = create_model('DynamicFoobarModel', foo=(str, ...), bar=123)


class StaticFoobarModel(BaseModel):
    foo: str
    bar: int = 123
```


## Posts:

- [Pydantic, two-way mapping](https://shiriev.ru/pydantic-two-way-mapping/)
- [Pydantic, preventing type coercion in Union type](https://shiriev.ru/pydantic-prevent-type-coercion-in-union/)