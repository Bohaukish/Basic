```https://leetcode.com/problemset/database/```

176. Second Highest Salary
```
SELECT
    (SELECT DISTINCT Salary
    FROM Employee
    ORDER BY Salary DESC
    LIMIT 1 OFFSET 1) AS SecondHighestSalary;
  ```

177. Nth Highest Salary
```
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  SET N=N-1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT Salary
      FROM Employee
      ORDER BY Salary DESC
      LIMIT 1 OFFSET N
  );
END
  ```
178. Rank Scores

Important Note: For MySQL solutions, to escape **reserved words** used as column names, you can use an apostrophe before and after the keyword. For example \`Rank`.
```
SELECT score, 
       DENSE_RANK() OVER (ORDER BY Score DESC) AS `Rank`
FROM Scores;
  ```

180. Consecutive Numbers

标答：self join, use 3 aliases for this table Logs
```
SELECT DISTINCT l1.Num AS ConsecutiveNums
FROM
    Logs AS l1,
    Logs AS l2,  
    Logs AS l3
WHERE
    l1.Id = l2.Id - 1
    AND l2.Id = l3.Id - 1
    AND l1.Num = l2.Num
    AND l2.Num = l3.Num
;

# 1 1 2 1 3 1
# 2 1 3 1 4 2
# ...
```

181. Employees Earning More Than Their Managers: self join EZ

182. Duplicate Emails: aggregation func COUNT() EZ

183. Customers Who Never Order
  ```
  SELECT Name AS Customers
  FROM Customers AS c
  LEFT JOIN Orders AS o
  ON c.Id=o.CustomerId
  WHERE o.CustomerId IS NULL;
  ```
错误：（因为有id不同但同一个名字的情况
  ```
  SELECT Name as Customers
  FROM Customers
  WHERE Name NOT IN (
      SELECT Name
      FROM Customers AS c
      INNER JOIN Orders AS o
      ON c.Id=o.CustomerId
      );
  ```

184. Department Highest Salary

先融再选
  ```
SELECT Department, Employee, Salary
FROM
(
    SELECT
        d.Name AS Department,
        e.Name AS Employee,
        e.Salary AS Salary,
        DENSE_RANK() OVER (
            PARTITION BY DepartmentId
            ORDER BY Salary DESC) AS SalaryRank
    FROM Employee AS e
    JOIN Department AS d
    ON e.DepartmentId=d.Id
) AS newTable  # Every derived table must have its own alias!
WHERE SalaryRank='1';
  ```

185. Department Top Three Salaries

同上，最后一行改成 WHERE SalaryRank<=3

196. Delete Duplicate Emails

  ```
SELECT *
  FROM Person p1, Person p2
  WHERE p1.Email = p2.Email;
  ```
Id  | Email | Id | Email
------------- | ------------- | -------------| -------------
1  | john@example.com| 1 | john@example.com
3  | john@example.com| 1 | john@example.com
2  | bob@example.com | 2 | bob@example.com
1  | john@example.com | 3 | john@example.com
3  | john@example.com | 2 | john@example.com

```
SELECT *
FROM Person p1, Person p2
WHERE p1.Email = p2.Email AND p1.Id>p2.Id;
  ```
Id  | Email | Id | Email
------------- | ------------- | -------------| -------------
3  | john@example.com| 1 | john@example.com

```
DELETE p1.*
FROM Person p1, Person p2
WHERE p1.Email = p2.Email AND p1.Id>p2.Id;
  ```
删掉的是：
Id  | Email
------------- | -------------
3  | john@example.com|


197. Rising Temperature
     
DATEDIFF(date1,date2):返回起始时间 date1 和结束时间 date2 之间的天数
```
SELECT w1.id AS ID
FROM Weather AS w1, Weather AS w2
WHERE DATEDIFF(w1.recordDate, w2.recordDate)=1
AND w1.Temperature>w2.Temperature;
  ```

262. Trips and Users
     
     题干好长明天写

263. Big Countries
     
  EZ

