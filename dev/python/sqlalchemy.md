# Code Snippets

## INSERT…FROM SELECT

```python
>>> select_stmt = select(user_table.c.id, user_table.c.name + "@aol.com")
>>> insert_stmt = insert(address_table).from_select(
...     ["user_id", "email_address"], select_stmt
... )
>>> print(insert_stmt)
```
```sql
INSERT INTO address (user_id, email_address)
SELECT user_account.id, user_account.name || :name_1 AS anon_1
FROM user_account
```

## Selecting from Labeled SQL Expressions

```python
>>> from sqlalchemy import func, cast
>>> stmt = (
...     select(
...         ("Username: " + user_table.c.name).label("username"),
...     ).order_by(user_table.c.name)
... )
>>> with engine.connect() as conn:
...     for row in conn.execute(stmt):
...         print(f"{row.username}")
```
```sql
BEGIN (implicit)
SELECT ? || user_account.name AS username
FROM user_account ORDER BY user_account.name
[...] ('Username: ',)
```
```
Username: patrick
Username: sandy
Username: spongebob
```

## Simple “equality” comparisons against a single entity

```python
>>> print(
...     select(User).filter_by(name='spongebob', fullname='Spongebob Squarepants')
... )
```
```sql
SELECT user_account.id, user_account.name, user_account.fullname
FROM user_account
WHERE user_account.name = :name_1 AND user_account.fullname = :fullname_1
```

## Aggregate functions with GROUP BY / HAVING

```python
>>> with engine.connect() as conn:
...     result = conn.execute(
...         select(User.name, func.count(Address.id).label("count")).
...         join(Address).
...         group_by(User.name).
...         having(func.count(Address.id) > 1)
...     )
...     print(result.all())
```
```sql
BEGIN (implicit)
SELECT user_account.name, count(address.id) AS count
FROM user_account JOIN address ON user_account.id = address.user_id GROUP BY user_account.name
HAVING count(address.id) > ?
[...] (1,)
```

## Using Aliases

```python
>>> user_alias_1 = user_table.alias()
>>> user_alias_2 = user_table.alias()
>>> print(
...     select(user_alias_1.c.name, user_alias_2.c.name).
...     join_from(user_alias_1, user_alias_2, user_alias_1.c.id > user_alias_2.c.id)
... )
```
```sql
SELECT user_account_1.name, user_account_2.name AS name_1
FROM user_account AS user_account_1
JOIN user_account AS user_account_2 ON user_account_1.id > user_account_2.id
```

## EXISTS subqueries

```python
>>> subq = (
...     select(address_table.c.id).
...     where(user_table.c.id == address_table.c.user_id)
... ).exists()
>>> with engine.connect() as conn:
...     result = conn.execute(
...         select(user_table.c.name).where(~subq)
...     )
...     print(result.all())
```
```sql
BEGIN (implicit)
SELECT user_account.name
FROM user_account
WHERE NOT (EXISTS (SELECT address.id
FROM address
WHERE user_account.id = address.user_id))
[...] ()
```

## UPDATE many

```python
>>> from sqlalchemy import bindparam
>>> stmt = (
...   update(user_table).
...   where(user_table.c.name == bindparam('oldname')).
...   values(name=bindparam('newname'))
... )
>>> with engine.begin() as conn:
...   conn.execute(
...       stmt,
...       [
...          {'oldname':'jack', 'newname':'ed'},
...          {'oldname':'wendy', 'newname':'mary'},
...          {'oldname':'jim', 'newname':'jake'},
...       ]
...   )
```
```sql
BEGIN (implicit)
UPDATE user_account SET name=? WHERE user_account.name = ?
[...] (('ed', 'jack'), ('mary', 'wendy'), ('jake', 'jim'))
<sqlalchemy.engine.cursor.CursorResult object at 0x...>
COMMIT
```

## EXISTS forms: has() / any()

```python
>>> stmt = (
...   select(User.fullname).
...   where(User.addresses.any(Address.email_address == 'pearl.krabs@gmail.com'))
... )
>>> session.execute(stmt).all()
```
```sql
SELECT user_account.fullname
FROM user_account
WHERE EXISTS (SELECT 1
FROM address
WHERE user_account.id = address.user_id AND address.email_address = ?)
[...] ('pearl.krabs@gmail.com',)
```
---
```python
>>> stmt = (
...   select(User.fullname).
...   where(~User.addresses.any())
... )
>>> session.execute(stmt).all()
```
```sql
SELECT user_account.fullname
FROM user_account
WHERE NOT (EXISTS (SELECT 1
FROM address
WHERE user_account.id = address.user_id))
[...] ()
```
---
```python
>>> stmt = (
...   select(Address.email_address).
...   where(Address.user.has(User.name=="pkrabs"))
... )
>>> session.execute(stmt).all()
```
```sql
SELECT address.email_address
FROM address
WHERE EXISTS (SELECT 1
FROM user_account
WHERE user_account.id = address.user_id AND user_account.name = ?)
[...] ('pkrabs',)
[('pearl.krabs@gmail.com',), ('pearl@aol.com',)]
```

# Notes

There is internal support for the psycopg2 dialect to INSERT many rows at once and also support RETURNING, which is leveraged by the SQLAlchemy ORM. However this feature has not been generalized to all dialects and is not yet part of SQLAlchemy’s regular API.

