# Pydantic: two-way mapping

With Pydantic we can build full-fledged two-way mapping using `alias` and [`allow_population_by_field_name`](https://pydantic-docs.helpmanual.io/usage/model_config/) setting in `Config`

Here is the model

```python
from pydantic import BaseModel, Field

class Item(BaseModel):
    item_id: str
    is_available: bool = Field(alias='isAvailable')
        
    class Config:
        allow_population_by_field_name = True

```

Thanks to `allow_population_by_field_name`, we can use both field name and alias to create new object:

```python
item1 = Item(item_id='test-item-id', is_available=True)
item2 = Item(item_id='test-item-id', isAvailable=True)
print(item1)
print(item2)

# Item(item_id='test-item-id', is_available=True),
# Item(item_id='test-item-id', is_available=True),
```

And here how we can get dictionary with field names and aliases:

```python
print(item1.dict())
print(item1.dict(by_alias=True))

# {'item_id': 'test-item-id', 'is_available': True}
# {'item_id': 'test-item-id', 'isAvailable': True}
```