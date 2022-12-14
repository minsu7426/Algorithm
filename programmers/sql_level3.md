[문제 링크](https://school.programmers.co.kr/learn/challenges?order=acceptance_desc&languages=mysql%2Coracle&page=1&levels=3)


## 1. 오랜 기간 보호한 동물(1)
+ FROM SUBQUERY
### 1. ORACLE
```sql
SELECT NAME, DATETIME
FROM (SELECT A.NAME, A.DATETIME
    FROM ANIMAL_INS A LEFT OUTER JOIN ANIMAL_OUTS B
    ON A.ANIMAL_ID = B.ANIMAL_ID
    WHERE B.ANIMAL_ID IS NULL
    ORDER BY DATETIME ASC
    )
WHERE ROWNUM <= 3
```

## 2. 오랜 기간 보호한 동물(2)
+ DATETIME - DATETIME
### 1. ORACLE
```sql
SELECT ANIMAL_ID, NAME
FROM (
    SELECT I.ANIMAL_ID, I.NAME
    FROM ANIMAL_INS I
    INNER JOIN ANIMAL_OUTS O
    ON I.ANIMAL_ID = O.ANIMAL_ID
    ORDER BY O.DATETIME - I.DATETIME DESC
)
WHERE ROWNUM <= 2
```

## 3. 있었는데요 없었습니다.
### 1. ORACLE
```sql
SELECT I.ANIMAL_ID, I.NAME
FROM ANIMAL_INS I
INNER JOIN ANIMAL_OUTS O
ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE I.DATETIME > O.DATETIME
ORDER BY I.DATETIME ASC
```

## 4. 없어진 기록 찾기
+ LEFT OUTER JOIN, IS NULL
```sql
SELECT O.ANIMAL_ID, O.NAME
FROM ANIMAL_OUTS O
LEFT OUTER JOIN ANIMAL_INS I
ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE I.ANIMAL_ID IS NULL
ORDER BY O.ANIMAL_ID
```

## 5. 조건별로 분류하여 주문상태 출력하기
+ CASE-WHEN-THEN-ELSE-END
### 1. ORACLE
```sql
SELECT ORDER_ID, PRODUCT_ID, TO_CHAR(OUT_DATE, 'YYYY-MM-DD') OUT_DATE
    , CASE
        WHEN OUT_DATE <= TO_DATE('2022-05-01', 'YYYY-MM-DD') THEN '출고완료'
        WHEN OUT_DATE > TO_DATE('2022-05-01', 'YYYY-MM-DD') THEN '출고대기'
        ELSE '출고미정'
    END AS 출고여부
FROM FOOD_ORDER
ORDER BY ORDER_ID ASC
```

## 6. 즐겨찾기가 가장 많은 식당 정보 출력하기
+ WHERE SUBQUERY
### 1. ORACLE
```sql
SELECT FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
FROM REST_INFO
WHERE (FOOD_TYPE, FAVORITES) IN (
    SELECT FOOD_TYPE, MAX(FAVORITES)
    FROM REST_INFO
    GROUP BY FOOD_TYPE)
ORDER BY FOOD_TYPE DESC
```
- 결과문에 REST_ID 형식이 5칸 빈칸으로 0이 채워져서 LPAD(REST_ID, 5, '0') REST_ID를 넣었으나 정답이 아니였다.

## 7. 헤비 유저가 소유한 장소
+ GROUP BY-HAVING
### 1. ORACLE
```sql
SELECT *
FROM PLACES
WHERE HOST_ID IN (SELECT HOST_ID
    FROM PLACES
    GROUP BY HOST_ID
    HAVING 2 <= COUNT(ID)
    )
ORDER BY ID ASC
```