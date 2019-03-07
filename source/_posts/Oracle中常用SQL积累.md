---
title: Oracle中常用SQL积累
date: 2016-08-20 11:00:38
tags: [Oracle,SQL]
---

### 增删用户、表空间，授权操作
删除用户
``` SQL
  DROP USER username CASCADE;
```

如果用户被占用则需要先关闭连接用户再删除
1、先查询指定连接用户的sid和serial#
``` SQL
  SELECT SID,SERIAL# FROM V$SESSION WHERE USERNAME = 'USERNAME';
```
2、再关闭指定的连接用户
``` SQL
  ALTER SYSTEM KILL SESSION '135,1385'; --参数为查询出的值
```

<!-- more -->

删除表空间
``` SQL
DROP TABLESPACE tablespace_name INCLUDING CONTENTS AND DATAFILES;
```

如果表空间被占用则需先对表空间做脱机处理然后再删除
``` SQL
  ALTER TABLESPACE tablespace_name OFFLINE;
```

创建表空间
``` SQL
  CREATE TABLESPACE TBS_HJYJ_HY                 --设置表空间的名称
  DATAFILE 'E:\Database\DBF\TBS_HJYJ_HY.DBF'    --设置表空间的位置
  SIZE 10M
  AUTOEXTEND ON NEXT 10M
  MAXSIZE UNLIMITED;
```

创建用户名并设置密码
``` SQL
  CREATE USER U_HJYJ_HY IDENTIFIED BY U_HJYJ_HY;
```

为用户指定默认的表空间
``` SQL
ALTER USER U_HJYJ_HY DEFAULT TABLESPACE TBS_HJYJ_HY;
```

创建用户名并设置密码，同时给创建的用户指定默认的表空间
``` SQL
CREATE USER U_HJYJ_HY IDENTIFIED BY U_HJYJ_HY DEFAULT TABLESPACE TBS_HJYJ_HY;
```

修改用户名并设置密码，同时给创建的用户指定默认的表空间
``` SQL
ALTER USER U_HJYJ_HY IDENTIFIED BY U_HJYJ_HY DEFAULT TABLESPACE TBS_HJYJ_HY;
```

给用户授权
``` SQL
  GRANT
        CREATE USER,DROP USER,ALTER USER,            --授予创建/删除/修改用户的权限
        CREATE ANY VIEW,DROP ANY VIEW,               --授予创建/删除试图的权限
        EXP_FULL_DATABASE,IMP_FULL_DATABASE,         --授予DBA,CONNECT,RESOURCE,CREATE SESSION权限
        DBA,CONNECT,RESOURCE,CREATE SESSION
  TO U_HJYJ_HY;
```

授权
``` SQL
    GRANT CONNECT TO U_JZXX_HY;
    GRANT SELECT ON T_HJXF_JZXX TO U_JZXX_HY;
    GRANT INSERT ON T_HJXF_JZXX TO U_JZXX_HY;
    GRANT UPDATE ON T_HJXF_JZXX TO U_JZXX_HY;
    GRANT DELETE ON T_HJXF_JZXX TO U_JZXX_HY;
    GRANT CREATE SYNONYM TO U_JZXX_HY;
    CREATE OR REPLACE SYNONYM T_HJXF_JZXX  FOR U_HJXF_HY.T_HJXF_JZXX;
```

说明：
> * DBA: 拥有全部特权，是系统最高权限，只有DBA才可以创建数据库结构。
> * RESOURCE:拥有Resource权限的用户只可以创建实体，不可以创建数据库结构。
> * CONNECT:拥有Connect权限的用户只可以登录Oracle，不可以创建实体，不可以创建数据库结构。

### 对数据表的操作

``` sql
-- 修改指定表指定字段的数据类型和长度
alter table <表名> modify <字段名> <数据类型和长度>;
alter table milestone modify description NVARCHAR2(500);

-- 添加字段信息
alter table <表名> add <字段名> <数据类型和长度>;
alter table project add COMPLETED_TIME TIMESTAMP(6);

-- 添加注释
comment on column <表名>.<字段名> is <'注释信息'>;
comment on column PROJECT.COMPLETED_SUBMITTER is '竣工提交人';
```





### 普通的数据库备份和还原

备份数据库（直接在dos中执行）   语法：用户名/密码/@数据库
``` SQL
  -- 导出本地数据库
  EXP U_HJYJ_HY/U_HJYJ_HY@ORCL ROWS=YES
  FILE='E:\Database\NAME.DMP' LOG='E:\Database\NAME.LOG'
  
  -- 导出远程数据库
  EXP USERNAME/PASSWORD@10.0.0.1:1521/ORCL FILE='E:\Database\NAME.DMP'              LOG='E:\Database\NAME.LOG' ROWS=YES
```

