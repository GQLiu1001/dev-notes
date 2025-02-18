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

### 去重

加distinct去重，去除多余的日期条数，比如有3个1月2 使用limit之后还是1月2 如果加了distinct 就只会留下一个1月2 达到搜索

hire_date 倒数第三天（使用limit x y实现）的日子 作为 子查询的条件 desc升序排列

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
