## Fixtures with non-function scope and database access
```python
@pytest.fixture(scope='module')
def some_fixture(django_db_setup, django_db_blocker):
    with django_db_blocker.unblock():
        pass
        # setup
    yield app
    with django_db_blocker.unblock():
        pass
        # teardown
```
