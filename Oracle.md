

[TOC]

# Oracle

## oracle分页

> https://blog.csdn.net/use_admin/article/details/83622414

> **1.无ORDER BY排序的写法。(效率最高)**
> (经过测试，此方法成本最低，只嵌套一层，速度最快！即使查询的数据量再大，也几乎不受影响，速度依然！)

```plsql
SELECT *
  FROM (SELECT ROWNUM AS rowno, t.*
          FROM emp t
         WHERE hire_date BETWEEN TO_DATE ('20060501', 'yyyymmdd')
                             AND TO_DATE ('20060731', 'yyyymmdd')
           AND ROWNUM <= 20) table_alias
 WHERE table_alias.rowno >= 10;
```

> **2.有ORDER BY排序的写法。(效率较高)**
> (经过测试，此方法随着查询范围的扩大，速度也会越来越慢哦！)

```plsql
SELECT *
  FROM (SELECT tt.*, ROWNUM AS rowno
          FROM (  SELECT t.*
                    FROM emp t
                   WHERE hire_date BETWEEN TO_DATE ('20060501', 'yyyymmdd')
                                       AND TO_DATE ('20060731', 'yyyymmdd')
                ORDER BY create_time DESC, emp_no) tt
         WHERE ROWNUM <= 20) table_alias
 WHERE table_alias.rowno >= 10;
```

> **3.无ORDER BY排序的写法。(建议使用方法1代替)**
> (此方法随着查询数据量的扩张，速度会越来越慢哦！)

```plsql
SELECT *
  FROM (SELECT ROWNUM AS rowno, t.*
          FROM k_task t
         WHERE flight_date BETWEEN TO_DATE ('20060501', 'yyyymmdd')
                               AND TO_DATE ('20060731', 'yyyymmdd')) table_alias
 WHERE table_alias.rowno <= 20 AND table_alias.rowno >= 10;
--TABLE_ALIAS.ROWNO  between 10 and 100;
```

> **4.有ORDER BY排序的写法.(建议使用方法2代替)**
> (此方法随着查询范围的扩大，速度会越来越慢哦！)

```plsql
SELECT *
  FROM (SELECT tt.*, ROWNUM AS rowno
          FROM (  SELECT *
                    FROM k_task t
                   WHERE flight_date BETWEEN TO_DATE ('20060501', 'yyyymmdd')
                                         AND TO_DATE ('20060531', 'yyyymmdd')
                ORDER BY fact_up_time, flight_no) tt) table_alias
 WHERE table_alias.rowno BETWEEN 10 AND 20;
```





## 存储过程

```plsql
-- 定义无参游标
declare
 -- 定义游标 is关键字后面是查询语句
 cursor cursor_me is SELECT project.PROJECTID FROM CSCADMIN.TBL_ABROADPROJECT project WHERE project.PARENTID = '-1';
 -- 定义接收变量
 P_ID CSCADMIN.TBL_ABROADPROJECT.PROJECTID%TYPE;
begin
  -- 开启游标
  OPEN cursor_me;
  -- 开启循环
  LOOP
  -- 获取游标中的数据（into 关键字后面的变量要和查询出来的字段位置一样）
  FETCH cursor_me INTO P_ID;
  -- 退出循环条件 当cursor_me 取不到数据
  EXIT WHEN cursor_me%NOTFOUND;
  -- 输出遍历的P_ID
  dbms_output.put_line(P_ID);
  -- 结束循环
  END LOOP;
  --关闭游标
  CLOSE cursor_me;
end;
```

