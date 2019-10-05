### 一、行转列
``` pgsql
SELECT
	string_agg(id::varchar, '-') AS "INT数据行转String列",
	string_agg(mobile, '-') AS "VARCHAR数据行转String列",
	array_agg(id) AS "INT数据转数组",
	pg_typeof(array_agg(id)) AS "INT数据转数组后的类型",
	array_agg(mobile) AS "VARCHAR数据转数组",
	pg_typeof(array_agg(mobile)) AS "INT数据转数组后的类型",
	json_agg(id) AS "INT数据转JSON数组",
	json_agg(mobile) AS "VARCHAR数据转JSON数组"
FROM "user";
```

### 二、多行转JSON数据
```pgsql
SELECT json_agg(row_to_json(dividion_province)) FROM dividion_province;
```

### 三、数组转换
```pgsql
SELECT
	"group" AS "数组数据",
	array_to_json("group") AS "数组转JSON数组数据"
	array_to_string("group",'_') AS "数组转字符串数据",
	string_to_array(array_to_string("group",'_'), '_') AS "字符串转数组数据",
	pg_typeof("group") AS "数组数据类型",
	pg_typeof(array_to_json("group")) AS "JSON数据类型"
FROM "user"
```

### 四、JSON数组 转 数组
```pgsql
SELECT
	array_agg(vaule) AS "JSON数组转数组",
	array_to_json(array_append(array_agg(vaule), json_build_object('name', 'Gadfly02', 'age', 44))) AS "JSON数组后添加数据",
	array_to_json(array_prepend(json_build_object('name', 'Gadfly02', 'age', 44), array_agg(vaule))) AS "JSON数组前添加数据",
	array_to_json(array_cat(ARRAY[json_build_object('name', 'Gadfly02', 'age', 44), json_build_object('name', 'Gadfly02', 'age', 45)], array_agg(vaule))) AS "JSON数组前添加数据"
FROM json_array_elements(json_build_array(json_build_object('name', 'Gadfly00', 'age', 43), json_build_object('name', 'Gadflyd01', 'age', 43))) as vaule;
```

### 五、获取insert插入的新纪录的id值
> 使用PostgreSQL的RETURNING语句来实现插入时快速获取insert id
>> 在INSERT INTO或者UPDATE的时候在最后面加上RETURNING colname，PostgreSQL会在插入或者更新数据之后会返回你指定的字段。
```pgsql
INSERT INTO test(name) values('name') RETURNING id;
```

### 六、PostgreSQL 动态SQL语句
  * 动态SQL语句中正确使用SELECT INTO语句
  ```pgsql
  sqlstring:= 'select max("' || idname || '") from "' || tablename || '";';  
  EXECUTE sqlstring into currentId; 
  ```
  * 在用户自定义函数中用 RETURN QUERY 返回一个表
  ```pgsql
  CREATE or Replace FUNCTION func_get_task_view(  
    sdkv integer)  
  RETURNS setof task_view AS $$  
  Declare  
      queryString varchar(260);  
  Begin  
      queryString := 'select * from task_view where sdvk = $1 order by id desc';  
      return query execute queryString using sdkv;  
  return;  
  End;  
  $$ LANGUAGE plpgsql;
  ```
  
### 七、获取指定表或视图的数据结构
```sql
SELECT col_description(pg_attribute.attrelid, pg_attribute.attnum) AS comment,
			 pg_type.typname AS datetype,
			 pg_attribute.attname AS field,
			 pg_attribute.attnotnull AS notnull
FROM pg_class, pg_attribute INNER JOIN pg_type ON pg_type.oid = pg_attribute.atttypid
WHERE pg_class.relname = 'base_lottery_log'
	AND pg_attribute.attrelid = pg_class.oid
	AND pg_attribute.attnum > 0;
```

### 八、获取指定表的主键信息
```sql
SELECT pg_attribute.attname AS pk_name,
       col_description(pg_attribute.attrelid, pg_attribute.attnum) AS comment,
       pg_type.typname AS datetype
FROM pg_constraint INNER JOIN pg_class ON pg_constraint.conrelid = pg_class.oid
                   INNER JOIN pg_attribute ON pg_attribute.attrelid = pg_class.oid
	                                              AND pg_attribute.attnum = pg_constraint.conkey[1]
                   INNER JOIN pg_type ON pg_type.oid = pg_attribute.atttypid
WHERE pg_class.relname = 'base_lottery_log' AND pg_constraint.contype = 'p'
```