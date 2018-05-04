Leetcode 数据库板块，不收费的18道SQL练习题。
### 175	Combine Two Tables   ###
```sql
# Write your MySQL query statement below.
SELECT
	FirstName,
	LastName,
	City,
	State
FROM
	Person
LEFT OUTER JOIN Address ON Person.PersonId = Address.PersonId;
```

###  176	Second Highest Salary  ###
```
# Write your MySQL query statement below.
SELECT
	IFNULL(
		(
			SELECT DISTINCT
				Salary
			FROM
				Employee
			ORDER BY
				Salary DESC
			LIMIT 1 OFFSET 1
		),
		NULL
	) AS SecondHighestSalary;
```
- 使用了 [`IFNULL(expr1, expr2)`](https://dev.mysql.com/doc/refman/5.7/en/control-flow-functions.html#function_ifnull) 函数，将空值替换为NULL

###  177	Nth Highest Salary  ###
```
CREATE FUNCTION getNthHighestSalary (N INT) RETURNS INT
BEGIN

DECLARE oset INT;
SET oset = N - 1;

RETURN (
	# Write your MySQL query statement below.
	SELECT
		IFNULL(
			(
				SELECT DISTINCT
					Salary
				FROM
					Employee
				ORDER BY
					Salary DESC
				LIMIT oset, 1
			),
			NULL
		)
);


END
```
- limit后不能直接写N-1，因为limit后面不能接表达式；

### 178	Rank Scores ###
方法一：
```
# Write your MySQL query statement below
SELECT
	Score,
	(
		SELECT
			count(*)
		FROM
			(
				SELECT DISTINCT
					Score
				FROM
					Scores
			) tmp
		WHERE
			tmp.Score >= Score
	) As Rank
FROM
	Scores
ORDER BY
	Score DESC
```
- 对每一个Score，找出>=它的score的数量，这个数量就是Rank；


方法二：
```
# Write your MySQL query statement below
SELECT
	Scores.Score AS Score,
	COUNT(*) AS Rank
FROM
	Scores
JOIN (
	SELECT DISTINCT
		Score
	FROM
		Scores
) Ranking ON Scores.Score <= Ranking.Score
GROUP BY
	Scores.Id
ORDER BY
	Scores.Score DESC;
```

- 求得每个Score与大于等于该Score的内连接表，再按照id分组，统计即可；


### 180. Consecutive Numbers ###
```
# Write your MySQL query statement below
SELECT DISTINCT
	log1.Num AS ConsecutiveNums
FROM
	LOGS log1,
	LOGS log2,
	LOGS log3
WHERE
	log1.Id = log2.Id - 1
AND log2.Id = log3.Id - 1
AND log1.Num = log2.Num
AND log2.Num = log3.Num;
```
- 自联结查询
- `SELECT DISTINCT log1.Num` 中的 `log1` 可替换为 `log2`, `log3`；
- 类似题目（更复杂）：[601. Human Traffic of Stadium](https://leetcode.com/problems/human-traffic-of-stadium/description/)

### 181	Employees Earning More Than Their Managers    ###
```
# Write your MySQL query statement below
SELECT
	a.`Name` AS Employee
FROM
	Employee AS a
INNER JOIN Employee AS b ON a.ManagerId = b.Id
AND a.Salary > b.Salary;
```

### 182. Duplicate Emails ###
方式一：
```
# Write your MySQL query statement below
SELECT
	Email
FROM
	Person
GROUP BY
	Email
HAVING
	count(Email) > 1
```

方式二：
```
# Write your MySQL query statement below
SELECT DISTINCT
	a.Email
FROM
	Person AS a
INNER JOIN Person AS b ON a.Id != b.Id
AND a.Email = b.Email;
```

### 183. Customers Who Never Order ###
```
# Write your MySQL query statement below
SELECT
	Customers. NAME AS 'Customers'
FROM
	Customers
WHERE
	Customers.Id NOT IN (
		SELECT DISTINCT
			CustomerId
		FROM
			Orders
	);
```

### 184	Department Highest Salary    ###

```
SELECT
	D.Name AS Department,
	E.Name AS Employee,
	E.Salary AS Salary
FROM
	Employee E
INNER JOIN Department D ON E.DepartmentId = D.Id
AND (E.DepartmentId, E.Salary) IN (
	SELECT
		DepartmentId,
		MAX(Salary) AS max
	FROM
		Employee
	GROUP BY
		DepartmentId
);
```
- 将Employee表按照DepartmentId分组，再将各组中Salary最高的对应Id和该Salary组成一个集合，即各Department中最高Salary的集合；
再逐一判断employee的id和Salary是否在该集合中即可；

### 	185	Department Top Three Salaries    ###
```
# Write your MySQL query statement below
SELECT
	d. NAME AS 'Department',
	e1. NAME AS 'Employee',
	e1.Salary
FROM
	Employee e1
INNER JOIN Department d ON e1.DepartmentId = d.Id

WHERE
	(
		SELECT
			COUNT(DISTINCT e2.Salary)
		FROM
			Employee e2
		WHERE
			e2.Salary > e1.Salary
		AND e1.DepartmentId = e2.DepartmentId
	) < 3
ORDER BY
	Department;
```
- 工资前三名的员工，可理解为：在该部门中比这三名员工中任一位工资高的人不超过3人，据此便可作为WHERE条件，加上 DepartmentId 连接，便可找出一个 Department 中工资前三的员工记录，最后再与 Department 表内连接，列出部门名称即可；

### 	196	Delete Duplicate Emails    ###
```
# Write your MySQL query statement below
DELETE p1
FROM
	Person p1,
	Person p2
WHERE
	p1.Email = p2.Email
AND p1.Id > p2.Id;
```

### 197. Rising Temperature ###
```
# Write your MySQL query statement below
SELECT
	w1.Id
FROM
	Weather w1
INNER JOIN Weather w2 ON DATEDIFF(w1.Date, w2.Date) = 1
AND w1.Temperature > w2.Temperature;
```
- 使用 [`DATEDIFF(expr1, expr2)`](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_datediff "DATEDIFF") 函数


### 262. Trips and Users ###
```
# Write your MySQL query statement below
SELECT
	t.Request_at AS `Day`,
	ROUND(
		SUM(IF (t.Status = 'completed', 0, 1)) / COUNT(*), 2
	) AS `Cancellation Rate`
FROM
	Trips t
INNER JOIN Users u ON t.Client_Id = u.Users_Id
WHERE
	t.Request_at BETWEEN '2013-10-01'
AND '2013-10-03'
AND u.Banned = 'No'
AND u.Role = 'client'
GROUP BY
	t.Request_at
ORDER BY
	t.Request_at;
```

 - 先筛选出符合条件的 trip 记录，然后按日期进行分组，再分别计算各组的 Cancellation Rate；
 - 计算 Cancellation Rate 时，使用 [`IF(expr1,expr2,expr3)`](https://dev.mysql.com/doc/refman/5.7/en/control-flow-functions.html#function_if) 函数、`ROUND()` 函数、`sum()` 函数；



###  595	Big Countries ###
```
# Write your MySQL query statement below
SELECT
	NAME,
	population,
	area
FROM
	World
WHERE
	area > 3000000
OR population > 25000000;


```


###	596	Classes More Than 5 Students ###
```
# Write your MySQL query statement below
SELECT
	class
FROM
	courses
GROUP BY
	class
HAVING
	COUNT(DISTINCT student) >= 5;
```
- `WHERE` 子句用于筛选行记录，筛选分组要使用 `HAVING` 子句；


### 601 Human Traffic of Stadium ###
```
# Write your MySQL query statement below
SELECT DISTINCT
	t1.*
FROM
	stadium t1,
	stadium t2,
	stadium t3
WHERE
	t1.people >= 100
AND t2.people >= 100
AND t3.people >= 100
AND (
	(
		t3.id - t2.id = 1
		AND t2.id - t1.id = 1
		AND t3.id - t1.id = 2
	) -- id: t1 < t2 < t3
	OR (
		t2.id - t1.id = 1
		AND t2.id - t3.id = 2
		AND t1.id - t3.id = 1
	) -- id: t3 < t1 < t2
	OR (
		t1.id - t2.id = 1
		AND t1.id - t3.id = 2
		AND t2.id - t3.id = 1
	) -- id: t3 < t2 < t1
)
ORDER BY
	t1.id;
```
- 自联结查询   
- 由于需要列出匹配的所有记录，因此此处的t1需能匹配到第一天、第二天、第三天……直到连续最后一天的所有情况；
-   若 `SELECT DISTINCT t1.*` 中的 `t1` 替换成 `tx`，则 WHERE 子句中的条件需要相应地做出改变；
- 类似题目（简单）：[180. Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/description/)


### 620	Not Boring Movies ###
```
# Write your MySQL query statement below
SELECT
	*
FROM
	cinema
WHERE
	MOD (id, 2) != 0
AND description != 'boring'
ORDER BY
	rating DESC;
```


### 627	Swap Salary ###
```
# Write your MySQL query statement below
UPDATE salary
SET
    sex = CASE sex
        WHEN 'm' THEN 'f'
        ELSE 'm'
    END;
```
- 使用 [`CASE WHEN`](https://dev.mysql.com/doc/refman/5.7/en/control-flow-functions.html#operator_case) 流程控制


