# 9월 9일

## 섭쿼리
* 값이 하나만 리턴되는 섭쿼리
```sql
SELECT Min(DATETIME)
FROM ANIMAL_INS
```
* 값이 여러개
    * 리스트
    ```sql
    SELECT DATETIME
    FROM INS
    ```
    * 테이블: 컬럼 두개 이상
    -> 아예 다른 테이블로 저장

* LEFT('apple', 3): apple에서 왼쪽에서 세개만 
* SUBSTRING_INDEX(A, ' ' 1): 첫번째 단어만
* UPPER(): 대문자로 바꾼다, LOWER(): 소문자로 바꾼다
* <>: means difference