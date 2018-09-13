---
title: mysql-8-window-function
date: 2018.09.12 14:36:45
reward: false
tags:
    - mysql
    - window-function
---

## mysql window-function 介绍
    
&nbsp;&nbsp;&nbsp;&nbsp;窗口函数早在SQL2003规范中就成为了标准SQL的一部分。而在mysql8.0带来了标准SQL的窗口函数功能，窗口函数与分组聚合函数相似的都提供了
对一组行数据的统计计算。但与分组聚合函数将多行合并成一行不同是窗口函数会在结果结果集中展现每一行的聚合。
窗口函数有两种使用方式，首先是常规的SQL聚合功能函数和特殊的窗口函数。

&nbsp;&nbsp;&nbsp;&nbsp;常规的聚合功能函数如：COUNT,SUM等函数。而窗口函数专有的则是RANK, DENSE_RANK, PERCENT_RANK, CUME_DIST, NTILE, ROW_NUMBER, 
FIRST_VALUE, LAST_VALUE, NTH_VALUE, LEADand LAG等函数。

&nbsp;&nbsp;&nbsp;&nbsp;窗口函数的主要优点是不会导致单个输出记录而进行分组。相反地，记录保留其单独的身份，并将聚合值添加到每条记录（行）

### films structure

```
CREATE TABLE films (
  id int(11) primary key not null,
  release_year int(11),
  category_id int(11),
  rating decimal(3,2)
);

insert into films (id,release_year,category_id,rating ) values
('1', '2015', '1', '8.00'),
('2', '2015', '2', '8.50'),
('3', '2015', '3', '9.00'),
('4', '2016', '2', '8.20'),
('5', '2016', '1', '8.40'),
('6', '2017', '2', '7.00'),
('7', '2015', '1', '7.20'),
('8', '2015', '3', '5.80'),
('9', '2016', '2', '7.00'),
('10', '2017', '4', '8.50'),
('11', '2017', '1', '9.20'),
('12', '2015', '2', '8.50');

```
### select

```
select * from films;
```
id|release_year|category_id|rating
---|---|---|---
1|2015| 1| 8.00
2| 2015| 2| 8.50
3| 2015| 3| 9.00
4| 2016| 2| 8.20
5| 2016| 1| 8.40
6| 2017| 2| 7.00
7| 2015| 1| 7.20
8| 2015| 3| 5.80
9| 2016| 2| 7.00
10| 2017| 4| 8.50
11| 2017| 1| 9.20
12| 2015| 2| 8.50

### cume_dist

>计算结果为相对位置/总行数,返回值为(0,1]<br/>
注意：对于重复值，计算的时候，取重复值的最后一行的位置

### percent_rank

>计算方法为（相对位置-1）/（总行数-1）<br/>
注意：对于重复值，计算的时候，取重复值的第一行的位<br/>
没有重复值的排序[记录相等也是不重复的]可以进行分页使用
    
### row_number

>排序返回值

```
--  cume_dist() / row_number() / 
select f.id,f.release_year,f.category_id,f.rating,
-- cume_dist() OVER (order by f.rating) as sum,
cume_dist() OVER w as total,
row_number() over w as `row`,
percent_rank()  over w as dfs
from films f
WINDOW w as (order by f.rating)
;
```


id | release_year | rating  | total  |  row  |  dfs
----|----------|----|----|----|----
8|  2015| 3| 5.80| 0.08333333333333333| 1|  0
6|  2017| 2| 7.00| 0.25|                2|  0.09090909090909091
9|  2016| 2| 7.00| 0.25|                3|  0.09090909090909091
7|  2015| 1| 7.20| 0.3333333333333333|  4|  0.2727272727272727
1|  2015| 1| 8.00| 0.4166666666666667|  5|  0.36363636363636365
4|  2016| 2| 8.20| 0.5|                 6|  0.45454545454545453
5|  2016| 1| 8.40| 0.5833333333333334|  7|  0.5454545454545454
2|  2015| 2| 8.50| 0.8333333333333334|  8|  0.6363636363636364
10| 2017| 4| 8.50| 0.8333333333333334|  9|  0.6363636363636364
12| 2015| 2| 8.50| 0.8333333333333334|  10| 0.6363636363636364
3|  2015| 3| 9.00| 0.9166666666666666|  11| 0.9090909090909091
11| 2017| 1| 9.20| 1|                   12| 1

### first_value