如果需要对空表也进行备份，执行查询出的SQL语句即可
``` SQL
  SELECT 'ALTER TABLE ' || TABLE_NAME || ' ALLOCATE EXTENT;' FROM USER_TABLES WHERE NUM_ROWS = 0;  
```

还原数据库（直接在dos中执行）  语法：用户名/密码/@数据库
``` SQL
  IMP U_HJYJ_HY/U_HJYJ_HY@ORCL FULL=Y
  FILE='E:\Database\U_HJYJ_HY.DMP'  LOG='E:\Database\U_HJYJ_HY.LOG'
```


### 通过高速泵备份还原数据库

1. 创建文件夹目录和授权（在PL/SQL中执行）
``` SQL
  CREATE OR REPLACE DIRECTORY EXP_DIR AS 'E:\DATABASE\XZCF';
```
2. 授权目录相关用户（在PL/SQL中执行）
``` SQL
  GRANT READ,WRITE ON DIRECTORY EXP_DIR TO PUBLIC;
```
3. 测试目录是否创建成功
``` SQL
  SELECT * FROM DBA_DIRECTORIES;
```

4. 数据库备份
``` SQL
  EXPDP U_HJYJ_HY/U_HJYJ_HY@ORCL DIRECTORY=EXP_DIR DUMPFILE=WRJPGZ.DMP LOGFILE=WRJPGZ.LOG;
```

5. 数据库还原
``` SQL
  IMPDP U_HJYJ_HY/U_HJYJ_HY@ORCL DIRECTORY=EXP_DIR DUMPFILE=U_HJYJ_HYS.DMP LOGFILE=U_HJYJ_HYS.LOG;
```
说明：
> * WRJPGZ.DMP备份到E:\DATABASE\XZCF目录下；
> * U_HJYJ_HYS.DMP必须在E:\DATABASE\XZCF目录下；
> * 如果提示.LOG不能访问，则需在对应目录下建该.LOG文件再执行

### 备份前先清理无用数据

清理系统访问日志(老系统)
``` SQL
  DELETE FORM T_ADMIN_RMS_XTFWRZ;
```

清理系统访问日志(环境应急指挥管理系统)
``` SQL
  DELETE FROM T_ADMIN_XTFWRZ;
```

### 常用SQL查询
查询字符集
``` SQL
  SELECT * FROM NLS_DATABASE_PARAMETERS;
```

查询用户所在的表空间
``` SQL
  SELECT USERNAME,DEFAULT_TABLESPACE FROM DBA_USERS  WHERE USERNAME='用户名';
```

查看所有表的数据量并排序

``` sql
SELECT TABLE_NAME, NUM_ROWS FROM USER_TABLES ORDER BY NUM_ROWS DESC;
-- 还可以直接查看dblink的：
SELECT TABLE_NAME, NUM_ROWS FROM USER_TABLES@DBLINK ORDER BY NUM_ROWS DESC;
```

查询数据库版本

``` SQL
  SELECT * FROM V$VERSION;
```
通过命令行远程连接数据库
``` SQL
  SQLPLUS 用户名/密码@IP:端口/实例名
  如：
  SQLPLUS U_HJYJ_HY/U_HJYJ_HY@192.168.0.1:1521/orcl
```

在工作中遇到了一个这样的问题：更新一个表中的数据， 但是这个表的数据是根据多个表才能查到， 即通过 select 查询出结果后，在通过查询出的结果 **修改** 或者 **添加** 数据： 

**添加** ：

insert的语句中查询出的字段一定要和需要插入的字段保持一致，不一致的话就使用"as" 用别名使之保持一致,否之无法插入；

``` sql
 INSERT INTO  z_test (  
 user_id,  
 user_name,  
 book_name,  
 game_name  
)  
select   
u.user_id,  
u.name as user_name,  
b.book_name,  
g.game_name   
from   
z_user u LEFT JOIN z_book b ON u.book_id = b.book_id   
LEFT JOIN z_game g ON u.game_id = g.game_id   
where  u.user_id = 1  
```

**修改1** ：

``` sql
UPDATE z_test t,z_user u,z_book b,z_game g SET  
t.user_name = u.name,  
t.book_name = b.book_name,  
t.game_name = g.game_name  
WHERE 1=1  
and t.user_id  =  u.user_id  
and b.book_id  =  u.book_id  
and g.game_id  =  u.game_id  
and u.user_id = 1  
```

**修改2** ：

``` sql
UPDATE A a  SET (a.a1,a.a2)=(SELECT b.b1,b,b2 FROM B b WHERE b.b3=a.a3)
```

