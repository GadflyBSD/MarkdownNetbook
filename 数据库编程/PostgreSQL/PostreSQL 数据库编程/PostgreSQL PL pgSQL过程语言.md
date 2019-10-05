## 一、概述
  > * PL/pgSQL函数在第一次被调用时，其函数内的源代码(文本)将被解析为二进制指令树，但是函数内的表达式和SQL命令只有在首次用到它们的时候，PL/pgSQL解释器才会为其创建一个准备好的执行规划，随后对该表达式或SQL命令的访问都将使用该规划。如果在一个条件语句中，有部分SQL命令或表达式没有被用到，那么PL/pgSQL解释器在本次调用中将不会为其准备执行规划，这样的好处是可以有效地减少为PL/pgSQL函数里的语句生成分析和执行规划的总时间，然而缺点是某些表达式或SQL命令中的错误只有在其被执行到的时候才能发现。
  >* 由于PL/pgSQL在函数里为一个命令制定了执行计划，那么在本次会话中该计划将会被反复使用，这样做往往可以得到更好的性能，但是如果你动态修改了相关的数据库对象，那么就有可能产生问题，如：

```pgsql
CREATE FUNCTION populate() RETURNS integer AS $$
DECLARE
  -- 声明段
BEGIN
  PERFORM my_function();
END;
$$ LANGUAGE plpgsql;
```

## 二、PL/pgSQL的结构
  > * PL/pgSQL是一种块结构语言，函数定义的所有文本都必须在一个块内，其中块中的每个声明和每条语句都是以分号结束，如果某一子块在另外一个块内，那么该子块的END关键字后面必须以分号结束，不过对于函数体的最后一个END关键字，分号可以省略
  > * 在PL/pgSQL中有两种注释类型，双破折号(--)表示单行注释。/* */表示多行注释，该注释类型的规则等同于C语言中的多行注释。
  > * 在语句块前面的声明段中定义的变量在每次进入语句块(BEGIN)时都会将声明的变量初始化为它们的缺省值，而不是每次函数调用时初始化一次。

  ```pgsql
  CREATE FUNCTION somefunc() RETURNS integer AS $$
    DECLARE
       quantity integer := 30;
    BEGIN
       RAISE NOTICE 'Quantity here is %', quantity;      --在这里的数量是30
       quantity := 50;
       --
       -- 创建一个子块
       --
       DECLARE
          quantity integer := 80;
       BEGIN
          RAISE NOTICE 'Quantity here is %', quantity;   --在这里的数量是80
       END;
       RAISE NOTICE 'Quantity here is %', quantity;      --在这里的数量是50    
       RETURN quantity;
    END;
    $$ LANGUAGE plpgsql;
    
    #执行该函数以进一步观察其执行的结果。
    postgres=# select somefunc();
    NOTICE:  Quantity here is 30
    NOTICE:  Quantity here is 80
    NOTICE:  Quantity here is 50
     somefunc
    ----------
           50
    (1 row)
  ```
  
##　三、声明
> 所有在块里使用的变量都必须在块的声明段里先进行声明，唯一的例外是FOR循环里的循环计数变量，该变量被自动声明为整型。变量声明的语法如下：

```pgsql
variable_name [ CONSTANT ] variable_type [ NOT NULL ] [ { DEFAULT | := } expression ];
```
> 1. SQL中的数据类型均可作为PL/pgSQL变量的数据类型，如integer、varchar和char等。
> 2. 如果给出了DEFAULT子句，该变量在进入BEGIN块时将被初始化为该缺省值，否则被初始化为SQL空值。缺省值是在每次进入该块时进行计算的。因此，如果把now()赋予一个类型为timestamp的变量，那么该变量的缺省值将为函数实际调用时的时间，而不是函数预编译时的时间。
> 3. CONSTANT选项是为了避免该变量在进入BEGIN块后被重新赋值，以保证该变量为常量。
> 4. 如果声明了NOT NULL，那么赋予NULL数值给该变量将导致一个运行时错误。因此所有声明为NOT NULL的变量也必须在声明时定义一个非空的缺省值。

1. 函数参数的别名
传递给函数的参数都是用$1、$2这样的标识符来表示的。为了增加可读性，我们可以为其声明别名。之后别名和数字标识符均可指向该参数值
    * 在函数声明的同时给出参数变量名。
      ```pgsql
      CREATE FUNCTION sales_tax(subtotal real) RETURNS real AS $$ 
      BEGIN
          RETURN subtotal * 0.06;
      END;
      $$ LANGUAGE plpgsql;
      ```
    * 在声明段中为参数变量定义别名。
      ```pgsql
      CREATE FUNCTION sales_tax(REAL) RETURNS real AS $$
      DECLARE
          subtotal ALIAS FOR $1;
      BEGIN
          RETURN subtotal * 0.06;
      END;
      $$ LANGUAGE plpgsql;
      ```
    * 对于输出参数而言，我们仍然可以遵守前两条的规则。
      ```pgsql
      CREATE FUNCTION sales_tax(subtotal real, OUT tax real) AS $$
      BEGIN
          tax := subtotal * 0.06;
      END;
      $$ LANGUAGE plpgsql; 
      ```
    * 如果PL/pgSQL函数的返回类型为多态类型(anyelement或anyarray)，那么函数就会创建一个特殊的参数：$0。我们仍然可以为该变量设置别名。
      ```pgsql
      CREATE FUNCTION add_three_values(v1 anyelement, v2 anyelement, v3 anyelement)
      RETURNS anyelement AS $$
      DECLARE
          result ALIAS FOR $0;
      BEGIN
          result := v1 + v2 + v3;
          RETURN result;
      END;
      $$ LANGUAGE plpgsql;
      ```
      
