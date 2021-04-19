## Patching in the wrong place

`data_source.py`
```python
def get_name():
    return "Alice"
```


`person.py`
```python
from data_source import get_name

class Person(object):
    def name(self):
        return get_name()
```

This won't work:

`test_person.py`
```python
from mock import patch
from person import Person

# mock the get_name function
@patch('data_source.get_name') # This won't work as expected!
def test_name(mock_get_name):
    # set a return value for our mock object
    mock_get_name.return_value = "Bob" 
    person = Person()
    name = person.name()
    assert name == "Bob"
```

But this will:

`person.py`
```python
import data_source

class Person(object):
    def name(self):
        return data_source.get_name()
```