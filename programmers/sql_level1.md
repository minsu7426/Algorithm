[문제 링크](https://school.programmers.co.kr/learn/challenges?tab=all_challenges&order=acceptance_desc&page=1&languages=mysql&levels=1)


## 1. 아픈 동물 찾기
+ SELECT
```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION = 'Sick'
ORDER BY animal_id ASC
```

## 2. 동물의 아이디와 이름
+ SELECT
```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID ASC
```

## 3. 어린 동물 찾기
+ SELECT
```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION != ('Aged')
ORDER BY ANIMAL_ID ASC
```
+ != 는 <>로 써주는것이 더 좋다.

## 4. 여러 기준으로 정렬하기
+ SELECT
```sql
SELECT ANIMAL_ID, NAME, DATETIME
FROM ANIMAL_INS
ORDER BY NAME ASC, DATETIME DESC
```

## 5. 상위 n개 레코드
+ SELECT
1. MySQL
```sql
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1
```
2. ORACLE
```sql
SELECT NAME
FROM (
    SELECT NAME
    FROM ANIMAL_INS
    ORDER BY DATETIME
    )
WHERE ROWNUM <= 1
```
1. 쿼리가 실행되는 순서
- FROM => WHERE => GROUP BY => SELECT => ORDER BY

## 6. 이름이 있는 동물의 아이디
- IS NULL
```sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
ORDER BY ANIMAL_ID ASC
```

## 7. 역순 정렬하기
- SELECT
```sql
SELECT NAME, DATETIME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID DESC
```

## 8. 나이 정보가 없는 회원 수 구하기
- IS NULL
```sql
SELECT COUNT(*) AS USERS
FROM USER_INFO
WHERE AGE IS NULL
```

## 9. 강원도에 위치한 생산공장 목록 출력하기
- SELECT
```sql
SELECT FACTORY_ID, FACTORY_NAME, ADDRESS
FROM FOOD_FACTORY
WHERE ADDRESS LIKE '강원도%'
ORDER BY FACTORY_ID ASC
```

## 10. 경기도에 위치한 식품창고 목록 출력하기
- IS NULL
1. MySQL
```sql
SELECT WAREHOUSE_ID, WAREHOUSE_NAME, ADDRESS, IF(FREEZER_YN IS NULL, 'N', FREEZER_YN) AS FREEZER_YN
FROM FOOD_WAREHOUSE
WHERE ADDRESS LIKE '경기%'
ORDER BY WAREHOUSE_ID ASC
```
2. ORACLE, MySQL
```sql
SELECT WAREHOUSE_ID, WAREHOUSE_NAME, ADDRESS,
    CASE
    WHEN FREEZER_YN IS NULL THEN 'N'
    ELSE FREEZER_YN
    END AS FREEZER_YN
FROM FOOD_WAREHOUSE
WHERE ADDRESS LIKE '경기%'
ORDER BY WAREHOUSE_ID ASC
```

## 11. 이름이 없는 동물의 아이디
- IS NULL
```sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NULL
ORDER BY ANIMAL_ID ASC
```

## 12. 흉부외과 또는 일반외과 의사 목록 출력하기
- SELECT
1. ORACLE
```sql
SELECT DR_NAME, DR_ID, MCDP_CD, TO_CHAR(HIRE_YMD, 'YYYY-MM-DD') AS HIRE_YMD
FROM DOCTOR
WHERE MCDP_CD IN ('CS', 'GS')
ORDER BY HIRE_YMD DESC, DR_NAME ASC
```
2. MySQL
```sql
SELECT DR_NAME, DR_ID, MCDP_CD, DATE_FORMAT(HIRE_YMD, '%Y-%m-%d') AS HIRE_YMD
FROM DOCTOR
WHERE MCDP_CD IN ('CS', 'GS')
ORDER BY HIRE_YMD DESC, DR_NAME ASC
```

## 13. 가장 비싼 상품 구하기
- SUM, MAX, MIN
```sql
SELECT MAX(PRICE) AS MAX_PRICE
FROM PRODUCT
```

## 14. 조건에 맞는 회원수 구하기
- SELECT
```sql

```


---
---
---
---
---
---
---

## 1. 최솟값 구하기
---
```sql
SELECT datetime as '시간' from animal_ins order by datetime limit 1;
```
<Br>

## 2. 동물 수 구하기
---
```sql
SELECT count(*) from animal_ins;
```
<Br>

## 3. 중복 제거하기
---
```sql
SELECT count(distinct name) as 'count' from animal_ins;
```
__집계함수 count(열이름)에서 특별히 따로 group by를 같은 조건이 없다면 해당 열의 값이 null인 경우 카운트하지 않아 null 자체가 조회 행에 없다.__  
distinct로 중복되는 것을 하나로 처리  
<Br>

## 4. 고양이와 개는 몇 마리 있을까
---
```sql
SELECT animal_type, count(*) as count from animal_ins group by animal_type order by animal_type;
```
group by로 묶었으므로 count도 자동으로 animal_type으로 묶여서 카운트 된다.
<Br>

## 5. NULL 처리하기
---
```sql
-- 방법 1
-- 첫파라미터가 널이면 2번 파라미터 출력, 첫파라미터가 널이 아니면 첫 파라미터 출력
SELECT animal_type, ifnull(name,'No name') as name, sex_upon_intake from animal_ins;

-- 방법 2
SELECT animal_type, if(name is null,'No name',name) as name, sex_upon_intake from animal_ins;
```
id가 pk라 알아서 id순 정렬된다.
<Br>

## 6. 동명 동물 수 찾기
---
```sql
SELECT name, count(*) as count from animal_ins 
where name is not null 
group by name 
having count(name)>1
order by name;
```
+ where 절에는 집계함수 사용 불가 -> group by 뒤에 having 문 사용  
+ 집계함수 : avg, min, max, count, stdev, var_samp

<Br>

## 7. 입양 시각 구하기1
---
```sql
select hour(datetime) hour, count(*) count
from animal_outs 
where 9<=hour(datetime) and hour(datetime)<=19
group by hour
order by hour
```
+ hour은 집계함수고 그냥 내장 함수다. -> where 조건으로 사용 가능  
+ group by로 묶인 순간부터 count는 묶인 행을 기준으로 카운트한다.
+ __select 문의 별칭은 order by, group by, having에서만 사용 가능하다.__

<Br>

## 8. 루시와 엘라 찾기
---
```sql
SELECT animal_id,name,sex_upon_intake 
from animal_ins
where name in ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty');
```
<Br>

## 9. 이름에 el이 들어가는 동물 찾기
---
```sql
SELECT animal_id,name 
from animal_ins
where name like '%el%' and animal_type ='dog'
order by name;
```
<Br>

## 10. 중성화 여부 파악하기
---
```sql
-- if 문
SELECT animal_id,name,
if (sex_upon_intake like '%Neutered%' or sex_upon_intake like '%Spayed%','O','X') '중성화'
from animal_ins;

-- case 문
SELECT animal_id,name,
case   
    when SEX_UPON_INTAKE like '%Neutered%' then 'O'
    when SEX_UPON_INTAKE like '%Spayed%' then 'O'
    else 'X'
end '중성화'
from animal_ins;
```
<Br>

## 11. DATETIME에서 DATE로 형 변환
---
```sql
SELECT animal_id,name,date_format(datetime,'%Y-%m-%d') '날짜'
from animal_ins;
```
+ date_format 형식
  + %Y(4자리 연도) ex) 2014
  + %y(2자리 연도) ex) 2014 -> 14
  + %M(영어로) ex) 9월 -> September
  + %m(2자리 월) ex) 9월 -> 09
  + %D(th) ex) 2일 -> 2nd
  + %d(2자리 일) ex) 2일 -> 02
  + %H(24시간)
  + %h(12시간)
  + %i(2자리 분) 대문자 I는 %h와 동일
  + %s(초) 대소문자 무관


<br>

10번 11번 다시 풀어볼 필요가 있다.