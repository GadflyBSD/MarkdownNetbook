>         mysql 操作同样有循环语句操作，网上说有3中标准的循环方式： while 循环 、 loop 循环和repeat循环。还有一种非标准的循环： goto。 鉴于goto语句的跳跃性会造成使用的的思维混乱，所以不建议使用。这几个循环语句的格式如下：
- **条件判断语句**
  - [x] *`IF ... else`*
- **分支语句**
  - [x] *`case`*
- **循环语句**
  - [x] *`WHILE……DO……END WHILE`*
  - [x] *`REPEAT……UNTIL END REPEAT`*
  - [x] *`LOOP……END LOOP`*
  - ~~*`GOTO`*~~

## 一、条件判断语句
```mysql
drop procedure if exists test_if;  
delimiter $$ 
create procedure test_if(in x int)  
  begin  
    if x=1 then  
      select 'OK';  
    elseif x=0 then  
      select 'No';  
    else   
      select 'good';  
    end if;  
  end  
delimiter $$  
call test_if(0); 
```
---
## 二、分支语句
```mysql
drop procedure if exists test_case;  
delimiter $$  
create procedure test_case(in x int)  
  begin  
    case x  
      when 1 then 
        select 'OK';  
      when 0 then 
        select 'No';  
      else 
        select 'good';  
    end case;  
  end  
delimiter $$  
call test_case(9);  
```
---
## 三、循环语句
* ### while语句
  ```mysql
  delimiter $$　　　　// 定义结束符为 $$
  drop procedure if exists wk;  //  删除 已有的 存储过程
  create procedure wk()　　　　　　//　 创建新的存储过程
    begin 
      declare i int;　　　　　　　　　　// 变量声明
      set i = 1;　　　　　
      while i < 11 do 　　　　　　　　　　//   循环体
        insert into user_profile (uid) values (i);
        set i = i +1;
      end while;
    end $$
  ```
---
* ### REPEAT 语句
  ```mysql
  delimiter $$
  drop procedure if exists looppc;
  create procedure looppc()
    begin 
      declare i int;
      set i = 1;
      repeat 
          insert into user_profile_company (uid) values (i+1);
          set i = i + 1;
      until i >= 20
      end repeat;
    end $$
  ```
---
* ### LOOP 语句
  ```mysql
  delimiter $$
  drop procedure if exists lopp;
  create procedure lopp()
    begin 
      declare i int ;
      set i = 1;
      lp1 : LOOP　　　//  lp1 为循环体名称   LOOP 为关键字
          insert into user_profile (uid) values (i);
          set i = i+1;
          if i > 30 then
      leave lp1;　　　　　　　　　　　　　　//  离开循环体
          end if;
      end LOOP;　　　　　　　　　　　　　　//  结束循环
    end $$
  ```