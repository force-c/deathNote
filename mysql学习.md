[TOC]

# 锁

## mysql行锁

```sh
如果更新条件字段为索引列则锁行
如果更新条件字段为非索引列则锁表
验证如下
```

```sql
#left
SHOW VARIABLES LIKE 'autocommit';
SET autocommit = 0;  
update Log  set log_name=30 WHERE log_name = 21 
commit;

#right
SHOW VARIABLES LIKE 'autocommit';
SET autocommit = 0;  
update Log  set log_name=22 WHERE log_name = 21;
update Log  set log_name=22 WHERE id = 11;
```



![image-20210326112530449](C:\Users\guochuang\AppData\Roaming\Typora\typora-user-images\image-20210326112530449.png)

