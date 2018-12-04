# leetcode数据库题解答
## 简单
#### 596. 超过5名学生的课

有一个courses 表 ，有: student (学生) 和 class (课程)。

请列出所有超过或等于5名学生的课。

例如,表:

| student | class      |
|---------|------------|
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |


应该输出:

| class   |
|---------|
| Math    |


Note:学生在每个课中不应被重复计算。

- 题解

```mysql
select class from courses group by class having count(DISTINCT student)>=5;
```
#### 182. 查找重复的电子邮箱

编写一个 SQL 查询，查找 Person 表中所有重复的电子邮箱。

示例：

| Id | Email   |
|----|---------|
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |

根据以上输入，你的查询应返回以下结果：

| Email   |
|---------|
| a@b.com |

说明：所有电子邮箱都是小写字母。

- 题解

```mysql
select Email from  Person group by Email having count(Email)>1
```
#### 175. 组合两个表

表1: Person

| 列名         | 类型     |
|-------------|---------|
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |

PersonId 是上表主键

表2: Address

| 列名         | 类型    |
|-------------|---------|
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |

AddressId 是上表主键

编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：

`FirstName, LastName, City, State`

- 题解

```mysql
SELECT Person.Firstname,Person.Lastname,Address.City,Address.State from Person LEFT join Address on Person.Personid=Address.Personid
```

#### 176. 第二高的薪水

编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary） 。

| Id | Salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。

| SecondHighestSalary |
|---------------------|
| 200                 |

- 题解

```mysql
select (SELECT distinct salary  from employee order by  salary desc  limit 1,1) as SecondHighestSalary
```
#### 181. 超过经理收入的员工

Employee 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。

| Id | Name  | Salary | ManagerId |
|----|-------|--------|-----------|
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |

给定 Employee 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

| Employee |
|----------|
| Joe      |

- 题解

```mysql
select e.Name as Employee from Employee as e ,Employee as m where e.ManagerId=m.Id AND e.Salary>m.Salary
```
## 中等

#### 177. 第N高的薪水

编写一个 SQL 查询，获取 Employee 表中第 n 高的薪水（Salary）。

| Id | Salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

例如上述 Employee 表，n = 2 时，应返回第二高的薪水 200。如果不存在第 n 高的薪水，那么查询应返回 null。

| getNthHighestSalary(2) |
|------------------------|
| 200                    |

- 题解

```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN DECLARE c INT ;
SELECT (N-1) INTO c;
  RETURN (
      # Write your MySQL query statement below.
      select (select distinct salary from employee order by salary  DESC limit 1 OFFSET C ) as getNthHighestSalary
  );
END
```


## 困难
