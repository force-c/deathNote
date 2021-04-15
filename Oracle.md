

[TOC]

# 存储过程

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

