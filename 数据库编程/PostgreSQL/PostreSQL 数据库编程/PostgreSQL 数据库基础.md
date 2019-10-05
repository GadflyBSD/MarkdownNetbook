* # [官方中文文档](http://www.postgres.cn/docs/10/index.html) (http://www.postgres.cn/docs/10)
* # 创建、变更和删除数据库、构架
  * ## 创建数据库
    ```pgsql
    CREATE DATABASE test_db; 
    ```
  * ## 变更数据库
    ```pgsql
    ALTER DATABASE test_db RENAME TO test_db_1;    -- 数据库改名
    ALTER DATABASE test_db_1 OWNER TO gadfly;      -- 更改数据库的拥有者
    ```
  * ## 删除数据库
    ```pgsql
    DROP DATABASE test_db_1;
    ```
  * ## 创建构架（模式）
    ```pgsql
    CREATE SCHEMA test_schema;
    ```
  * ## 变更构架（模式）
    ```pgsql
    ALTER SCHEMA test_schema RENAME TO test_schema_new;   -- 更改架构（模式）的名称
    ALTER SCHEMA test_schema_new OWNER TO gadfly;         -- 更改架构（模式）的拥有者
    ```
  * ## 删除构架（模式）
    ```pgsql
    DROP SCHEMA test_schema_new;
    ```
* # 数据结构（数据表的创建、变更和删除）
  * ## 基础数据类型
    * ### 数字类型
      名字|存储尺寸|描述|范围
      -|-|-|-
      smallint|2字节|小范围整数|-32768 to +32767
      integer|4字节|整数的典型选择|-2147483648 to +2147483647
      bigint|8字节|大范围整数|-9223372036854775808 to +9223372036854775807
      decimal|可变|用户指定精度，精确|最高小数点前131072位，以及小数点后16383位
      numeric|可变|用户指定精度，精确|最高小数点前131072位，以及小数点后16383位
      real|4字节|可变精度，不精确|6位十进制精度
      double precision|8字节|可变精度，不精确|15位十进制精度
      smallserial|2字节|自动增加的小整数|1到32767
      serial|4字节|自动增加的整数|1到2147483647
      bigserial|8字节|自动增长的大整数|1到9223372036854775807
    * ### 货币类型
      名字|存储尺寸|描述|范围
      -|-|-|-
      money|8 bytes|货币额|-92233720368547758.08到+92233720368547758.07
    * ### 字符类型
      名字|描述
      -|-
      character varying(n), varchar(n)|有限制的变长
      character(n), char(n)|定长，空格填充
      text|无限变长

    * ### 二进制数据类型
      名字|存储尺寸|描述
      -|-|-
      bytea|1或4字节外加真正的二进制串|变长二进制串
    * ### 日期/时间类型
      名字|存储尺寸|描述|最小值|最大值|解析度
      -|-|-|-|-|-
      timestamp [ (p) ] [ without time zone ]|8字节|包括日期和时间（无时区）|4713 BC|	294276 AD|1微秒 / 14位
      timestamp [ (p) ] with time zone|8字节|包括日期和时间，有时区|4713 BC|294276 AD|1微秒 / 14位
      date|4字节|日期（没有一天中的时间）|4713 BC|5874897 AD|1日
      time [ (p) ] [ without time zone ]|8字节|一天中的时间（无日期）|00:00:00|24:00:00|1微秒 / 14位
      time [ (p) ] with time zone|12字节|一天中的时间（不带日期），带有时区|00:00:00+1459|24:00:00-1459|1微秒 / 14位
      interval [ fields ] [ (p) ]|16字节|时间间隔|-178000000年|178000000年|1微秒 / 14位
    * ### 布尔类型
      名字|存储字节|描述
      -|-|-
      boolean|1字节|状态为真或假
    * ### 枚举类型
      > 枚举类型需要先声明后使用
      ```pgsql
      CREATE TYPE happiness AS ENUM ('happy', 'very happy', 'ecstatic');
      ```
      
    * ### 网络地址类型
      名字|存储尺寸|描述
      -|-|-
      cidr|7或19字节|IPv4和IPv6网络
      inet|7或19字节|IPv4和IPv6主机以及网络
      macaddr|6字节|MAC地址
      macaddr8|8 字节|MAC 地址 (EUI-64 格式)
    * ### UUID类型
      > 一个UUID被写成一个小写十六进制位的序列，该序列被连字符分隔成多个组：首先是一个8位组，接下来是三个4位组，最后是一个12位组。总共的32位
      ```pgsql
      a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11
      CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
      ```
      
    * ### JSON 和 XML 类型
      > 用来存储标准的JSON格式或XML格式数据
      
  * ## 约束
    * ### 主键约束
      > 主键约束表示某列或某几列在行中是唯一的非空的。一般用`serial`或`uuid`类型。
      ```pgsql
      CREATE TABLE category(
          id SERIAL,
          code VARCHAR(10),
          lable VARCHAR(200),
          PRIMARY KEY (id)
      );
      ```
    * ### 外键约束
      > 外键约束指定一列（或一组列）中的值必须匹配出现在另一个表中某些行的值。
      
      ```pgsql
      CREATE TABLE vehicle(
          id SERIAL,
          cid INTEGER,
          name text,
          price money
          PRIMARY KEY (id),
          FOREIGN KEY (cid) REFERENCES category(id)
      );
      ```
    * ### 默认值约束
      > 列可以被分配一个默认值
      
      ```pgsql
      CREATE TABLE products (
          id SERIAL,
          product_no UUID DEFAULT uuid_generate_v4(),
          name text NOT NULL,
          price money DEFAULT 9.99,
          datetime TIMESTAMP(3) WITHOUT TIME ZONE DEFAULT now(),
          PRIMARY KEY (id, product_no),
      );
      ```
    * ### 非空约束
      > 非空约束指定一个列中不能有空值。
      
      ```pgsql
      CREATE TABLE products (
          product_no integer NOT NULL,
          name text NOT NULL,
          price money
      );
      ```
    * ### 检查校验约束
      ```pgsql
      CREATE TABLE products (
          product_no integer NOT NULL,
          name text NOT NULL,
          -- 列约束，产品价格为正值，简单模式
          price money CHECK (price > 0),
          -- 列约束，打折价格为正值，完整模式
          discounted_price money CONSTRAINT products_discounted_price CHECK (discounted_price > 0),
          -- 行约束，打折价格小于产品价格
          CHECK (price > discounted_price)
      );
      ```
    * ### 唯一值约束
      > 唯一约束校验数据所在列中的值在表中所有行间同列是唯一的
      
      ```pgsql
      CREATE TABLE products (
          product_no integer UNIQUE,
          name text,
          price numeric
      );
      ```
  * ## 表和字段的解释说明
    * ### 定义
      ```pgsql
      CREATE TABLE dictionary(
      	  id SERIAL,
      	  code VARCHAR(10),
      	  cate VARCHAR(10),
      	  lable VARCHAR(200),
      	  PRIMARY KEY (id)
      );
      COMMENT ON TABLE dictionary IS '字典数据表';
      COMMENT ON COLUMN dictionary.id IS '自增主键';
      COMMENT ON COLUMN dictionary.code IS '字典编码';
      COMMENT ON COLUMN dictionary.cate IS '字典编码';
      COMMENT ON COLUMN dictionary.lable IS '编码说明';
      ```
    * ### 用途
      ```pgsql
      CREATE VIEW formation_view as
        SELECT c.relname     AS table_name,
               a.attname     AS field,
               t.typname     AS type,
               b.description AS comment
        FROM pg_class c,
             (pg_attribute a
        	     LEFT JOIN pg_description b ON (((a.attrelid = b.objoid) AND (a.attnum = b.objsubid)))),
             pg_type t
        WHERE  (a.attnum > 0) AND (a.attrelid = c.oid) AND
               (a.atttypid = t.oid)
        ORDER BY a.attnum;
        
        select *  from  formation_view where table_name='base_user';
      ```
  * ## 对数据库表结构的其他操作
    * ### 更改一个表的定义
      ```pgsql
      -- 向dictionary表增加一个类型为varchar列名为address的列
      ALTER TABLE dictionary ADD COLUMN address varchar(30);
      -- 要从表中删除一列
      ALTER TABLE dictionary DROP COLUMN address RESTRICT;
      -- 更改现有列的类型
      ALTER TABLE dictionary ALTER COLUMN address TYPE varchar(80),
      -- 重命名一个现有的表
      ALTER TABLE dictionary RENAME TO suppliers;
      -- 为一列增加一个非空约束
      ALTER TABLE dictionary ALTER COLUMN lable SET NOT NULL;
      -- 从一列移除一个非空约束
      ALTER TABLE dictionary ALTER COLUMN lable DROP NOT NULL;
      ```
    * ### 删除一张表
      ```pgsql
      -- 如果存在dictionary表则删除，不存在则什么也不做
      DROP TABLE IF EXISTS dictionary;
      -- 删除dictionary表，如果不存在则报错
      DROP TABLE dictionary;
      ```
