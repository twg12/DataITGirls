# 8월 7일

## JOIN
* INNER JOIN
    * Used to join different tables into one
    * INNER JOIN table_1 ON table_1.value = table_2.value
* LEFT JOIN
    * Used along with 'IS NULL'
    * To show exceptions to the rule - those that are not shown

## Using representatives
```sql
SELECT * 
FROM table_1
    INNER JOIN A ON A.value = B.value
```

## Alias
* You can store the same value in different aliases
```sql
SELECT 
    w2.id AS 'Id'
FROM weather AS w1, weather AS w2
WHERE w2.recordDate = Date_ADD(w1.RecordDate, Interval 1 Day)
AND w1.temperature < w2.temperature
```

## Subquery
* A query within a query
```sql
SELECT D.Name AS Department
    , E.Name AS Employee
    , E.Salary
FROM Employee AS E
    INNER JOIN Department AS D ON E.DepartmentId = D.Id
WHERE (E.DepartmentId, Salary) IN
    (SELECT DepartmentId
        , MAX(Salary)
    FROM Employee
    GROUP BY DepartmentId)
```
* In this case, the query to Group Department Id and Max Salary is within the query to in the query to find the Employee name.
* One is looking for the *employees* within the department(groupby) who have the highest salary, not the department with the highest salary itself.

