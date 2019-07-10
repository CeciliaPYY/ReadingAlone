# 第1章 检所记录

## 1.6 在 WHERE 子句中引用别名列

```
select sal as salary, comm as commission
from emp
where salary < 5000
```

以上这种写法是错误的，可以把把查询包装为一个内嵌视图，这样就可以引用别名列了。

```
select * from
(select sal as salary, comm as commission
from emp) x
where salary < 5000
```



当你想在 WHERE 子句中引用下列内容时，

- 聚合函数；
- 标量子查询；
- 窗口函数；
- 别名；

将含有别名列的查询放入内嵌视图，就可以在外层查询中引用别名列。这样做的原理是，

WHERE 子句、SELECT 子句 以及 FROM 子句 的运行顺序如下，

**FROM 子句 > WHERE 子句 > SELECT 子句先执行**



## 1.7 串联多列的值

### 问题

你想将多列的值合并为一列。例如，你想查询 EMP 表，并获得如下结果集。



### 解决方法

1. DB2、Oracle 和 PostgreSQL

   ```
   select ename||' WORKS AS A '||job as msg
   from emp
   where deptno=10
   ```

2. MySQL

   ```
   select concat(ename, ' WORKS AS A ',job) as msg
   from emp
   where deptno=10
   ```

3. SQL Server

   ```
   select ename + ' WORKS AS A ' + job as msg
   from emp
   where deptno=10
   ```



## 1.8 在SELECT语句里使用条件逻辑

### 问题

你想在 SELECT 语句中针对查询结果值执行 IF-ELSE 操作。



### 解决方法

```
select ename,sal,
case when sal <= 2000 then 'UNDERPAID'
          when sal >= 4000 then 'OVERPAID'
          else 'OK'
     end as status
from emp
```

其中，ELSE 子句是可选的，若没有它，对于不满足测试条件的行，CASE 表达式会返回 Null。



## 1.10 随机返回若干行记录

### 问题

你希望从表中获取特定数量的随机记录。



### 解决方法

1. DB2

   ```
   select ename,job
   from emp
   order by rand() fetch first 5 rows only
   ```

2. MySQL

   ```
   select ename,job
   from emp
   order by rand() limit 5
   ```

3. PostgreSQL

   ```
   select ename,job
   from emp
   order by random() limit 5
   ```

4. Oracle

   ```
   select *
   from (
   select ename, job
   from emp
   order by dbms_random.value() 7)
   where rownum <= 5
   ```

5. SQL Server

   ```
   select top 5 ename,job
   from emp
   order by newid()
   ```



### 讨论

- ORDER BY 子句能够接受一个函数的返回值，并利用该值改变当前结果集的顺序；



## 1.12 把Null值转换为实际值

### 问题

有一些行包含 Null 值，但是你想在返回结果里将其替换为非 Null 值。



### 解决方法

```
select coalesce(comm,0)
from emp
```



### 讨论

需要为 COALESCE 函数指定一个或多个参数。该函数会返回参数列表里的第一个非 Null 值。