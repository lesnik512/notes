## The Naming Algorithm

1. Is the function a test? -> `test_<entity>_<behavior>`.
1. Does the function has a @property decorator? -> don’t use a verb in the function name.
1. Does the function use a disk or a network:
    1. to store data? -> `save_to`, `send`, `write_to`
    1. to receive data? -> fetch, load, read
1. Does the function output any data? -> `print`, `output`
1. Returns boolean value? -> `is_`, `has_/have_`, `can_`, `check_if_<entity>_<characteristic>`
1. Aggregates data? -> `calculate`, `extract`, `analyze`
1. Put data from one form to another:
    1. Creates a single meaningful object? -> `create`
    1. Fills an existing object with data? -> `initialize`, `configure`
    1. Clean raw data? -> `clean`
    1. Receive a string as input? -> `parse`
    1. Return a string as output? -> `render`
    1. Return an iterator as output? -> `iter`
    1. Mutates its arguments or some global state? -> `update`, `mutate`, `add`, `remove`, `insert`, `set`
    1. Return a list of errors? -> `validate`
    1. Checks data items recursively? -> `walk`
    1. Finds appropriate item in data? -> `find`, `search`, `match`
    1. Transform data type? -> `<sth>_to_<sth_else>`
    1. None of the above, but still works with data? -> Check one of those: `morph`, `compose`, `prepare`, `extract`, `generate`, `initialize`, `filter`, `map`, `aggregate`, `export`, `import`, `normalize`, `calculate`.

## Модели БД

- Название модели пишется в единственном числе: User, UserAnswer
- Название таблицы, если оно задается вручную, пишется во множественном числе в underscore: users, user_answers.
- Поля модели и колонки в таблице пишутся в underscore: login, first_name.
- Поля типа boolean начинаются с is_: is_admin, is_hidden.
- Поля типов DATETIME заканчиваются на _at: created_at, updated_at.
- Поля типов DATE заканчиваются на _date: connect_date, end_date.

### The Blacklist
`get`, `run`, `process`, `make`, `handle`, `do`, `main`, `compare`.

[Source](https://melevir.medium.com/python-functions-naming-the-algorithm-74320a18278d)