* # 数据库查询
  > 从数据库中检索数据的过程或命令叫做查询。在 SQL 里SELECT命令用于指定查询。
  * ## 简单形式的查询
    ```pgsql
    DROP TABLE IF EXISTS test_schema.student;
    CREATE TABLE test_schema.student (
	     "id" SERIAL,
		    "name" VARCHAR(10),
	      "chinese" INTEGER,
	      "english" INTEGER,
	      "mathematical" INTEGER,
	      "class" VARCHAR(10),
	      PRIMARY KEY (id)
    );
    INSERT INTO test_schema.student(name, chinese, english, mathematical, class)
    VALUES('李雯', 89, 92, 78, '初一一班'),('张三', 98, 96, 95, '初一一班'),
           ('李思', 90, 91, 88, '初一二班'),('王倩', 78, 79, 78, '初一二班'),
           ('刘备', 84, 76, 87, '初一三班'),('张飞', 88, 91, 84, '初一三班'),
           ('矛盾', 84, 76, 87, '初一四班'),('鲁迅', 88, 91, 84, '初一四班');
      
    -- 查询输出test_schema.student表中的所有行和列
    SELECT * FROM test_schema.student;         
    -- 查询输出test_schema.student表中的所有行中指定的name, chinese, english, mathematical三列
    SELECT name, chinese, english, mathematical FROM test_schema.student;
    -- 查询输出student表中的所有行中指定列运算后的值
    SELECT name, (chinese + english + mathematical) AS total FROM test_schema.student;
    -- 直接进行运算查询
    SELECT 3 * 4;
    -- 调用函数或存储过程
    SELECT now();
    ```
  * ## 查询表表达式
    * ### `FROM` 子句，代表数据来源
      > `table_reference`可以是一个或多个表名字、子查询，多个用逗号分隔
      ```pgsql
      FROM table_reference [, table_reference [, ...]]
      ```
    * ### `DISTINCT` 字段去重
      ```pgsql
      SELECT DISTINCT chinese FROM test_schema.student;
      ```
    * ### `WHERE` 子句，代表查询条件
      ```pgsql
      WHERE search_condition
      ```
      比较操作符
      操作符|描述
      -|-
      <|小于
      >|大于
      <=|小于等于
      >=|大于等于
      =|等于
      <> or !=|不等于
      a BETWEEN x AND y|在x和y之间
      a NOT BETWEEN x AND y|不在x和y之间
      a BETWEEN SYMMETRIC x AND y|在对比较值排序后位于x和y之间
      a NOT BETWEEN SYMMETRIC x AND y|在对比较值排序后不位于x和y之间
      a IS DISTINCT FROM b|不等于，空值被当做一个普通值
      a IS NOT DISTINCT FROM b|等于，空值被当做一个普通值
      expression IS NULL|是空值
      expression IS NOT NULL|不是空值
      expression ISNULL|是空值（非标准语法）
      expression NOTNULL|不是空值（非标准语法）
      boolean_expression IS TRUE|为真
      boolean_expression IS NOT TRUE|为假或未知
      boolean_expression IS FALSE|为假
      boolean_expression IS NOT FALSE|为真或者未知
      boolean_expression IS UNKNOWN|值为未知
      boolean_expression IS NOT UNKNOWN|为真或者为假
    * ### `GROUP BY` 和 `HAVING` 子句，用于分组统计场景
      > 在通过了WHERE过滤器之后，生成的输入表可以使用GROUPBY子句进行分组，然后用HAVING子句删除一些分组行。
      
      ```pgsql
      -- 按班级分组统计语文的总分和平均分
      select 
          "class" AS 班级, 
          sum("chinese") AS 语文总分, 
          avg("chinese") AS 语文平均分 
      FROM test_schema.student GROUP BY class;
      
      -- 按班级分组统计语文的总分和平均分，展示平均分在90分以下的班级
      select 
          "class" AS 班级, 
          sum("chinese") AS 语文总分, 
          avg("chinese") AS 语文平均分 
      FROM test_schema.student GROUP BY "class" HAVING avg("chinese") < 90;
      ```
    * ### `ORDER BY` 排序语句 
      ```pgsql
      -- 把所有学生按照语文成绩从低到高排序
      SELECT * FROM test_schema.student ORDER BY "chinese";
      SELECT * FROM test_schema.student ORDER BY "chinese" ASC;
      
      -- 把所有学生按照语文成绩从高到低排序
      SELECT * FROM test_schema.student ORDER BY "chinese" DESC;
      
      -- 把所有学生按照语文成绩从高到低排序，如语文成绩一样则按英语成绩从高到低排序
      SELECT * FROM test_schema.student ORDER BY "chinese"  DESC, "english" DESC;
      ```
    * ### `LIMIT` 和 `OFFSET` 用于翻页或仅展示指定的部分行语句
      ```pgsql
      -- 展示所有数据
      SELECT * FROM test_schema.student
      SELECT * FROM test_schema.student LIMIT ALL;
      -- 仅展示从头开始的5条记录
      SELECT * FROM test_schema.student LIMIT 5;
      -- 仅展示忽略头两条记录的后3条记录
      SELECT * FROM test_schema.student LIMIT 3 OFFSET 2;
      ```
    * ### `OVER` 窗口函数
      > 一个窗口函数在一系列与当前行有某种关联的表行上执行一种计算。
      
      ```pgsql
      SELECT 
          "class" AS 班级, 
          "name" AS 姓名, 
          "chinese" AS 语文, 
          avg("chinese") OVER (PARTITION BY "class") AS 班级平均分 
      FROM test_schema.student;
      ```
    * ### `WITH` 字句，一般用于递归查询
      > WITH提供了一种方式来书写在一个大型查询中使用的辅助语句。
      
      ```pgsql
      DROP TABLE IF EXISTS test_schema.elevel;
      CREATE TABLE test_schema.elevel(
         "id"        integer,
         "name"      CHARACTER VARYING (20),
         "parent_id" integer
      );
      INSERT INTO elevel ("id", "name", "parent_id") 
        VALUES (1, '英语', NULL), (2, '计算机', NULL),
              (3, '会计', NULL), (11, '英语专业四八级', 1),
              (111, '英语专业四级', 11), (112, '英语专业八级', 11),
              (121, '大学英语三级', 12), (122, '大学英语四级', 12),
              (12, '大学英语三、四、六级', 1), (123, '大学英语六级', 12),
              (21, 'NCR计算机等级', 2), (22, 'IT认证类考试', 2),
              (211, 'NCR计算机一级', 21), (212, 'NCR计算机二级', 21),
              (213, 'NCR计算机三级', 21), (214, 'NCR计算机四级', 21),
              (221, 'CISCO认证', 22), (222, 'ORACLE认证', 22),
              (31, '会计从业证', 3), (32, '会计职称', 3),
              (321, '初级职称(助理会计师)', 32), (322, '中级职称(会计师)', 32),
              (323, '高级职称(高级职称)', 32), (3231, '正高级会计师', 323),
              (3232, '副高级会计师', 323);
      -- 递归查询
      WITH RECURSIVE tree (id,name,parent_id,depath) as(
    	SELECT
    	       id,
    	       name,
    	       parent_id,
    	       1 as depath
    	FROM test_schema.elevel WHERE id IN (1,2,3)
    	UNION ALL
    	SELECT
    	       e2.id,
    	       e2.name AS name,
    	       e2.parent_id,
    	       e3.depath+1
    	FROM test_schema.elevel e2,tree e3 WHERE e3.id=e2.parent_id
      )SELECT * FROM tree ORDER BY rpad(id::VARCHAR,5,'0');
      ```
  * ## 在表之间进行连接查询
    ![](https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png)
    > 准备数据表与数据。
    ```pgsql
    DROP TABLE IF EXISTS test_schema.website;
    CREATE TABLE test_schema.website(
      "id" SERIAL,
      "name" VARCHAR(50),
      "url" VARCHAR(200),
      "alexa" INTEGER,
      "country" VARCHAR(4),
      PRIMARY KEY (id)
    );
    
    INSERT INTO test_schema.website(name, url, alexa, country) VALUES
    	('Google', 'https://www.google.cm/', 1, 'USA'),
    	('淘宝', 'https://www.taobao.com/', 13, 'CN'),
    	('菜鸟教程', 'http://www.runoob.com/', 4689, 'CN'),
    	('微博', 'http://weibo.com/', 20, 'CN'),
    	('Facebook', 'https://www.facebook.com/', 13, 'CN'),
    	('stackoverflow', 'http://stackoverflow.com/', 0, 'IND');
    
    DROP TABLE IF EXISTS test_schema.apps;
    CREATE TABLE test_schema.apps(
    	"id" SERIAL,
    	"app_name" VARCHAR(50),
    	"url" VARCHAR(200),
    	"country" VARCHAR(4),
    	PRIMARY KEY (id)
    );
    
    INSERT INTO test_schema.apps(app_name, url, country) VALUES
    ('QQ APP', 'http://im.qq.com/', 'CN'),
    ('微博 APP', 'http://weibo.com/', 'CN'),
    ('淘宝 APP', 'https://www.taobao.com/', 'CN');
    
    DROP TABLE IF EXISTS test_schema.access_log;
    CREATE TABLE test_schema.access_log(
      "aid" INTEGER,
      "site_id" INTEGER,
      "count" INTEGER,
      "date" DATE
    );
    
    INSERT INTO test_schema.access_log(aid, site_id, count, date) VALUES
    (1, 1, 45, '2016-05-10'),
    (2, 3, 100, '2016-05-13'),
    (3, 1, 230, '2016-05-14'),
    (4, 2, 10, '2016-05-14'),
    (5, 5, 205, '2016-05-14'),
    (6, 4, 13, '2016-05-15'),
    (7, 3, 220, '2016-05-15'),
    (8, 5, 545, '2016-05-16'),
    (9, 3, 201,'2016-05-17'),
    (1, 9, 45, '2016-05-10');
    ```
    * ### `SQL INNER JOIN` 内连接
      > INNER JOIN 关键字在表中存在至少一个匹配时返回行。如果 "Websites" 表中的行在 "access_log" 中没有匹配，则不会列出这些行。
      ![](https://www.runoob.com/wp-content/uploads/2013/09/img_innerjoin.gif)
      ```pgsql
      SELECT 
             w.name, 
             a.count, 
             a.date
      FROM test_schema.website w
      	     INNER JOIN test_schema.access_log a
      	                ON w.id=a.site_id
      ORDER BY a.count;
      ```
      
    * ### `SQL LEFT JOIN` 左连接
      > 即使右表中没有匹配，也从左表返回所有的行
      ![](https://www.runoob.com/wp-content/uploads/2013/09/img_leftjoin.gif)
      ```pgsql
      SELECT
             w.name,
             a.count,
             a.date
      FROM test_schema.website w
      	     LEFT JOIN test_schema.access_log a
      	               ON w.id=a.site_id
      ORDER BY a.count DESC;
      ```
      
    * ### `SQL RIGHT JOIN` 右连接
      > 即使左表中没有匹配，也从右表返回所有的行
      ![](https://www.runoob.com/wp-content/uploads/2013/09/img_rightjoin.gif)
      ```pgsql
      SELECT 
             w.name, 
             a.count, 
             a.date
      FROM test_schema.access_log a
      	     RIGHT JOIN test_schema.website w
      	                ON a.site_id=w.id
      ORDER BY a.count;
      ```
      
    * ### `SQL FULL OUTER JOIN` 全连接
      > 只要其中一个表中存在匹配，则返回行
      ![](https://www.runoob.com/wp-content/uploads/2013/09/img_fulljoin.gif)
      ```pgsql
      SELECT
             w.name,
             a.count,
             a.date
      FROM test_schema.website w
      	     FULL OUTER JOIN test_schema.access_log a
      	                     ON w.id=a.site_id
      ORDER BY a.count DESC;
      ```
      
    * ### `UNION` 合并
      > UNION 操作符用于合并两个或多个 SELECT 语句的结果集。
      ```pgsql
      SELECT * FROM test_schema.website
      UNION
      SELECT a.id, a.app_name AS name, a.url, 0 AS alexa, a.country  FROM test_schema.apps a
      ORDER BY country;
      
      -- 从 "Websites" 和 "apps" 表中选取所有不同的country（只有不同的值）
      SELECT country FROM test_schema.website
      UNION
      SELECT country  FROM test_schema.apps a
      ORDER BY country;
      
      -- 使用 UNION ALL 从 "Websites" 和 "apps" 表中选取所有的country（也有重复的值）
      SELECT country FROM test_schema.website
      UNION ALL
      SELECT country  FROM test_schema.apps a
      ORDER BY country;
      ```
* # 索引
  > 索引是对数据库表中一列或多列的值进行排序的一种结构，使用索引可快速访问数据库表中的特定信息。
  * ## 索引的类型
    * ### B-tree
      > 可排序数据上的处理等值和范围查询
      > 适用的字段类型：数字型、金额型、日期时间型
      > 适用的运算符：`<`、`<=`、`=`、`>=`、`>`、`BETWEEN ... AND ...`、`IN`
      > 支持多列索引，最多可以指定32个列
    * ### Hash
      > 只能处理简单等值比较
      > 适用的字段类型：数字型、金额型、日期时间型、字符型
      > 适用的运算符：`=`
    * ### GiST
      > 用于二维几何数据类型
      > 支持多列索引，最多可以指定32个列
    * ### SP-GiST
      > 利用四叉树、k-d树和radix树建立的二维点索引，用于二维几何数据类型
    * ### GIN 倒排索引
      > 适用于多个值组成的数组类型，为每一个组成值都包含一个单独的项，它可以高效地处理测试指定组成值是否存在的查询。
      > 适用的字段类型：数组类
      > 支持多列索引，最多可以指定32个列
    * ### BRIN 块范围索引
      > 适用于具有线性排序的数据类型
      > 支持多列索引，最多可以指定32个列
  * ## 多列索引
    * ### 只有 B-tree、GiST、GIN 和 BRIN 索引类型支持多列索引，最多可以指定32个列
    * ### 该限制可以在源代码文件pg_config_manual.h中修改，但是修改后需要重新编译PostgreSQL
    * ### 多列索引应该较少地使用。在绝大多数情况下，单列索引就足够了且能节约时间和空间。具有超过三个列的索引不太有用，反而效率更低
  * ## 唯一索引
    * PostgreSQL会自动为定义了一个唯一约束或主键的表创建一个唯一索引
    * 只有B-tree能够被声明为唯一
  * ## 注意事项（索引的使用原则）
    * ### 应该创建索引的列：
      * 在经常需要搜索的列上，可以加快搜索的速度；
      * 在作为主键和唯一值的列上，不需要额外创建索引，系统已经自动创建了索引；
      * 在外键上增加索引可以增加表与表之间的连接速度；
      * 在经常需要排序的列上创建索引，因为索引已经排序，这样查询可以利用索引的排序，加快排序查询时间；
      * 在经常使用在WHERE子句中的列上面创建索引，加快条件的判断速度。
    * ### 不应该创建索引的列：
      * 对于那些在查询中很少使用或者参考的列不应该创建索引
      * 对于那些只有很少数据值的列也不应该增加索引。（比如性别列）
      * 对于那些定义为`text`,`image`和`bit`数据类型的列不应该增加索引。
      * 当修改操作远远多于检索操作时，不应该创建索引。
        > 修改性能和检索性能是互相矛盾的。当增加索引时，会提高检索性能，但是会降低修改性能。当减少索引时，会提高修改性能，降低检索性能
  * ## PostgreSQL 中文分词及全文检索扩展
    > 全文索引的实现要靠 PgSQL 的 gin 索引。分词功能 PostgreSQL 内置了英文、西班牙文等拉丁语系的分词，中文分词需要借助开源插件 zhparser
    * ### SCWS中文分词
      * 项目主页：`http://www.xunsearch.com/scws/` 
      * Github 主页：`https://github.com/hightman/scws`
      * SCWS-1.2.3 安装说明
        1. 取得 scws-1.2.3 的代码
          ```shell
          wget http://www.xunsearch.com/scws/down/scws-1.2.3.tar.bz2
          ```
        2. 解开压缩包
          ```shell
          tar xvjf scws-1.2.3.tar.bz2
          ```
        3. 进入目录执行配置脚本和编译
          ```shell
          cd scws-1.2.3
          ./configure
          make
          make install
          ```
          > 注：这里和通用的 GNU 软件安装方式一样，具体选项参数执行 ./configure --help 查看。
          > 常用选项为：--prefix=<scws的安装目录>
        4. 顺利的话已经编译并安装成功到 /usr/local/scws 中了，执行下面命令看看文件是否存在
          ```shell
          ls -al /usr/local/lib/libscws.la
          ```
        5. 试试执行 scws-cli 文件
          ```shell
          /usr/local/scws/bin/scws -h
          scws (scws-cli/1.2.3)
          Simple Chinese Word Segmentation - Command line usage.
          Copyright (C)2007 by hightman.
          ```
        6. 用 wget 下载并解压词典，或从主页下载然后自行解压再将 *.xdb 放入 /usr/local/scws/etc 目录中
          ```shell
          cd /usr/local/etc
          wget http://www.xunsearch.com/scws/down/scws-dict-chs-gbk.tar.bz2
          wget http://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2
          tar xvjf scws-dict-chs-gbk.tar.bz2
          tar xvjf scws-dict-chs-utf8.tar.bz2
          ```
      * 安装完后，就可以在命令行中使用 scws 命令进行测试分词了， 其参数主要有：
        * -c utf8 指定字符集
        * -d dict 指定字典 可以是 xdb 或 txt 格式
        * -M 复合分词的级别， 1~15，按位异或的 1|2|4|8 依次表示 短词|二元|主要字|全部字，默认不复合分词，这个参数可以帮助调整到最想要的分词效果。
    * ### zhpaser全文检索
      * 项目主页：`http://blog.amutu.com/zhparser/` 
      * Github 主页：`https://github.com/amutu/zhparser`
      * 安装
        1. 下载zhparser源码
            ```shell
            git clone https://github.com/amutu/zhparser.git
            cd zhparser
            ```
        2. 编译和安装zhparser
            ```shell
            make && make install
            ```
        3. 创建extension
            ```shell
            psql dbname superuser -c 'CREATE EXTENSION zhparser'
            ```
        4. 配置`postgresql.conf`
            ```shell
            # 忽略所有的标点等特殊符号: 
            zhparser.punctuation_ignore = t
            
            # 闲散文字自动以二字分词法聚合: 
            zhparser.seg_with_duality = t
  
            # 将词典全部加载到内存里: 
            zhparser.dict_in_memory = t
  
            # 短词复合: 
            zhparser.multi_short = t
  
            # 散字二元复合: 
            zhparser.multi_duality = t
  
            # 重要单字复合: 
            zhparser.multi_zmain = t
  
            # 全部单字复合: 
            zhparser.multi_zall = t
            
            # 自定义词典，自定义词典的文件必须放在share/postgresql/tsearch_data目录中,zhparser根据文件扩展名确定词典的格式类型，.txt扩展名表示词典是文本格式，.xdb扩展名表示这个词典是xdb格式，多个文件使用逗号分隔
            #zhparser.extra_dicts = 'dict_extra.txt,mydict.xdb'
            ```
      * 使用
        ```pgsql
        -- create the extension
        CREATE EXTENSION zhparser;
        
        -- make test configuration using parser
        CREATE TEXT SEARCH CONFIGURATION testzhcfg (PARSER = zhparser);
        
        -- zhparser可以将中文切分成下面26种token
        SELECT ts_token_type('zhparser');
        
        -- add token mapping
        -- 下面的token映射只映射了名词(n)，动词(v)，形容词(a)，成语(i),叹词(e)和习用语(l)6种，这6种以外的全部被屏蔽。词典使用的是内置的simple词典
        ALTER TEXT SEARCH CONFIGURATION testzhcfg ADD MAPPING FOR n,v,a,i,e,l WITH simple;
        
        -- ts_parse
        SELECT * FROM ts_parse('zhparser', 'hello world! 2010年保障房建设在全国范围内获全面启动，从中央到地方纷纷加大 了保障房的建设和投入力度下。2011年，保障房进入了更大规模的建设阶段。住房城乡建设部党组书记、部长姜伟新去年底在全国住房城乡建设工作会议上表示，要继续推进保障性安居工程建设。');
        
        -- test to_tsvector
        SELECT to_tsvector('testzhcfg','“今年保障房新开工数量虽然有所下调，但实际的年度在建规模以及竣工规模会超以往年份，相对应的对资金的需求也会创历史纪录。”陈国强说。在他看来，与2011年相比，2012年的保障房建设在资金配套上的压力将更为严峻。');
        
        -- test to_tsquery（将句子解析成各个词的组合向量）
        SELECT to_tsquery('testzhcfg', '保障房资金压力');
        ```
      * 自定义词库
        ```pgsql
        test=# SELECT * FROM ts_parse('zhparser', '保障房资金压力');
         tokid | token
        -------+-------
           118 | 保障
           110 | 房
           110 | 资金
           110 | 压力
        
        test=# insert into zhparser.zhprs_custom_word values('资金压力');
        --删除词insert into zhprs_custom_word(word, attr) values('word', '!');
        --\d zhprs_custom_word 查看其表结构，支持TD, IDF
        test=# select sync_zhprs_custom_word();
         sync_zhprs_custom_word
        ------------------------
        
        (1 row)
        
        test=# \q --sync 后重新建立连接
        [lzzhang@lzzhang-pc bin]$ ./psql -U lzzhang -d test -p 1600
        test=# SELECT * FROM ts_parse('zhparser', '保障房资金压力');
         tokid |  token
        -------+----------
           118 | 保障
           110 | 房
           120 | 资金压力
        ```
      * 在数据表中存储分词结果
        ```pgsql
        CREATE SCHEMA test_schema;
        
        CREATE EXTENSION zhparser;

        CREATE TEXT SEARCH CONFIGURATION test_schema.zhparser_cfg (PARSER = zhparser);
        ALTER TEXT SEARCH CONFIGURATION test_schema.zhparser_cfg ADD MAPPING FOR c,g,h,o,k,p,r,u,v,b,d,f,n,v,a,i,j,m,q,s,t,w,y,z,e,l,x WITH simple;
        
        DROP TABLE IF EXISTS test_schema.film_parser;
        CREATE TABLE test_schema.film_parser (
        		"id" SERIAL,
        		"film_title" VARCHAR(200),
        		"release_time" VARCHAR(10),
        		"director" VARCHAR(200),
        		"scriptwriter" VARCHAR(200),
        		"protagonist" VARCHAR(200),
        		"type" VARCHAR(20),
        		"nation" VARCHAR(50),
        		"language" VARCHAR(50),
        		"grade" NUMERIC(4, 1),
        		"comments" INTEGER,
        		"plot" TEXT,
        		"tsv_plot" tsvector,
        		PRIMARY KEY ("id")
        );
        COMMENT ON TABLE test_schema.film_parser IS '豆瓣影评数据表';
        COMMENT ON COLUMN test_schema.film_parser."id" IS '自增主键';
        COMMENT ON COLUMN test_schema.film_parser."film_title" IS '片名';
        COMMENT ON COLUMN test_schema.film_parser."release_time" IS '上映时间';
        COMMENT ON COLUMN test_schema.film_parser."director" IS '导演';
        COMMENT ON COLUMN test_schema.film_parser."scriptwriter" IS '编剧';
        COMMENT ON COLUMN test_schema.film_parser."protagonist" IS '主演';
        COMMENT ON COLUMN test_schema.film_parser."type" IS '类型';
        COMMENT ON COLUMN test_schema.film_parser."nation" IS '国家/地区';
        COMMENT ON COLUMN test_schema.film_parser."language" IS '语言';
        COMMENT ON COLUMN test_schema.film_parser."grade" IS '评分';
        COMMENT ON COLUMN test_schema.film_parser."comments" IS '评论数';
        COMMENT ON COLUMN test_schema.film_parser."plot" IS '剧情简介';
        COMMENT ON COLUMN test_schema.film_parser."tsv_plot" IS '剧情分词';
        
        // 在新字段上创建索引
        DROP INDEX IF EXISTS idx_gin_plot;
        CREATE INDEX idx_gin_plot ON test_schema.film_parser USING GIN(tsv_plot);
        
        //创建一个更新分词触发器
        CREATE TRIGGER trigger_after_insert_film BEFORE INSERT ON test_schema.film_parser
        	FOR EACH ROW
        EXECUTE PROCEDURE tsvector_update_trigger(tsv_plot, 'test_schema.zhparser_cfg', plot);
        
        // 将字段的分词向量更新到新字段中
        UPDATE test_schema.film_parser SET tsv_plot = to_tsvector('test_schema.zhparser_cfg', coalesce(plot,''));
        
        -- 再进行查询时就可以直接使用 
        SELECT * FROM test_schema.film_parser WHERE tsv_plot @@ '中国';
        ```
    * ## ---- 附：词性标注 ----
      * Ag 
      形语素 
      形容词性语素。形容词代码为a，语素代码ｇ前面置以A。 
      * a 
      形容词 
      取英语形容词adjective的第1个字母。 
      * ad 
      副形词 
      直接作状语的形容词。形容词代码a和副词代码d并在一起。 
      * an 
      名形词 
      具有名词功能的形容词。形容词代码a和名词代码n并在一起。 
      * b 
      区别词 
      取汉字“别”的声母。 
      * c 
      连词 
      取英语连词conjunction的第1个字母。 
      * Dg 
      副语素 
      副词性语素。副词代码为d，语素代码ｇ前面置以D。 
      * d 
      副词 
      取adverb的第2个字母，因其第1个字母已用于形容词。 
      * e 
      叹词 
      取英语叹词exclamation的第1个字母。 
      * f 
      方位词 
      取汉字“方” 
      * g 
      语素 
      绝大多数语素都能作为合成词的“词根”，取汉字“根”的声母。 
      * h 
      前接成分 
      取英语head的第1个字母。 
      * i 
      成语 
      取英语成语idiom的第1个字母。 
      * j 
      简称略语 
      取汉字“简”的声母。 
      * k 
      后接成分 
      * l 
      习用语 
      习用语尚未成为成语，有点“临时性”，取“临”的声母。 
      * m 
      数词 
      取英语numeral的第3个字母，n，u已有他用。 
      * Ng 
      名语素 
      名词性语素。名词代码为n，语素代码ｇ前面置以N。 
      * n 
      名词 
      取英语名词noun的第1个字母。 
      * nr 
      人名 
      名词代码n和“人(ren)”的声母并在一起。 
      * ns 
      地名 
      名词代码n和处所词代码s并在一起。 
      * nt 
      机构团体 
      “团”的声母为t，名词代码n和t并在一起。 
      * nz 
      其他专名 
      “专”的声母的第1个字母为z，名词代码n和z并在一起。 
      * o 
      拟声词 
      取英语拟声词onomatopoeia的第1个字母。 
      * ba 介词 把、将 　 
      * bei 介词 被 　 
      * p 
      介词 
      取英语介词prepositional的第1个字母。 
      * q 
      量词 
      取英语quantity的第1个字母。 
      * r 
      代词 
      取英语代词pronoun的第2个字母,因p已用于介词。 
      * s 
      处所词 
      取英语space的第1个字母。 
      * Tg 
      时语素 
      时间词性语素。时间词代码为t,在语素的代码g前面置以T。 
      * t 
      时间词 
      取英语time的第1个字母。 
      * dec 助词 的、之 　 
      * deg 助词 得 　 
      * di 助词 地 　 
      * etc 助词 等、等等 　 
      * as 助词 了、着、过 　 
      * msp 助词 所 　 
      * u 
      其他助词 
      取英语助词auxiliary 
      * Vg 
      动语素 
      动词性语素。动词代码为v。在语素的代码g前面置以V。 
      * v 
      动词 
      取英语动词verb的第一个字母。 
      * vd 
      副动词 
      直接作状语的动词。动词和副词的代码并在一起。 
      * vn 
      名动词 
      指具有名词功能的动词。动词和名词的代码并在一起。 
      * w 
      其他标点符号 
      * x 
      非语素字 
      非语素字只是一个符号，字母x通常用于代表未知数、符号。 
      * y 
      语气词 
      取汉字“语”的声母。 
      * z 
      状态词 
      取汉字“状”的声母的前一个字母。
    
    * ## 使用`Jieba`分词和全文检索
      * 安装
        ```shell
        git clone https://github.com/jaiminpan/pg_jieba
        cd pg_jieba
        
        # initilized sub-project
        git submodule update --init --recursive
        
        mkdir build
        cd build
        yum install cmake
        cmake.. 
        ```
* # 数据库编程开发
  * ## 触发器
    > 可以用来在数据表发生写操作时（新增、更改或删除）时，去执行指定的存储过程
    ```pgsql
    create extension if not exists "uuid-ossp";
    create extension if not exists pgcrypto ;
    
    DROP TABLE IF EXISTS base_user;
    CREATE TABLE base_user (
    	id SERIAL8,
    	uuid uuid DEFAULT uuid_generate_v4(),
    	mobile VARCHAR(11) NOT NULL,
    	password VARCHAR(100) DEFAULT NULL,
    	clear_password VARCHAR(64) NOT NULL,
    	register_recip CIDR DEFAULT NULL,
    	login_recip CIDR DEFAULT NULL,
    	register_datetime TIMESTAMP(3) WITHOUT TIME ZONE DEFAULT now(),
    	login_datetime TIMESTAMP(3) WITHOUT TIME ZONE DEFAULT NULL,
    	logout_datetime TIMESTAMP(3) WITHOUT TIME ZONE DEFAULT NULL,
    	last_changes VARCHAR(64) DEFAULT 'Register',
    	last_recip CIDR NOT NULL,
    	PRIMARY KEY (id),
    	UNIQUE(mobile)
    );
    COMMENT ON TABLE base_user IS '用户数据表';
    COMMENT ON COLUMN base_user.id IS '自增主键';
    COMMENT ON COLUMN base_user.uuid IS 'uuid';
    COMMENT ON COLUMN base_user.mobile IS '手机号码';
    COMMENT ON COLUMN base_user.password IS '用户密码';
    COMMENT ON COLUMN base_user.clear_password IS '明文密码';
    COMMENT ON COLUMN base_user.register_recip IS '用户注册时IP';
    COMMENT ON COLUMN base_user.login_recip IS '用户最后登录时IP';
    COMMENT ON COLUMN base_user.register_datetime IS '注册时间';
    COMMENT ON COLUMN base_user.login_datetime IS '最后登录时间';
    COMMENT ON COLUMN base_user.logout_datetime IS '最后退出时间';
    COMMENT ON COLUMN base_user.last_changes IS '最后一次变更类型';
    COMMENT ON COLUMN base_user.last_recip IS '最后一次操作者ip';
    
    DROP TABLE IF EXISTS base_log_user;
    CREATE TABLE base_log_user (
    	id SERIAL8,
    	uid INTEGER DEFAULT 0,
    	changes VARCHAR(20) DEFAULT NULL,
    	info TEXT NOT NULL,
    	recip CIDR NOT NULL,
    	datetime TIMESTAMP(3) WITHOUT TIME ZONE NOT NULL DEFAULT now(),
    	PRIMARY KEY (id)
    );
    COMMENT ON TABLE base_log_user IS '用户系统日志记录表';
    COMMENT ON COLUMN base_log_user.id IS '自增主键';
    COMMENT ON COLUMN base_log_user.uid IS '用户uid';
    COMMENT ON COLUMN base_log_user.changes IS '变更类型';
    COMMENT ON COLUMN base_log_user.info IS '日志说明';
    COMMENT ON COLUMN base_log_user.recip IS '操作者ip';
    COMMENT ON COLUMN base_log_user.datetime IS '数据变动时间';
    
    CREATE OR REPLACE FUNCTION trigger_after_insert_base_user()
    	RETURNS TRIGGER
    AS $$
    DECLARE
    	infos TEXT;
    BEGIN
    	infos := '新用户【' || NEW.mobile || '】成功注册成为用户!';
    	UPDATE base_user SET "password" = crypt(NEW.clear_password, gen_salt('md5')),
	                     register_recip = NEW.last_recip
	    WHERE id = NEW.id;
    	INSERT INTO base_log_user (uid, changes, "info", recip)
    	VALUES (NEW.id, 'Register', infos, NEW.last_recip);
    	RETURN NEW;
    END;
    $$ LANGUAGE plpgsql;
    
    DROP TRIGGER IF EXISTS base_user_after_insert ON base_user;
    CREATE TRIGGER base_user_after_insert AFTER INSERT ON base_user
    	FOR EACH ROW
    EXECUTE PROCEDURE trigger_after_insert_base_user();
    
    CREATE OR REPLACE FUNCTION trigger_after_update_base_user()
    	RETURNS TRIGGER
    AS $$
    DECLARE
    	infos TEXT;
    BEGIN
    	CASE NEW.last_changes
    		WHEN 'Login' THEN
    			NEW.login_recip := NEW.last_recip;
    			NEW.login_datetime := now();
    			NEW.logout_datetime := NULL;
    			infos := 'APP 用户登录操作！';
    		WHEN 'Logout' THEN
    			NEW.login_datetime := NULL;
    			NEW.logout_datetime := now();
    			infos := 'APP 用户退出操作！';
    	ELSE
    		NEW.last_changes := 'manageSet';
    		infos := '管理后台对用户进行修改操作！';
    	END CASE ;
    	INSERT INTO base_log_user (uid, changes, "info", recip)
    	VALUES (NEW.id, NEW.last_changes, infos, NEW.last_recip);
    	RETURN NEW;
    END;
    $$ LANGUAGE plpgsql;
    
    DROP TRIGGER IF EXISTS base_user_after_update ON base_user;
    CREATE TRIGGER base_user_after_update BEFORE UPDATE OF last_changes ON base_user
    	FOR EACH ROW
    EXECUTE PROCEDURE trigger_after_update_base_user();
    ```
   * ## 存储过程 
    ```pgsql
    DROP FUNCTION IF EXISTS logic_user_reister(JSON);
    CREATE OR REPLACE FUNCTION logic_user_reister(
    	IN reister JSON
    ) RETURNS JSON
    AS $$
    DECLARE
    	user_mobile VARCHAR(11);
    	user_password VARCHAR(100);
    	user_recip VARCHAR(15);
    	user_num INTEGER;
    	user_id INTEGER;
    	user_record RECORD;
    BEGIN
    	IF (json_extract_path_text(reister, 'mobile') IS NULL) THEN
    		RETURN json_build_object('type', 'Error', 'message', '用户注册手机不能为空!', 'code', 230);
    	ELSE
    		user_mobile := json_extract_path_text(reister, 'mobile');
    	END IF;
    	IF (json_extract_path_text(reister, 'password') IS NULL) THEN
    		RETURN json_build_object('type', 'Error', 'message', '用户注册密码不能为空!', 'code', 230);
    	ELSE
    		user_password := json_extract_path_text(reister, 'password');
    	END IF;
    	IF (json_extract_path_text(reister, 'recip') IS NULL) THEN
    		RETURN json_build_object('type', 'Error', 'message', '用户注册操作IP不能为空!', 'code', 230);
    	ELSE
    		user_recip := json_extract_path_text(reister, 'recip');
    	END IF;
    	SELECT count(*) INTO user_num FROM base_user WHERE mobile = user_mobile;
    	IF(user_num = 0) THEN
    		INSERT INTO base_user (mobile, clear_password, last_recip) VALUES (user_mobile, user_password, user_recip::CIDR) RETURNING id INTO user_id;
    		UPDATE base_user SET last_changes = 'Login', last_recip = user_recip::CIDR WHERE id = user_id;
    		SELECT uuid, mobile INTO user_record FROM base_user WHERE id = user_id;
    		RETURN json_build_object(
    				'code', 201,
    				'type', 'Success',
    				'message', '用户成功注册并登录!',
    				'user', json_build_object(
    						'uid', user_id,
    						'uuid', user_record.uuid,
    						'mobile', user_record.mobile
    				)
    		);
    	ELSE
    		RETURN json_build_object('type', 'Error', 'message', '用户手机号码已经被注册!', 'code', 220);
    	END IF;
    	EXCEPTION WHEN OTHERS THEN
    	RETURN json_build_object('type', 'Error', 'message', '用户注册失败!', 'error', replace(SQLERRM, '"', '`'), 'sqlstate', SQLSTATE);
    END;
    $$ LANGUAGE plpgsql;
    COMMENT ON FUNCTION logic_user_reister(JSON) IS '用户注册的操作';
    
    DROP FUNCTION IF EXISTS logic_user_logout(INTEGER);
    CREATE OR REPLACE FUNCTION logic_user_logout(
    	IN user_id INTEGER
    ) RETURNS JSON
    AS $$
    DECLARE
    	user_num INTEGER;
    BEGIN
    	SELECT COUNT(*) INTO user_num FROM base_user WHERE id = user_id;
    	IF(user_num > 0) THEN
    		UPDATE base_user SET last_changes = 'Logout' WHERE id = user_id;
    		RETURN json_build_object(
    				'code', 201,
    				'type', 'Success',
    				'message', '用户成功退出系统!'
    		);
    	ELSE
    		RETURN json_build_object('type', 'Error', 'message', '指定用户UID不存在!', 'code', 230);
    	END IF;
    	EXCEPTION WHEN OTHERS THEN
    	RETURN json_build_object('type', 'Error', 'message', '用户退出系统失败!', 'error', replace(SQLERRM, '"', '`'), 'sqlstate', SQLSTATE);
    END;
    $$ LANGUAGE plpgsql;
    COMMENT ON FUNCTION logic_user_logout(INTEGER) IS '用户退出系统的操作';
    
    DROP FUNCTION IF EXISTS logic_user_login(JSON);
    CREATE OR REPLACE FUNCTION logic_user_login(
    	IN logins JSON
    ) RETURNS JSON
    AS $$
    DECLARE
    	user_mobile VARCHAR(11);
      	user_password VARCHAR(50);
    	recip VARCHAR(15);
  	user_num INTEGER;
    	user_record RECORD;
    BEGIN
    	IF (json_extract_path_text(logins, 'mobile') IS NULL) THEN
      		RETURN json_build_object('type', 'Error', 'message', '用户注册手机不能为空!', 'code', 230);
      	ELSE
      		user_mobile := json_extract_path_text(logins, 'mobile');
    	END IF;
    	IF (json_extract_path_text(logins, 'password') IS NULL) THEN
    		RETURN json_build_object('type', 'Error', 'message', '用户注册密码不能为空!', 'code', 230);
    	ELSE
    		user_password := json_extract_path_text(logins, 'password');
    	END IF;
    	IF (json_extract_path_text(logins, 'recip') IS NULL) THEN
    		RETURN json_build_object('type', 'Error', 'message', '用户注册操作IP不能为空!', 'code', 230);
    	ELSE
    	  recip := json_extract_path_text(logins, 'recip');
    	END IF;
    	SELECT count(*) INTO user_num FROM base_user WHERE mobile = user_mobile AND "password" = crypt(user_password, "password");
    	IF(user_num > 0) THEN
    		UPDATE base_user SET last_changes = 'Login', last_recip = recip::CIDR WHERE mobile = user_mobile AND "password" = crypt(user_password, "password");
    		SELECT id, uuid INTO user_record FROM base_user WHERE mobile = user_mobile AND "password" = crypt(user_password, "password");
    		RETURN json_build_object(
    				'code', 201,
    				'type', 'Success',
    				'message', '用户成功登录!',
    				'user', json_build_object(
      						'uid', user_record.id,
      						'uuid', user_record.uuid,
    						  'mobile', user_mobile
    				)
    		);
    	ELSE
    		RETURN json_build_object('type', 'Error', 'message', '用户手机号码或密码错误!', 'code', 220);
    	END IF;
    	EXCEPTION WHEN OTHERS THEN
    	RETURN json_build_object('type', 'Error', 'message', '用户登录失败!', 'error', replace(SQLERRM, '"', '`'), 'sqlstate', SQLSTATE);
    END;
    $$ LANGUAGE plpgsql;
    COMMENT ON FUNCTION logic_user_login(JSON) IS '用户登录的操作';
    ```
  * ## 应用触发器和存储过程实现的用户注册登录和退出的逻辑
    * 用户注册
      * `mobile`：注册用手机号码
      * `password`：用户注册密码
      * `recip`：用户IP地址
      * `request_id`：手机验证码发送请求（在配置中设置了用户注册时通过短信验证，必填）
      * `code`：手机验证码（在配置中设置了用户注册时通过短信验证，必填）
      ```sql
      SELECT logic_user_reister(
        json_build_object(
          'mobile', '13354631000',
          'password', 'xx123456',
          'recip', '127.0.0.1'
        )
      );
      ```
    * 用户退出系统
        * `uid`：用户uid
        ```sql
        SELECT logic_user_logout(2);
        ```
    * 用户登录系统
        * `mobile`：手机号码
        * `password`：用户密码
        * `recip`：用户IP地址
        ```sql
        SELECT logic_user_login(
          json_build_object(
              'mobile', '13354631000',
              'password', 'xx123456',
              'recip', '127.0.0.1'
            )
        );
        ```
  * ## 视图
  * ## 定时任务