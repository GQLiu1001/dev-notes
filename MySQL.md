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

### 左连接 LEFT JOIN 内连接 INNER JOIN 默认内连接

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

使用group by子句时，select子句中只能有聚合键、聚合函数、常数。

gruop by 之后直接查询emp_no默认取非聚合的第一条,不符合某些场景

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

> 获取所有员工当前的manager

有一个员工表dept_emp简况如下:

| emp_no | dept_no | from_date  | to_date    |
| ------ | ------- | ---------- | ---------- |
| 10001  | d001    | 1986-06-26 | 9999-01-01 |
| 10002  | d001    | 1996-08-03 | 9999-01-01 |
| 10003  | d002    | 1995-12-03 | 9999-01-01 |

第一行表示为员工编号为10001的部门是d001部门。

有一个部门经理表dept_manager简况如下:

| dept_no | emp_no | from_date  | to_date    |
| ------- | ------ | ---------- | ---------- |
| d001    | 10002  | 1996-08-03 | 9999-01-01 |
| d002    | 10003  | 1990-08-05 | 9999-01-01 |

第一行表示为d001部门的经理是编号为10002的员工。

获取所有的员工和员工对应的经理，如果员工本身是经理的话则不显示

```sql
select a.emp_no, b.emp_no as manager_no
from dept_emp as a join dept_manager as b 
on a.dept_no = b.dept_no
and a.emp_no != b.emp_no
```

### AND 关键词

增加条件

### ！= 关键词

不等于的限制条件

### AS 关键词

可以作为别名 也可省略as 本质上输出的都是emp_no

```sql
select a.emp_no, b.emp_no 
from dept_emp  a 
join 
dept_manager  b 
on a.dept_no = b.dept_no
and a.emp_no != b.emp_no
```

> 获取每个部门中薪水最高的员工相关信息

有一个员工表dept_emp简况如下:

| emp_no | dept_no | from_date  | to_date    |
| ------ | ------- | ---------- | ---------- |
| 10001  | d001    | 1986-06-26 | 9999-01-01 |
| 10002  | d001    | 1996-08-03 | 9999-01-01 |
| 10003  | d002    | 1996-08-03 | 9999-01-01 |

有一个薪水表salaries简况如下:

| emp_no | salary | from_date  | to_date    |
| ------ | ------ | ---------- | ---------- |
| 10001  | 88958  | 2002-06-22 | 9999-01-01 |
| 10002  | 72527  | 2001-08-02 | 9999-01-01 |
| 10003  | 92527  | 2001-08-02 | 9999-01-01 |

获取每个部门中薪水最高的员工相关信息，给出dept_no, emp_no以及其对应的salary

```sql
select
    dept_no,
    s.emp_no,
    salary
from
    dept_emp de
    join salaries s on de.emp_no = s.emp_no
where
    (dept_no, salary) in (
        select
            dept_no,
            max(salary)
        from
            dept_emp de
            join salaries s on de.emp_no = s.emp_no
        group by
            dept_no
    )
order by
    dept_no

```

### WHERE 子句

- 这里使用了一个子查询来筛选每个部门中薪水最高的员工。子查询的结果必须包含在主查询的 WHERE 条件中。
- (dept_no, salary) IN (...) 确保我们只选择那些部门编号和薪水组合在子查询结果中的记录。

**子查询**：

- 子查询同样从 dept_emp 和 salaries 表中获取数据，并通过 JOIN 连接。
- GROUP BY dept_no 对每个部门进行分组。
- MAX(salary) 用于找出每个部门中最高的薪水。

**ORDER BY**：

- 最后，通过 ORDER BY dept_no 对结果按部门编号进行排序。

### IN关键词

检查某值是否在指定的集合中，在这里用于确保主查询的 dept_no 和 salary 组合在子查询的结果中。

### MAX()关键词

注意需要接group by

### AVG()关键词

```sql
select
ti.title , avg(s.salary)
from
titles ti
join
salaries s
on
ti.emp_no  = s.emp_no 
group by
ti.title
order by
avg(s.salary)
```

注意需要接group by

### 聚合函数

#### count() sum() avg() max() min()

### 日期函数

#### YEAR() 函数

返回给定日期或日期时间的年份部分。

