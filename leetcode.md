# leetcode数据库题解答
## 简单
#### 596. 超过5名学生的课
有一个courses 表 ，有: student (学生) 和 class (课程)。  
请列出所有超过或等于5名学生的课。  
例如,表:  
+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+

应该输出:

+---------+
| class   |
+---------+
| Math    |
+---------+

Note:  
学生在每个课中不应被重复计算。

- 题解  
```mysql
select class from courses group by class having count(DISTINCT student)>=5;
```

## 中等
## 困难