2. 拷贝类型
    * 见如下形式的变量声明`variable %TYPE` `%TYPE`表示一个变量或表字段的数据类型，`PL/pgSQL`允许通过该方式声明一个变量，其类型等同于`variable`或表字段的数据类型，见如下示例：`user_id users.user_id %TYPE;`
    * 在上面的例子中，变量`user_id` 的数据类型等同于`users`表中`user_id`字段的类型。
    * 通过使用`%TYPE`，一旦引用的变量类型今后发生改变，我们也无需修改该变量的类型声明。最后需要说明的是，我们可以在函数的参数和返回值中使用该方式的类型声明。
3. 行类型
    * 见如下形式的变量声明
      ```pgsql
      name table_name%ROWTYPE;
      name composite_type_name;
      ```
    * table_name%ROWTYPE表示指定表的行类型，我们在创建一个表的时候，PostgreSQL也会随之创建出一个与之相应的复合类型，该类型名等同于表名，因此，我们可以通过以上两种方式来声明行类型的变量。由此方式声明的变量，可以保存SELECT返回结果中的一行。如果要访问变量中的某个域字段，可以使用点表示法，如rowvar.field，但是行类型的变量只能访问自定义字段，无法访问系统提供的隐含字段，如OID等。对于函数的参数，我们只能使用复合类型标识变量的数据类型。最后需要说明的是，推荐使用%ROWTYPE的声明方式，这样可以具有更好的可移植性，因为在Oracle的PL/SQL中也存在相同的概念，其声明方式也为%ROWTYPE。见如下示例：
      ```pgsql
      CREATE FUNCTION merge_fields(t_row table1) RETURNS text AS $$
      DECLARE
          t2_row table2%ROWTYPE;
      BEGIN
          SELECT * INTO t2_row FROM table2 WHERE id = 1 limit 1;
          RETURN t_row.f1 || t2_row.f3 || t_row.f5 || t2_row.f7;
      END;
      $$ LANGUAGE plpgsql;
      ```
4. 记录类型
    * 见如下形式的变量声明：
      ```pgsql
      name RECORD;
      ```
    * 记录变量类似于行类型变量，但是它们没有预定义的结构，只能通过SELECT或FOR命令来获取实际的行结构，因此记录变量在被初始化之前无法访问，否则将引发运行时错误。
    * RECORD不是真正的数据类型，只是一个占位符。
    
      
## 四、基本语句
1. 赋值语句
    > PL/pgSQL中赋值语句的形式为：`identIFier := expression`，等号两端的变量和表达式的类型或者一致，或者可以通过PostgreSQL的转换规则进行转换，否则将会导致运行时错误
2. SELECT INTO语句
    * 通过该语句可以为记录变量或行类型变量进行赋值，其表现形式为：SELECT INTO target `select_expressions FROM ...`，该赋值方式一次只能赋值一个变量。表达式中的target可以表示为是一个记录变量、行变量，或者是一组用逗号分隔的简单变量和记录/行字段的列表。select_expressions以及剩余部分和普通SQL一样。
    * 如果将一行或者一个变量列表用做目标，那么选出的数值必需精确匹配目标的结构，否则就会产生运行时错误。如果目标是一个记录变量，那么它自动将自己构造成命令结果列的行类型。如果命令返回零行，目标被赋予空值。如果命令返回多行，那么将只有第一行被赋予目标，其它行将被忽略。在执行`SELECT INTO`语句之后，可以通过检查内置变量FOUND来判断本次赋值是否成功
      ```pgsql
      SELECT INTO myrec * FROM emp WHERE empname = myname;
      IF NOT FOUND THEN
          RAISE EXCEPTION 'employee % not found', myname;
      END IF;
      ```
    * 要测试一个记录/行结果是否为空，可以使用`IS NULL`条件进行判断，但是对于返回多条记录的情况则无法判断
      ```pgsql
      DECLARE
        users_rec RECORD;
      BEGIN
        SELECT INTO users_rec * FROM users WHERE user_id = 3;
        IF users_rec.homepage IS NULL THEN
            RETURN 'http://';
        END IF;
      END;
      ```
3. 执行一个没有结果的表达式或者命令
    > 在调用一个表达式或执行一个命令时，如果对其返回的结果不感兴趣，可以考虑使用PERFORM语句：PERFORM query，该语句将执行PERFORM之后的命令并忽略其返回的结果。其中query的写法和普通的SQL SELECT命令是一样的，只是把开头的关键字SELECT替换成PERFORM
    ```pgsql
    PERFORM create_mv('cs_session_page_requests_mv', my_query);
    ```
4. 执行动态命令
    * 如果在PL/pgSQL函数中操作的表或数据类型在每次调用该函数时都可能会发生变化，在这样的情况下，可以考虑使用PL/pgSQL提供的EXECUTE语句：EXECUTE command-string [ INTO target ]，其中command-string是用一段文本表示的表达式，它包含要执行的命令。而target是一个记录变量、行变量或者一组用逗号分隔的简单变量和记录/行域的列表。这里需要特别注意的是，该命令字符串将不会发生任何PL/pgSQL变量代换，变量的数值必需在构造命令字符串时插入到该字符串中。
    * 和所有其它PL/pgSQL命令不同的是，一个由EXECUTE语句运行的命令在服务器内并不会只prepare和保存一次。相反，该语句在每次运行的时候，命令都会prepare一次。因此命令字符串可以在函数里动态的生成以便于对各种不同的表和字段进行操作，从而提高函数的灵活性。然而由此换来的却是性能上的折损。
      ```pgsql
      EXECUTE 'UPDATE tbl SET ' || quote_ident(columnname) || ' = ' || quote_literal(newvalue); 
      ```