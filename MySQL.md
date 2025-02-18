# MySQL

## 分页

> 分页查询employees表，每5行一页，返回第2页的数据

```sql
select * from employees limit 5,5
```

###  LIMIT 关键字

注意：在 LIMIT X,Y 中，Y代表返回几条记录，X代表从第几条记录开始返回（第一条记录序号为0），切勿记反。

## 查找

>**查找最晚入职员工employees的所有信息**

| emp_no | birth_date | first_name | last_name | gender | hire_date   |
| ------ | ---------- | ---------- | --------- | ------ | ----------- |
| 10001  | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26  |
| 10002  | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21  |
| 10003  | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28  |
| 10004  | 1954-05-01 | Christian  | Koblick   | M      | 1986-12-01  |
| 10005  | 1955-01-21 | Kyoichi    | Maliniak  | M      | 1989-09-12' |
| 10006  | 1953-04-20 | Anneke     | Preusig   | F      | 1989-06-02  |
| 10007  | 1957-05-23 | Tzvetan    | Zielinski | F      | 1989-02-10  |
| 10008  | 1958-02-19 | Saniya     | Kalloufi  | M      | 1994-09-15  |
| 10009  | 1952-04-19 | Sumant     | Peac      | F      | 1985-02-18  |
| 10010  | 1963-06-01 | Duangkaew  | Piveteau  | F      | 1989-08-24  |
| 10011  | 1953-11-07 | Mary       | Sluis     | F      | 1990-01-22  |

```sq
select * from  employees where hire_date = (select max(hire_date) from employees)
```

### 子查询

先找出 hire_date 字段的最大值，再把该值当成 employees 表的hire_date 查询条件。 

> 查找入职员工时间升序排名的情况下的倒数第三的员工所有信息employees

| emp_no | birth_date | first_name | last_name | gender | hire_date  |
| ------ | ---------- | ---------- | --------- | ------ | ---------- |
| 10001  | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
| 10002  | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |
| 10003  | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |
| 10004  | 1954-05-01 | Christian  | Koblick   | M      | 1986-12-01 |

> 注意：可能会存在同一个日期入职的员工，所以入职员工时间排名倒数第三的员工可能不止一个,存在多个员工的情况按照emp_no升序排列。

```sql
select * from employees where hire_date=(select distinct hire_date from employees order by hire_date desc limit 2,1);
```

### 去重 DISTINCT

加distinct去重，去除多余的日期条数，比如有3个1月2 使用limit之后还是1月2 如果加了distinct 就只会留下一个1月2 达到搜索

hire_date 倒数第三天（使用limit x y实现）的日子 作为 子查询的条件 desc升序排列

```sql
select 
distinct
salary
from
salaries
order by
salary
desc
```

### 降序排列

desc将日期

从晚到早

排序，此时：

- 第1行 → 最晚日期（倒数第1名）
- 第2行 → 第二晚日期（倒数第2名）
- 第3行 → 第三晚日期（倒数第3名）

> 查找当前薪水详情以及部门编号dept_no

有一个全部员工的薪水表salaries简况如下:

| emp_no | salary | from_date  | to_date    |
| ------ | ------ | ---------- | ---------- |
| 10001  | 88958  | 2002-06-22 | 9999-01-01 |
| 10002  | 72527  | 2001-08-02 | 9999-01-01 |
| 10003  | 43311  | 2001-12-01 | 9999-01-01 |

有一个各个部门的领导表dept_manager简况如下:

| dept_no | emp_no | to_date    |
| ------- | ------ | ---------- |
| d001    | 10001  | 9999-01-01 |
| d002    | 10003  | 9999-01-01 |

请你查找各个部门当前领导的薪水详情以及其对应部门编号dept_no，输出结果以salaries.emp_no升序排序，并且请注意输出结果里面dept_no列是最后一列。

```sql
select sa.emp_no, sa.salary , sa.from_date,sa.to_date, de.dept_no from salaries sa right join dept_manager de on sa.emp_no=de.emp_no order by emp_no
```

### 表简称

在 from 表的时候 salaries sa 相当于sa作为表salaries的简称 再选取salaries表的元素可以sa.

### 右连接 RIGHT JOIN 与 ON 关键词

右连接需要有 on 关键词 ：只有当 `salaries.emp_no` 等于 `dept_manager.emp_no` 时，两表的行才会被连接。

salaries sa right join dept_manager de on sa.emp_ no=de.emp_no 

### ORDER BY 关键词

order by emp_no 默认升序

> 查找所有员工的last_name和first_name以及对应部门编号dept_no

有一个员工表，employees简况如下:

| emp_no | birth_date | first_name | last_name | gender | hire_date  |
| ------ | ---------- | ---------- | --------- | ------ | ---------- |
| 10001  | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
| 10002  | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |
| 10003  | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |
| 10004  | 1954-05-01 | Christian  | Koblick   | M      | 1986-12-01 |

