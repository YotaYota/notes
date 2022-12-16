# SQL

## Indexes

An index speeds up an additional table with the indexed column sorted.


Speeds up things used in `WHERE` or `JOIN ON` clauses.

For multicolumn indexes, order matters.

`(a, b)` also speeds up queries of the type

```sql
WHERE a = x
```

