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
    