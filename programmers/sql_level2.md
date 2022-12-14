[문제 링크](https://school.programmers.co.kr/learn/challenges?order=acceptance_desc&languages=mysql%2Coracle&page=1&levels=2)


## 1. 동물 수 구하기
+ COUNT(*)
### 1. ORACLE, MySQL
```sql
SELECT COUNT(*)
FROM ANIMAL_INS
```

## 2. 중복 제거하기
+ IS NOT NULL, DISTINCT()
### 1. ORACLE, MySQL
```sql
SELECT COUNT(DISTINCT NAME)
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
```

## 3. 최솟값 구하기
+ MIN
### 1. ORACLE, MySQL
```sql
SELECT MIN(DATETIME)
FROM ANIMAL_INS
```

## 4. 이름에 el이 들어가는 동물 찾기
+ LOWER, LIKE
### 1. ORACLE, MySQL
```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE LOWER(NAME) LIKE '%el%'
AND LOWER(ANIMAL_TYPE) = 'dog'
ORDER BY NAME
```

## 5. 동명 동물 수 찾기
+ COUNT(), GROUP BY, HAVING
### 1. ORACLE, MySQL
```sql
SELECT NAME, COUNT(NAME) AS COUNT
FROM ANIMAL_INS
GROUP BY NAME
HAVING COUNT(NAME) >= 2
ORDER BY NAME
```

## 6. NULL 처리하기
+ NVL(), NVL2()
### 1. ORACLE - 1
```sql
SELECT ANIMAL_TYPE, NVL(NAME, 'No name') AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
*NVL(expr1, expr2)
- expr1의 값이 NULL일 경우 expr2의 값을 반환하다.

### 2. ORACLE - 2
```sql
SELECT ANIMAL_TYPE, NVL2(NAME,NAME,'No name') AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
*NVL2(expr, expr1, expr2)
- expr의 값이 NULL이 아닐 경우에 expr1의 값을 반환한다.
- expr의 값이 NULL일 경우에 expr2의 값을 반환한다.

### 3. ORACLE, MySQL
```sql
SELECT ANIMAL_TYPE, IFNULL(NAME,'No name') AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

## 7. DATETIME에서 DATE로 형 변환
+ ORACLE: TO_CHAR()
+ MySQL: DATE_FORMAT()
### 1. ORACLE
```sql
SELECT ANIMAL_ID, NAME, TO_CHAR(DATETIME, 'YYYY-MM-DD') AS 날짜
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

### 2. MySQL
```sql
SELECT
    ANIMAL_ID
    , NAME
    , DATE_FORMAT(DATETIME, '%Y-%m-%d') AS 날짜
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
- %Y-%m-%d = 2022-11-12
- %y-%m-%d = 22-11-12

## 8. 고양이와 개는 몇마리 있을까
+ COUNT(), GROUP BY, ORDER BY
### 1. ORACLE, MySQL
```sql
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE)
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE
```

## 9. 중성화 여부 파악하기
+ CASE, LIKE
### 1. ORACLE, MySQL
```sql
SELECT ANIMAL_ID, NAME, 
CASE
    WHEN SEX_UPON_INTAKE LIKE '%Neutered%'
    OR SEX_UPON_INTAKE LIKE '%Spayed%'
    THEN 'O'
    ELSE 'X'
END AS 중성화
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
- CASE문 중 WHEN은 IF문으 대체하고 OR은 ELSE IF문을 대체한다
    - THEN은 WHEN문이 일치할 때 실행할 결과이다.
    - 마지막으로 END문으로 감싸주고, 해당 Column의 이름을 적는다.

## 10. 입양 시각 구하기(1)
+ TO_CHAR(DATETIME, 'HH24'), BETWEEN
### 1. ORACLE - 1
```sql
SELECT TO_CHAR(DATETIME, 'HH24') AS HOUR, COUNT(*) AS COUNT
FROM ANIMAL_OUTS
WHERE TO_CHAR(DATETIME, 'HH24') BETWEEN 9 AND 19
GROUP BY TO_CHAR(DATETIME, 'HH24')
ORDER BY TO_CHAR(DATETIME, 'HH24')
```
- 'HH24'는 24시간 값으로 출력할 수 있다.

### 2. ORACLE - 2
```sql
SELECT HOUR, COUNT(*)
FROM
    (SELECT TO_CHAR(DATETIME, 'HH24') AS HOUR
     FROM ANIMAL_OUTS)
 WHERE HOUR >= 9 AND HOUR <= 20
 GROUP BY HOUR
 ORDER BY HOUR
```

## 11. 가격이 제일 비싼 식품의 정보 출력하기
+ MAX()
### 1. ORACLE, MySQL
```sql
SELECT *
FROM FOOD_PRODUCT
WHERE PRICE IN (SELECT MAX(PRICE)
                FROM FOOD_PRODUCT)
```

