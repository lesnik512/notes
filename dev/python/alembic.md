## New required field

```python
op.add_column('reservation_goals', sa.Column('is_work', sa.Boolean(), nullable=True))
op.execute("UPDATE reservation_goals SET is_work = false")
op.alter_column('reservation_goals', 'is_work', nullable=False)
```