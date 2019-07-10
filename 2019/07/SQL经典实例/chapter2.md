# 第2章 查询结果排序

## 2.2 多字段排序

### 问题

针对 EMP 表的数据，你想先按照 DEPTNO 升序排列，然后再按照 SAL 降序排列。你希望返回
如下所示的结果集。



### 解决方法

在 ORDER BY 子句中列出不同的排序列，以逗号分隔。



### 讨论

- ORDER BY 的执行顺序是从左到右的；
- 如果你的查询语句里有 GROUP BY 或 DISTINCT，那么就不能按照 SELECT 列表之外的列进行排序；



## 2.3 依据子串排序

### 问题

你想按照一个字符串的特定部分排列查询结果。例如，你希望从 EMP 表检索员工的名字和职位，并且按照职位字段的最后两个字符对检索结果进行排序。



### 解决方法

1. DB2、MySQL、Oracle 和 PostgreSQL

   ```
   select ename,job
   from emp
   order by substr(job,length(job)-2)
   ```

2. SQL Server

   ```
   select ename,job
   from emp
   order by substring(job,len(job)-2,2)
   ```

