---
title: hbase shell
date: 2018.1.1 16:36:45
reward: false
tags: 
    - big-data
    - hbase shell
---

### shell

```shell script
create 't1', 'f' # 创建表 t

describe 't1' # 查看表信息

scan 't1' #查看表数据

put 't1', 'row5', 'f:f','value3',{VERSIONS=>1}  #插入数据,指定version

put 't1', 'row5', 'f:f','value1'  #普通插入数据

alter 't1', {NAME=>'f', VERSIONS=>3} # update cloumn famliy = `f` version

alter 't1', {NAME=>'f', MIN_VERSIONS=>3}  # update cloumn famliy = `f` min version must not is 0 that version effect 

alter 't1', {NAME=>b} # add a new family

get 't1', 'row5',{COLUMN=>'f:f'} # get latest data

get 't1', 'row5',{COLUMN=>'f:f',VERSIONS=>3} # get three data

delete 't1', 'row5', 'b:a' #delete all data

delete 't1', 'row5', {COLUMN='f:f', VERSIONS=>1} # delete VERSIONS is 1 , rowkey is row5, famliy name is  'f:f'

```

