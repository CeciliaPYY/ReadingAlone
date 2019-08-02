# 字符串处理

## 6.10 创建分隔列表

### 问题

你想把行数据变成以某种符号分隔的列表，例如以逗号分隔，而不是常见的竖排的列数据形式。你希望转换下面的结果集。

| deptno | ename  |
| ------ | ------ |
| 20     | SMITH  |
| 30     | ALLEN  |
| 30     | WARD   |
| 20     | JONES  |
| 30     | MARTIN |
| 30     | BLAKE  |
| 10     | CLARK  |
| 20     | SCOTT  |
| 10     | KING   |
| 30     | TURNER |
| 20     | ADAMS  |
| 30     | JAMES  |
| 20     | FORD   |
| 10     | MILLER |

从上面这张表转换成下面这张表，

| deptno | emps                                 |
| ------ | ------------------------------------ |
| 10     | CLARK,KING,MILLER                    |
| 20     | SMITH,JONES,SCOTT,ADAMS,FORD         |
| 30     | ALLEN,WARD,MARTIN,BLAKE,TURNER,JAMES |



### 解决方法

使用 group_concat 方法。

```
select deptno,
group_concat(ename order by empno separator ',') as emps
from emp
WHERE deptno is not NULL
group by deptno
```





## 6.12 按字母表顺序排列字符

### 问题

你想按照字母表顺序对字符串里的字符进行排序，考虑下面的结果集。

| ename  |
| ------ |
| SMITH  |
| ALLEN  |
| WARD   |
| JONES  |
| MARTIN |
| BLAKE  |
| CLARK  |
| SCOTT  |
| KING   |
| TURNER |
| ADAMS  |
| JAMES  |
| FORD   |
| MILLER |
| YODA   |

你希望得到如下所示的结果集。

| old_name | new_name |
| -------- | -------- |
| ADAMS    | AADMS    |
| ALLEN    | AELLN    |
| BLAKE    | ABEKL    |
| CLARK    | ACKLR    |
| FORD     | DFOR     |
| JAMES    | AEJMS    |
| JONES    | EJNOS    |
| KING     | GIKN     |
| MARTIN   | AIMNRT   |
| MILLER   | EILLMR   |
| SCOTT    | COSTT    |
| SMITH    | HIMST    |
| TURNER   | ENRRTU   |
| WARD     | ADRW     |
| YODA     | ADOY     |



### 解决方法

这里的关键是 `group_concat` 函数，该函数不仅能连接员工名字字符串里的每个字符，还能对它们进行排序。

思路是先将每个名字分隔成一个个的字母，然后对字母进行排序。

以下这张表是其中的一个中间过程。

| ename  | c    |
| ------ | ---- |
| SMITH  | S    |
| SMITH  | M    |
| SMITH  | I    |
| SMITH  | T    |
| SMITH  | H    |
| ALLEN  | A    |
| ALLEN  | L    |
| ALLEN  | L    |
| ALLEN  | E    |
| ALLEN  | N    |
| WARD   | W    |
| WARD   | A    |
| WARD   | R    |
| WARD   | D    |
| JONES  | J    |
| JONES  | O    |
| JONES  | N    |
| JONES  | E    |
| JONES  | S    |
| MARTIN | M    |
| MARTIN | A    |
| MARTIN | R    |
| MARTIN | T    |
| MARTIN | I    |
| MARTIN | N    |
| BLAKE  | B    |
| BLAKE  | L    |
| BLAKE  | A    |
| BLAKE  | K    |
| BLAKE  | E    |
| CLARK  | C    |
| CLARK  | L    |
| CLARK  | A    |
| CLARK  | R    |
| CLARK  | K    |
| SCOTT  | S    |
| SCOTT  | C    |
| SCOTT  | O    |
| SCOTT  | T    |
| SCOTT  | T    |
| KING   | K    |
| KING   | I    |
| KING   | N    |
| KING   | G    |
| TURNER | T    |
| TURNER | U    |
| TURNER | R    |
| TURNER | N    |
| TURNER | E    |
| TURNER | R    |
| ADAMS  | A    |
| ADAMS  | D    |
| ADAMS  | A    |
| ADAMS  | M    |
| ADAMS  | S    |
| JAMES  | J    |
| JAMES  | A    |
| JAMES  | M    |
| JAMES  | E    |
| JAMES  | S    |
| FORD   | F    |
| FORD   | O    |
| FORD   | R    |
| FORD   | D    |
| MILLER | M    |
| MILLER | I    |
| MILLER | L    |
| MILLER | L    |
| MILLER | E    |
| MILLER | R    |
| YODA   | Y    |
| YODA   | O    |
| YODA   | D    |
| YODA   | A    |

sql语句如下，

```
select x.ename as old_name, group_concat(x.c order by x.c separator '') as new_name
from 
(select emp.ename, substr(emp.ename, iter.pos, 1) c 
from emp,
(select id pos from t10) iter
where iter.pos <= length(emp.ename)) x
group by x.ename
```

