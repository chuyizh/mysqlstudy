# leetcode数据库题解答
## 简单
#### 627. 交换工资

给定一个 salary表，如下所示，有m=男性 和 f=女性的值 。交换所有的 f 和 m 值(例如，将所有 f 值更改为 m，反之亦然)。要求使用一个更新查询，并且没有中间临时表。

例如:

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |

运行你所编写的查询语句之后，将会得到以下表:

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |

- 题解

```mysql
update salary set sex = case sex when "m" then "f" else  "m" end;
```

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

#### 595. 大的国家

这里有张 World 表

| name            | continent  | area       | population   | gdp           |
|-----------------|------------|------------|--------------|---------------|
| Afghanistan     | Asia       | 652230     | 25500100     | 20343000      |
| Albania         | Europe     | 28748      | 2831741      | 12960000      |
| Algeria         | Africa     | 2381741    | 37100000     | 188681000     |
| Andorra         | Europe     | 468        | 78115        | 3712000       |
| Angola          | Africa     | 1246700    | 20609294     | 100990000     |

如果一个国家的面积超过300万平方公里，或者人口超过2500万，那么这个国家就是大国家。

编写一个SQL查询，输出表中所有大国家的名称、人口和面积。

例如，根据上表，我们应该输出:

| name         | population  | area         |
|--------------|-------------|--------------|
| Afghanistan  | 25500100    | 652230       |
| Algeria      | 37100000    | 2381741      |

- 题解

```mysql
select name,population,area from world where population>25000000 or area>3000000
```
#### 620. 有趣的电影

某城市开了一家新的电影院，吸引了很多人过来看电影。该电影院特别注意用户体验，专门有个 LED显示板做电影推荐，上面公布着影评和相关电影描述。

作为该电影院的信息部主管，您需要编写一个 SQL查询，找出所有影片描述为非 boring (不无聊) 的并且 id 为奇数 的影片，结果请按等级 rating 排列。

 

例如，下表 cinema:

|   id    | movie     |  description |  rating   |
|---------|-----------|--------------|-----------|
|   1     | War       |   great 3D   |   8.9     |
|   2     | Science   |   fiction    |   8.5     |
|   3     | irish     |   boring     |   6.2     |
|   4     | Ice song  |   Fantacy    |   8.6     |
|   5     | House card|   Interesting|   9.1     |

对于上面的例子，则正确的输出是为：

|   id    | movie     |  description |  rating   |
|---------|-----------|--------------|-----------|
|   5     | House card|   Interesting|   9.1     |
|   1     | War       |   great 3D   |   8.9     |

- 题解

```mysql
select id,movie,description,rating from cinema where not  description="boring" and id%2!=0 order by rating DESC
```

#### 183. 从不订购的客户

某网站包含两个表，Customers 表和 Orders 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。

Customers 表：

| Id | Name  |
|----|-------|
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |

Orders 表：

| Id | CustomerId |
|----|------------|
| 1  | 3          |
| 2  | 1          |

例如给定上述表格，你的查询应返回：

| Customers |
|-----------|
| Henry     |
| Max       |

- 题解

```mysql
select name as customers from customers where id not in (select customerid from orders )
```
#### 197. 上升的温度

给定一个 Weather 表，编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 Id。

| Id(INT) | RecordDate(DATE) | Temperature(INT) |
|---------|------------------|------------------|
|       1 |       2015-01-01 |               10 |
|       2 |       2015-01-02 |               25 |
|       3 |       2015-01-03 |               20 |

例如，根据上述给定的 Weather 表格，返回如下 Id:

| Id |
|----|
|  2 |
|  4 |

- 题解

```mysql
select Yesterday.id as id  from weather as Yesterday join weather as today where DATEDIFF(yesterday.RecordDate,today.RecordDate)=1 and Yesterday.Temperature>today.Temperature
```
#### 196. 删除重复的电子邮箱

编写一个 SQL 查询，来删除 Person 表中所有重复的电子邮箱，重复的邮箱里只保留 Id 最小 的那个。

| Id | Email            |
|----|------------------|
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
Id 是这个表的主键。

例如，在运行你的查询语句之后，上面的 Person 表应返回以下几行:

| Id | Email            |
|----|------------------|
| 1  | john@example.com |
| 2  | bob@example.com  |

```mysql
delete mins from person as mins join person as maxs where maxs.id<mins.id and maxs.email=mins.email 
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
      select (select distinct salary from employee order by salary DESC limit 1 OFFSET C ) as getNthHighestSalary
  );
END
```
#### 184. 部门工资最高的员工

Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。

| Id | Name  | Salary | DepartmentId |
|----|-------|--------|--------------|
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |

Department 表包含公司所有部门的信息。

| Id | Name     |
|----|----------|
| 1  | IT       |
| 2  | Sales    |

编写一个 SQL 查询，找出每个部门工资最高的员工。例如，根据上述给定的表格，Max 在 IT 部门有最高工资，Henry 在 Sales 部门有最高工资。

| Department | Employee | Salary |
|------------|----------|--------|
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |

- 题解

```mysql
select department.name as department,employee.name as employee ,employee.salary from department join employee on employee.DepartmentId=Department.id and Employee.salary>=(select max(salary) from employee where DepartmentId = department.Id)
```

## 困难