```sql
SELECT YEAR('2023-06-15');  -- 返回 2023
SELECT YEAR(CURDATE());      -- 返回当前年份
SELECT YEAR(NOW());          -- 返回当前年份
```

#### MONTH() 函数

返回给定日期或日期时间的月份部分，返回值是1到12之间的整数。

```sql
SELECT MONTH('2023-06-15'); -- 返回 6
SELECT MONTH(CURDATE());    -- 返回当前月份
SELECT MONTH(NOW());        -- 返回当前月份
```

这些函数可以接受各种日期格式的字符串或MySQL的日期类型（如 DATE, DATETIME, TIMESTAMP）作为参数。

如果传入的参数不是有效的日期或时间格式，函数会返回 NULL。

### 字符串操作函数

#### SUBSTRING() 函数

SUBSTRING(str, pos, [len]) 或 SUBSTRING(str FROM pos FOR len)

从字符串中提取子字符串。

- str 是要操作的字符串。
- pos 是起始位置（从1开始，不是从0开始）。
- len 是要提取的长度，可选参数，如果省略，则提取到字符串末尾。

```sql
SELECT SUBSTRING('abcdefg', 1, 2); -- 返回 'ab'
SELECT SUBSTRING('abcdefg', 3); -- 返回 'cdefg'
SELECT SUBSTRING('abcdefg' FROM 2 FOR 3); -- 返回 'bcd' (等同于 SUBSTRING('abcdefg', 2, 3))
```

SUBSTRING() 通常用于数据清洗、提取特定格式的数据片段或进行字符串操作。

#### CONCAT() 函数

CONCAT(str1, str2, ..., strN)

用于将两个或多个字符串连接起来，形成一个新字符串。

```sql
SELECT CONCAT('Hello', ' ', 'World'); -- 返回 'Hello World'
SELECT CONCAT(first_name, ' ', last_name) FROM employees; -- 连接姓和名
SELECT CONCAT(column1, '-', column2) FROM table_name; -- 使用分隔符连接两个列
```

如果任何参数为 NULL，结果将是 NULL，除非使用 CONCAT_WS()（带有分隔符的连接函数），它可以忽略 NULL 值。

CONCAT() 常用于生成格式化的字符串，如全名、地址、或日志消息。

### 通配符

% - 代表零个或多个字符。

- 示例：SELECT * FROM users WHERE name LIKE 'J%' 会匹配所有名字以J开头的记录。

_ - 代表单个字符。

- 示例：SELECT * FROM users WHERE name LIKE '_o_' 会匹配名字是三个字符且中间是'o'的记录。

可以将 % 用在 LIKE 子句中，用于模糊匹配：

- % 可以匹配零个或多个字符。
- % 可以出现在字符串的开头、中间或结尾。

```sql
SELECT * FROM table WHERE field LIKE 'abc%' -- 获取 field 以 "abc" 开头的所有值
SELECT * FROM table WHERE field LIKE '%xyz' -- 获取 field 以 "xyz" 结尾的所有值
SELECT * FROM table WHERE field LIKE '%abc%' -- 获取 field 包含 "abc" 字符串的所有值
```

如果需要匹配实际的 % 字符，需要使用转义字符，如：

```sql
SELECT * FROM table WHERE field LIKE '%\%%' ESCAPE '\'
```

### 单引号  '

主要用于**字符串 **（即字符串常量）的界定。任何需要插入字符串值的地方，都应该使用单引号。

```sql
SELECT * FROM users WHERE name = 'John Doe';
```
## Mybatis
### concat
```sql
SELECT  
    SUM(info.adjusted_quantity) AS adjusted_quantity,    SUM(info.adjusted_amount) AS adjusted_amountFROM  
    order_info infoWHERE  
    info.order_update_time LIKE CONCAT(#{date}, '%')
```
### 时间比较可以直接字符串 2025-01
![image.png](https://raw.githubusercontent.com/GQLiu1001/mytc/master/imgPc/20250301200837.png)
### 动态set
![image.png](https://raw.githubusercontent.com/GQLiu1001/mytc/master/imgPc/20250302213830.png)
### 动态if 
在`<set>`自动拼接`，`

在`where`会自动拼接`and`

为了防止and出错 可以在where后加入 `1=1`