有一个部门表，dept_emp简况如下:

| emp_no | dept_no | from_date  | to_date    |
| ------ | ------- | ---------- | ---------- |
| 10001  | d001    | 1986-06-26 | 9999-01-01 |
| 10002  | d002    | 1989-08-03 | 9999-01-01 |

请你查找所有已经分配部门的员工的last_name和first_name以及dept_no，也包括暂时没有分配具体部门的员工

```sql
select 
em.last_name ,
em.first_name ,
de.dept_no
from
employees em 
left join
dept_emp de
on
em.emp_no  = de.emp_no
```

### 左连接 LEFT JOIN 内连接 INNER JOIN

INNER JOIN 两边表同时有对应的数据，即任何一边缺失数据就不显示。
LEFT JOIN 会读取左边数据表的全部数据，即便右边表无对应数据。
RIGHT JOIN 会读取右边数据表的全部数据，即便左边表无对应数据。

>查找薪水记录超过15条的员工号emp_no以及其对应的记录次数t

有一个薪水表，salaries简况如下:

| emp_no | salary | from_date  | to_date    |
| ------ | ------ | ---------- | ---------- |
| 10001  | 60117  | 1986-06-26 | 1987-06-26 |
| 10001  | 62102  | 1987-06-26 | 1988-06-25 |
| 10001  | 66074  | 1988-06-25 | 1989-06-25 |
| 10001  | 66596  | 1989-06-25 | 1990-06-25 |
| 10001  | 66961  | 1990-06-25 | 1991-06-25 |
| 10001  | 71046  | 1991-06-25 | 1992-06-24 |
| 10001  | 74333  | 1992-06-24 | 1993-06-24 |
| 10001  | 75286  | 1993-06-24 | 1994-06-24 |
| 10001  | 75994  | 1994-06-24 | 1995-06-24 |
| 10001  | 76884  | 1995-06-24 | 1996-06-23 |
| 10001  | 80013  | 1996-06-23 | 1997-06-23 |
| 10001  | 81025  | 1997-06-23 | 1998-06-23 |
| 10001  | 81097  | 1998-06-23 | 1999-06-23 |
| 10001  | 84917  | 1999-06-23 | 2000-06-22 |
| 10001  | 85112  | 2000-06-22 | 2001-06-22 |
| 10001  | 85097  | 2001-06-22 | 2002-06-22 |
| 10002  | 72527  | 1996-08-03 | 1997-08-03 |

请你查找薪水记录超过15条的员工号emp_no以及其对应的记录次数t

```sql
select emp_no,count(salary) t  
from salaries  
group by emp_no 
having t>15
```

### 聚合函数COUNT() 

COUNT(salary) 聚合函数，统计某个字段的非空值数量。 如果只是统计行数（无论字段是否为空），可以用 `COUNT(*)`。

### 按字段分组GROUP BY

按指定字段分组，将相同 `emp_no` 的记录合并为一行。

按 `emp_no` 分组，每个分组对应一个员工的全部薪水记录。

分组后，`COUNT(salary)` 会计算每个分组内的记录数。

### 聚合后的条件 HAVING 

对分组后的结果进行过滤，类似 `WHERE`，但用于聚合后的条件。

过滤出分组后记录数 `t` 大于 15 的员工。

`HAVING` 必须与 `GROUP BY` 一起使用，且可以引用聚合函数结果（如 `COUNT(salary)` 或别名 `t`）。

### HAVING 与 WHERE

**`WHERE`**：在分组前过滤原始数据（不能使用聚合函数）。

**`HAVING`**：在分组后过滤聚合结果（可以使用聚合函数或别名）。

> 获取所有非manager的员工emp_no

有一个员工表employees简况如下:

| emp_no | birth_date | first_name | last_name | gender | hire_date  |
| ------ | ---------- | ---------- | --------- | ------ | ---------- |
| 10001  | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
| 10002  | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |
| 10003  | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |

有一个部门领导表dept_manager简况如下:

| dept_no | emp_no | from_date  | to_date    |
| ------- | ------ | ---------- | ---------- |
| d001    | 10002  | 1996-08-03 | 9999-01-01 |
| d002    | 10003  | 1990-08-05 | 9999-01-01 |

请你找出所有非部门领导的员工emp_no

```sql
SELECT emp_no 
FROM employees
WHERE emp_no 
NOT IN (SELECT emp_no FROM dept_manager)
```

```sql
SELECT e.emp_no
FROM employees e
LEFT JOIN dept_manager dm ON e.emp_no = dm.emp_no
WHERE dm.emp_no IS NULL
```

### NOT IN 关键词

通过子查询利用NOT IN 达到目标

检查 `employees.emp_no` 是否 **不在** 子查询的结果集中。 

如果子查询返回的 `emp_no` 列表中存在 `NULL` 值，`NOT IN` 的逻辑会失效
