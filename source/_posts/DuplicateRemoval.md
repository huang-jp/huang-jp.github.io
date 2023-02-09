---
title: mysql 查询两个表相同数据 全部数据 查询一个表中另外一个表不存在的数据
---
查询相同数据：	
select * from table1 inner join table2 on table1.codeid=table2.codeid 

两张表数据数据（去掉重复数据）：		
select t1.codeid ,t1.cedename from  table1  union  select t2.codeid ,t2.cedename from  table1

全部数据（包括重复数据）：       	
select codeid,cedename from table1 union all select codeid,cedename from table2  

SQL查询一个表中另外一个表不存在的数据：

#方法一：使用 not in ,容易理解,效率低  ~执行时间为：1.395秒~
SELECT table1.name FROM table1 WHERE table1.name NOT IN (SELECT table2.name FROM table2);
#方法二：使用 left join...on... , "B.ID isnull" 表示左连接之后在B.ID 字段为 null的记录  ~执行时间：0.739秒~
SELECT COUNT(1) FROM ecs_goods LEFT JOIN ecs_member_price ON ecs_goods.goods_id=ecs_member_price.goods_id WHERE ecs_member_price.goods_id IS NULL;
#方法三：逻辑相对复杂,但是速度最快  ~执行时间: 0.570秒~
SELECT COUNT(1) FROM  ecs_goods c WHERE (SELECT COUNT(1) AS num FROM ecs_member_price WHERE ecs_member_price.goods_id=ecs_goods.goods_id) = 0;
