---
title: mysql partition
date: 2019/10/25 14:40:45
reward: false
tags: 
    - mysql
    - partition
categories: mysql
---

```sql
-- create table
CREATE TABLE `test_partition` (
  `name` varchar(20) DEFAULT NULL,
  `time_test` datetime NOT NULL
);

-- to_days()
select to_days("2019-10-10"); -- 737707： from 1970-01-01 to 2019-10-10 have 737707 day 
select month("2019-10-10"); -- 10： from 2019-01-01 to 2019-10-10 have 10 months 
select year("2019-10-10"); -- 2019

-- `partition` can range to_days() and year()

-- alter table add partition
alter table test_partition partition by range (to_days(time_test)) (
partition p20191001 values less than (to_days("2019-10-01")), 
partition p20191101 values less than (to_days("2019-11-01")));

alter table test_partition add partition(partition p20191201 VALUES LESS THAN (to_days("2019-12-01") ) );

-- alter table drop partition ,but this table must have one partition
ALTER TABLE test_partition DROP PARTITION p20191201;

-- select table partition

SELECT
  partition_name part,
  partition_expression expr,
  partition_description descr,
  FROM_DAYS(partition_description) lessthan_sendtime,
  table_rows
FROM
  INFORMATION_SCHEMA.partitions
WHERE
  TABLE_SCHEMA = SCHEMA() AND TABLE_NAME='test_partition';


-- 事务
DELIMITER $$

USE `test`$$

DROP PROCEDURE IF EXISTS `create_partition`$$
/*这里复制的时候注意自己的登录用户*/
CREATE DEFINER=`root`@`localhost` PROCEDURE `create_partition_test`(IN tableName VARCHAR(20),IN interval_ INT)
BEGIN
/* 事务回滚，其实放这里没什么作用，ALTER TABLE是隐式提交，回滚不了的 */
    DECLARE EXIT HANDLER FOR SQLEXCEPTION ROLLBACK;
    START TRANSACTION;
  
 /* 到系统表查出这个表的最大分区，得到最大分区的日期。在创建分区的时候，名称就以日期格式存放，方便后面维护 */
    SELECT REPLACE(partition_name,'p','') INTO @P12_Name FROM INFORMATION_SCHEMA.PARTITIONS 
    WHERE table_name=tableName ORDER BY partition_ordinal_position DESC LIMIT 1;
      SET @Max_date= DATE(DATE_ADD(@P12_Name+0, INTERVAL interval_ DAY))+0;
 /* 修改表，在最大分区的后面增加一个分区，时间范围加 interval_ 天 */
     SET @s1=CONCAT('ALTER TABLE ' ,tableName ,' ADD PARTITION (PARTITION p',@Max_date,' VALUES LESS THAN (TO_DAYS (''',DATE(@Max_date),''')))');
    /* 输出查看增加分区语句*/
     SELECT @s1;
   PREPARE stmt2 FROM @s1;
     EXECUTE stmt2;
    DEALLOCATE PREPARE stmt2;
 /* 提交 */
     COMMIT ;
  END$$

DELIMITER ;

```