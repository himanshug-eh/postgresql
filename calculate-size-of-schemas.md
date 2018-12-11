# How to Calculate Size of Each Schema?

```
postgres# SELECT schemaname, pg_size_pretty(SUM(pg_total_relation_size(quote_ident(schemaname) || '.' || quote_ident(tablename)))::BIGINT) FROM pg_tables GROUP BY schemaname;
```
