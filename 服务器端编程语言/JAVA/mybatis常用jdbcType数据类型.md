# Mybatis中javaType和jdbcType对应关系

JDBC Type|Java Type
-|-
CHAR|String
VARCHAR|String
LONGVARCHAR|String
NUMERIC|java.math.BigDecimal
DECIMAL|java.math.BigDecimal
BIT|boolean
BOOLEAN|boolean
TINYINT|byte
SMALLINT|short
INTEGER|int
BIGINT|long
REAL|float
FLOAT|double
DOUBLE|double
BINARY|byte[]
VARBINARY|byte[]
LONGVARBINARY|byte[]
DATE|java.sql.Date
TIME|java.sql.Time
TIMESTAMP|java.sql.Timestamp
CLOB|Clob
BLOB|Blob
ARRAY|Array
DISTINCT|mapping of underlying type
STRUCT|Struct
REF|Ref
DATALINK|java.net.URL

# SQL数据类型和Java数据类型的对应关系 
SQL Type|Java Type
-|-
integer、int|int 
tinyint、smallint|short 
bigint|long 
decimal、numeric|java.math.BigDecimal 
float|float 
double|double 
char、varchar|String 
boolean、bit|boolean 
date|java.sql.Date 
time|java.sql.Time 
timestamp|java.sql.Timestamp 
blob|java.sql.Blob 
clob|java.sql.Clob 
array|java.sql.Array