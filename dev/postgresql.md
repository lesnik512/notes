## Cache hit rate
```sql
SELECT 
  sum(heap_blks_read) as heap_read,
  sum(heap_blks_hit)  as heap_hit,
  sum(heap_blks_hit) / (sum(heap_blks_hit) + sum(heap_blks_read)) as ratio
FROM 
  pg_statio_user_tables;
```

## Index usage rate
```sql
SELECT 
  relname, 
  100 * idx_scan / (seq_scan + idx_scan) percent_of_times_index_used, 
  n_live_tup rows_in_table
FROM 
  pg_stat_user_tables
WHERE 
    seq_scan + idx_scan > 0 
ORDER BY 
  n_live_tup DESC;
```

## Index Cache Hit Rate
Finally to combine the two if you’re interested in how many of your indexes are within your cache you can run:
```sql
SELECT 
  sum(idx_blks_read) as idx_read,
  sum(idx_blks_hit)  as idx_hit,
  (sum(idx_blks_hit) - sum(idx_blks_read)) / sum(idx_blks_hit) as ratio
FROM 
  pg_statio_user_indexes;
```

## Tips
- use CREATE INDEX CONCURRENTLY to add an index on a production database in the background and not hold a lock on your table. The limitation to creating indexes concurrently is they can typically take 2-3 times longer  to create and can’t be run within a transaction.
