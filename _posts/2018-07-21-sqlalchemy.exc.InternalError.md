---
layout: post
title:  "sqlalchemy.exc.InternalError"
date:   2018-07-21 13:13:52
categories: Error
tags: Flask Python Error
excerpt: Flask运行SQLAlchemy中数据库连接错误
---

* content
{:toc}
在将本地项目放到云端执行时出现一个错误

完整错误信息

> # sqlalchemy.exc.InternalError
>
> sqlalchemy.exc.InternalError: (pymysql.err.InternalError) (1055, "Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'fisher.gift.create_time' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by") [SQL: 'SELECT DISTINCT gift.create_time AS gift_create_time, gift.status AS gift_status, gift.id AS gift_id, gift.uid AS gift_uid, gift.isbn AS gift_isbn, gift.launched AS gift_launched \nFROM gift \nWHERE gift.launched = false AND gift.status = %s GROUP BY gift.isbn ORDER BY gift.create_time DESC \n LIMIT %s'][parameters: (1, 30)] (Background on this error at: http://sqlalche.me/e/2j85)

显示为MySQL驱动错误，更新pymysql和替换为cpmysql均不行，再回过头仔细看错误内容，发现是MySQL查询错误。

经过检查，发现本地是5.6版本，云端是5.7版本。

主要问题是

> ERROR 1055 (42000): Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'column' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by



**ONLY_FULL_GROUP_BY**的意思是：对于GROUP BY聚合操作，如果在SELECT中的列，没有在GROUP BY中出现，那么这个SQL是不合法的，因为列不在GROUP BY从句中，也就是说查出来的列必须在group by后面出现否则就会报错，或者这个字段出现在聚合函数里面。

官网摘抄：[ONLY_FULL_GROUP_BY](https://dev.mysql.com/doc/refman/5.7/en/sql-mode.html#sqlmode_only_full_group_by) 
Reject queries for which the select list, HAVING condition, or ORDER BY list refer to nonaggregated columns that are neither named in the GROUP BY clause nor are functionally dependent on (uniquely determined by) GROUP BY columns.

As of MySQL 5.7.5, the default SQL mode includes ONLY_FULL_GROUP_BY. (Before 5.7.5, MySQL does not detect functional dependency and ONLY_FULL_GROUP_BY is not enabled by default. For a description of pre-5.7.5 behavior, see the MySQL 5.6 Reference Manual.)

A MySQL extension to standard SQL permits references in the HAVING clause to aliased expressions in the select list. Before MySQL 5.7.5, enabling ONLY_FULL_GROUP_BY disables this extension, thus requiring the HAVING clause to be written using unaliased expressions. As of MySQL 5.7.5, this restriction is lifted so that the HAVING clause can refer to aliases regardless of whether ONLY_FULL_GROUP_BY is enabled.

MySQL 5.7版本默认的sql_mode就是ONLY_FULL_GROUP_BY，查询语句需要更严谨，所以出现问题了。



### 解决方案

```mysql
set global sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
```

