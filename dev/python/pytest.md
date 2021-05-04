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

## Global Mock

```python
@pytest.fixture(autouse=True)
def disable_network_calls(monkeypatch):
    def stunted_get():
        raise RuntimeError("Network access not allowed during testing!")
    monkeypatch.setattr(requests, "get", lambda *args, **kwargs: stunted_get())
```

## Running options

`pytest -m database_access` - run tests marked by `@pytest.mark.database_access`
`pytest -m "not database_access"` - run all tests except for `database_access`

```shell
$ pytest --durations=3
3.03s call     test_code.py::test_request_read_timeout
1.07s call     test_code.py::test_request_connection_timeout
0.57s call     test_code.py::test_database_read
======================== 7 passed in 10.06s ==============================
```