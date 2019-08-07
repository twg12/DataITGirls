# 8월 5일

## Order By (정렬)
```SQL
SELECT *
FROM Customers
WHERE ___
ORDER BY ___
```
* ORDER BY ___ DESC: 거꾸로 정렬한다
* ORDER BY ___, ___
* LEFT/RIGHT(___, n): ___ is value, n is position
    * WHERE LEFT(name, 3) = 'SAM' : SAM으로 시작하는 이름들을 다 찾고 싶다.
* LIMIT n: answer limited to n


## Group By
* WHERE (x), HAVING (ㅇ): GROUP BY 아래 기재할 것
* 순서
    1. GROUP BY
    2. ORDER BY

## Join