## 12. 카테고리 별 상품 개수 출력하기
+ ORACLE: SUBSTR()
+ MySQL: LEFT()
### 1. ORACLE
```sql
SELECT SUBSTR(PRODUCT_CODE, 0, 2) AS CODE, COUNT(PRODUCT_CODE) COUNT
FROM PRODUCT
GROUP BY SUBSTR(PRODUCT_CODE, 0, 2)
ORDER BY SUBSTR(PRODUCT_CODE, 0, 2) ASC
```

### 2. MySQL
```sql
SELECT LEFT(PRODUCT_CODE, 2) AS CODE, COUNT(PRODUCT_CODE) COUNT
FROM PRODUCT
GROUP BY CODE
```

## 13. 루시와 엘라 찾기
+ WHERE - IN ()
### 1. ORACLE
```sql
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
WHERE NAME IN ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty')
ORDER BY ANIMAL_ID
```

## 14. 3월에 태어난 여성 회원 목록 출력하기
+ TO_CHAR, IS NOT NULL
### 1. ORACLE
```sql
SELECT MEMBER_ID, MEMBER_NAME, GENDER, TO_CHAR(DATE_OF_BIRTH, 'yyyy-MM-DD') AS DATE_OF_BIRTH
FROM MEMBER_PROFILE
WHERE TO_CHAR(DATE_OF_BIRTH, 'MM') IN '03'
AND TLNO IS NOT NULL
AND GENDER = 'W'
ORDER BY MEMBER_ID ASC
```

## 15. 진료과별 총 예약 횟수 출력하기
+ TO_CHAR(), GROUP BY
### 1. ORACLE
```sql
SELECT MCDP_CD AS 진료과코드, COUNT(MCDP_CD) AS 예약건수
FROM APPOINTMENT
WHERE TO_CHAR(APNT_YMD, 'YYYY-MM') IN '2022-05'
GROUP BY MCDP_CD
ORDER BY 예약건수 ASC, 진료과코드 ASC
```

## 16. 상품 별 오프라인 매출 구하기
+ SUM()
### 1. ORACLE - 1
```sql
SELECT P.PRODUCT_CODE, SUM(P.PRICE * O.SALES_AMOUNT) AS 매출액
FROM PRODUCT P
INNER JOIN OFFLINE_SALE O
ON P.PRODUCT_ID = O.PRODUCT_ID
GROUP BY P.PRODUCT_CODE
ORDER BY 매출액 DESC, P.PRODUCT_CODE ASC
```

### 2. ORACLE - 2
+ INNER JOIN SUBQUERY
```sql
SELECT P.PRODUCT_CODE, P.PRICE * O.SALES_AMOUNT AS 매출액
FROM PRODUCT P
INNER JOIN (
    SELECT PRODUCT_ID, SUM(SALES_AMOUNT) AS SALES_AMOUNT
    FROM OFFLINE_SALE
    GROUP BY PRODUCT_ID
) O
ON P.PRODUCT_ID = O.PRODUCT_ID
ORDER BY 매출액 DESC, P.PRODUCT_CODE ASC
```
- OFFLINE_SALE 테이블에서 PRODUCT_ID별 SALES_AMOUNT의 합을 구한 데이터를 PRODUCT_ID 기준으로 INNER JOIN한다.
- 그 다음, 가격과 SALES_AMOUNT를 곱한 값을 SALES 컬럼으로 두고 정렬 기준대로 정렬한다.

## 17. 가격대 별 상품 개수 구하기
+ SUBSTR(), FLOOR()
### 1. ORACLE - 1
```sql
SELECT SUBSTR(PRICE/10000, 1, 1) * 10000 AS PRICE_GROUP
     , COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY SUBSTR(PRICE/10000, 1, 1) * 10000
ORDER BY PRICE_GROUP ASC
```

### 2. ORACLE - 2
```sql
SELECT FLOOR(PRICE / 10000) * 10000 AS PRICE_GROUP
     , COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY FLOOR(PRICE / 10000) * 10000
ORDER BY PRICE_GROUP ASC
```

## 18. 성분으로 구분한 아이스크림 총 주문량
+ SUM()
```sql
SELECT I.INGREDIENT_TYPE, SUM(F.TOTAL_ORDER) AS TOTAL_ORDER
FROM FIRST_HALF F
LEFT OUTER JOIN ICECREAM_INFO I
ON F.FLAVOR = I.FLAVOR
GROUP BY I.INGREDIENT_TYPE
ORDER BY TOTAL_ORDER
```

## 19. 재구매가 일어난 상품과 회원 리스트 구하기
+ HAVING
### 1. ORACLE
```sql
SELECT USER_ID, PRODUCT_ID
FROM ONLINE_SALE
GROUP BY USER_ID, PRODUCT_ID
HAVING COUNT(*) >= 2
ORDER BY USER_ID ASC, PRODUCT_ID DESC
```