>FIRST_VALUE 返回组中数据窗口的分组中的第一个值<br/> 
FIRST_VALUE ( [scalar_expression )OVER ( [ partition_by_clause ] order_by_clause ) 

### last_value

>LAST_VALUE返回组中数据窗口的分组中的最后一个值<br/>
LAST_VALUE ( [scalar_expression )OVER ( [ partition_by_clause ] order_by_clause ) 


### NTH_VALUE

> NTH_VALUE（rating, x）返回组中数据窗口的分组第x+1的rating值，没有在计算到中的为null
``` 
select *
,first_value(rating) over w as `first` -- based on partition by release_year
,last_value(rating) over w as `last` -- based on partition by release_year
,NTH_VALUE(rating, 2) OVER w AS 'second'  
from films
window w as (PARTITION BY release_year
-- order by id 
ROWS  --?? 
UNBOUNDED
PRECEDING
)
;      
```

id | release_year|category_id | rating|first  | last  | second
----|---|---|---|---|---|---
1| 2015| 1| 8.00| 8.00| 8.00| NULL
2| 2015| 2| 8.50| 8.00| 8.50| NULL
3| 2015| 3| 9.00| 8.00| 9.00| NULL
7| 2015| 1| 7.20| 8.00| 7.20| 7.20
8| 2015| 3| 5.80| 8.00| 5.80| 7.20
12| 2015| 2| 8.50| 8.00| 8.50| 7.20
4| 2016| 2| 8.20| 8.20| 8.20| NULL
5| 2016| 1| 8.40| 8.20| 8.40| NULL
9| 2016| 2| 7.00| 8.20| 7.00| NULL
6| 2017| 2| 7.00| 7.00| 7.00| NULL
10| 2017| 4| 8.50| 7.00| 8.50| NULL
11| 2017| 1| 9.20| 7.00| 9.20| NULL


### lag

### lead

```
select *
,lag(rating) over w as 	`lag`  -- last row rating to show lag, lag row is null, so lag is null
,lead(rating) over w as `lead` -- next row rating to show lead , next row is null, so lead is null 
from reptile_dobao.films
window w as (partition by release_year)  -- partition by 分区 ,lag / lead based on partition of release_year
;

```

id | release_year|category_id | rating|lag  | lead
----|---|---|---|---|---
1| 2015| 1| 8.00| NULL| 8.50
2| 2015| 2| 8.50| 8.00| 9.00
3| 2015| 3| 9.00| 8.50| 7.20
7| 2015| 1| 7.20| 9.00| 5.80
8| 2015| 3| 5.80| 7.20| 8.50
12| 2015| 2| 8.50| 5.80| NULL
4| 2016| 2| 8.20| NULL| 8.40
5| 2016| 1| 8.40| 8.20| 7.00
9| 2016| 2| 7.00| 8.40| NULL
6| 2017| 2| 7.00| NULL| 8.50
10| 2017| 4| 8.50| 7.00| 9.20
11| 2017| 1| 9.20| 8.50| NULL


lag(v,n,x) ,lead(v,n,x)带参数时，v 是字段，n指分组中区间数据的行数，n+1开始给值，当计算值没有值时用x值表式
```
select *
,lag(rating,2,0) over w as 	`lag`  -- last row rating to show lag, lag row is 0, so lag is 0
,lead(rating,2,0) over w as `lead` -- next row rating to show lead , next row is null, so lead is null 
from reptile_dobao.films
window w as (partition by release_year)  -- partition by 分区 ,lag / lead based on partition of release_year
;
```

id | release_year|category_id | rating|lag  | lead
----|---|---|---|---|---
1| 2015| 1| 8.00| 0.00| 9.00
2| 2015| 2| 8.50| 0.00| 7.20
3| 2015| 3| 9.00| 8.00| 5.80
7| 2015| 1| 7.20| 8.50| 8.50
8| 2015| 3| 5.80| 9.00| 0.00
12| 2015| 2| 8.50| 7.20| 0.00
4| 2016| 2| 8.20| 0.00| 7.00
5| 2016| 1| 8.40| 0.00| 0.00
9| 2016| 2| 7.00| 8.20| 0.00
6| 2017| 2| 7.00| 0.00| 9.20
10| 2017| 4| 8.50| 0.00| 0.00
11| 2017| 1| 9.20| 7.00| 0.00

### RANK()

> 跳跃排序

### NTILE()

> 连续排序  