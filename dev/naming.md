## The Naming Algorithm
1. Is the function a test? -> `test_<entity>_<behavior>`.
2. Does the function has a @property decorator? -> don’t use a verb in the function name.
3. Does the function use a disk or a network:
    1. to store data? -> `save_to`, `send`, `write_to`
    2. to receive data? -> fetch, load, read
4. Does the function output any data? -> `print`, `output`
5. Returns boolean value? -> `is_`, `has_/have_`, `can_`, `check_if_<entity>_<characteristic>`
6. Aggregates data? -> `calculate`, `extract`, `analyze`
7. Put data from one form to another:
    1. Creates a single meaningful object? -> `create`
    2. Fills an existing object with data? -> `initialize`, `configure`
    3. Clean raw data? -> `clean`
    4. Receive a string as input? -> `parse`
    5. Return a string as output? -> `render`
    6. Return an iterator as output? -> `iter`
    7. Mutates its arguments or some global state? -> `update`, `mutate`, `add`, `remove`, `insert`, `set`
    8. Return a list of errors? -> `validate`
    9. Checks data items recursively? -> `walk`
    10. Finds appropriate item in data? -> `find`, `search`, `match`
    11. Transform data type? -> `<sth>_to_<sth_else>`
    12. None of the above, but still works with data? -> Check one of those: `morph`, `compose`, `prepare`, `extract`, `generate`, `initialize`, `filter`, `map`, `aggregate`, `export`, `import`, `normalize`, `calculate`.

### The Blacklist
`get`, `run`, `process`, `make`, `handle`, `do`, `main`, `compare`.

## Модели БД
- Название модели пишется в единственном числе: User, UserAnswer
- Название таблицы, если оно задается вручную, пишется во множественном числе в underscore: users, user_answers.
- Поля модели и колонки в таблице пишутся в underscore: login, first_name.
- Поля типа boolean начинаются с is_: is_admin, is_hidden.
- Поля типов DATETIME заканчиваются на _at: created_at, updated_at.
- Поля типов DATE заканчиваются на _date: connect_date, end_date.
