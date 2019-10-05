```sql
SELECT 
  c.relname as tabname, 
  cast(obj_description(c.relfilenode,'pg_class') as varchar) as comment, 
  a.attnum,
  a.attname, 
  d.description,
  concat_ws('',t.typname,SUBSTRING(format_type(a.atttypid,a.atttypmod) from '\(.*\)')) as type
FROM pg_class c, pg_attribute a , pg_type t, pg_description d
WHERE a.attnum>0 and a.attrelid = c.oid and a.atttypid = t.oid and d.objoid=a.attrelid and d.objsubid=a.attnum;
```