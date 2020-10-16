176. Second Highest Salary

SELECT
    (SELECT DISTINCT Salary
    FROM Employee
    ORDER BY Salary DESC
    LIMIT 1 OFFSET 1) AS SecondHighestSalary;

177. Nth Highest Salary

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

178. Rank Scores

Important Note: For MySQL solutions, to escape **reserved words** used as column names, you can use an apostrophe before and after the keyword. For example `Rank`.

SELECT score, DENSE_RANK() OVER (ORDER BY Score DESC) AS `Rank`
FROM Scores;

180. Consecutive Numbers

标答：use 3 aliases for this table Logs

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

1 1 2 1 3 1
2 1 3 1 4 2
...

181. Employees Earning More Than Their Managers: self join EZ

182. Duplicate Emails: aggregation function count() EZ

183. Customers Who Never Order

  SELECT Name AS Customers
  FROM Customers AS c
  LEFT JOIN Orders AS o
  ON c.Id=o.CustomerId
  WHERE o.CustomerId IS NULL;

  错误：（因为有id不同但同一个名字的情况
  SELECT Name as Customers
  FROM Customers
  WHERE Name NOT IN (
      SELECT Name
      FROM Customers AS c
      INNER JOIN Orders AS o
      ON c.Id=o.CustomerId
      );

184. Department Highest Salary
