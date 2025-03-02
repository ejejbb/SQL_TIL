# Seventh Study Week

LV.4 7문제 + LV.5 1문제

#### [문제1~8_링크](https://school.programmers.co.kr/learn/challenges?order=acceptance_desc&languages=mysql&page=1&levels=4%2C5)

## LV.4 ~ Q7, LV.5 Q8

## Q1. 오프라인/온라인 판매 데이터 통합하기
> UNION

#### 날짜: 0225

### 문제
ONLINE_SALE 테이블과 OFFLINE_SALE 테이블에서 2022년 3월의 오프라인/온라인 상품 판매 데이터의 판매 날짜, 상품ID, 유저ID, 판매량을 출력하는 SQL문을 작성해주세요. OFFLINE_SALE 테이블의 판매 데이터의 USER_ID 값은 NULL 로 표시해주세요. 결과는 판매일을 기준으로 오름차순 정렬해주시고 판매일이 같다면 상품 ID를 기준으로 오름차순, 상품ID까지 같다면 유저 ID를 기준으로 오름차순 정렬해주세요.

### 정답 쿼리

그놈의 DATE_FORMAT.....

```sql
SELECT
    DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE,
    PRODUCT_ID,
    USER_ID,
    SALES_AMOUNT
FROM ONLINE_SALE
WHERE SALES_DATE LIKE '2022-03-%'
UNION
SELECT
    DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE,
    PRODUCT_ID,
    NULL AS USER_ID,
    SALES_AMOUNT
FROM OFFLINE_SALE
WHERE SALES_DATE LIKE '2022-03-%'
ORDER BY SALES_DATE, PRODUCT_ID, USER_ID;
```

## Q2. 입양 시각 구하기(2)
> WITH RECURSIVE, COALESCE

#### 날짜: 0302

### 문제
보호소에서는 몇 시에 입양이 가장 활발하게 일어나는지 알아보려 합니다. 0시부터 23시까지, 각 시간대별로 입양이 몇 건이나 발생했는지 조회하는 SQL문을 작성해주세요. 이때 결과는 시간대 순으로 정렬해야 합니다.

### 정답 쿼리

```SQL
WITH RECURSIVE A AS (
    SELECT 0 AS HOUR
    UNION ALL
    SELECT HOUR + 1
    FROM A
    WHERE HOUR < 23
)
SELECT 
    A.HOUR,
    COALESCE(COUNT(B.DATETIME), 0) AS COUNT
FROM A
LEFT JOIN ANIMAL_OUTS B
ON A.HOUR = HOUR(B.DATETIME)
GROUP BY A.HOUR
ORDER BY A.HOUR;
```

0시부터 23시까지의 시간을 나타나게 하고 싶으나, 원본 테이블에는 7시부터 19시까지의 데이터만 존재하므로 아래와 같이 쿼리를 작성하면 틀린 답으로 나온다.

```sql
SELECT
    HOUR(DATETIME) AS HOUR,
    COUNT(*) AS COUNT
FROM ANIMAL_OUTS
GROUP BY HOUR(DATETIME)
ORDER BY HOUR;
```

### SQL 문법, 함수 정리
> 1. WITH RECURSICE
- **재귀적인 CTE (Common Table Expression)**를 생성하는 SQL 문법
- 자기 자신을 참조하면서 반복적으로 데이터를 생성
```
[위의 쿼리로부터 본 WITH RECURSIVE의 동작 과정]

# 0부터 23까지의 모든 시간 값을 포함하는 가상 테이블을 생성

WITH RECURSIVE A AS (
    SELECT 0 AS HOUR  -- 1️⃣ ANCHOR (0부터 시작)
    UNION ALL
    SELECT HOUR + 1
    FROM A
    WHERE HOUR < 23   -- 2️⃣ Recursive Step (조건을 만족할 때까지 자기 자신을 호출)
)
```
[`WITH RECURSIVE A` 테이블 결과]
| HOUR |
|------|
| 0    |
| 1    |
| 2    |
| ...  |
| 23   |

---

> 2. COALESCE() 함수   
- NULL 값을 다른 값으로 대체할 때 사용하는 함수
- 주어진 값들 중에서 NULL이 아닌 첫 번째 값을 반환

> **🔥 COALESCE vs IFNULL vs CASE**

|함수|설명|
|---|---|
|COALESCE(value1, value2, ...)|여러 개의 값 중 NULL이 아닌 첫 번째 값을 반환|
|IFNULL(value, default_value)|value가 NULL이면 default_value를 반환 (MySQL에서만 지원)|
|CASE WHEN column IS NULL THEN default_value ELSE column END|NULL일 때 특정 값으로 변환하는 CASE 문|

```
[위의 쿼리로부터 본 COALESCE의 역할]

# `B.DATETIME`이 `NULL`이면 `COUNT`도 `NULL`이 됨.
# `COALESCE`는 `NULL` 값을 0으로 바꿔줌

COALESCE(COUNT(B.DATETIME), 0) AS COUNT
```


## Q3. 특정 세대의 대장균 찾기
> 

#### 날짜: 0303

### 문제
3세대의 대장균의 ID(ID) 를 출력하는 SQL 문을 작성해주세요. 이때 결과는 대장균의 ID 에 대해 오름차순 정렬해주세요.

### 정답 쿼리

```sql

```

## Q4. 우유와 요거트가 담긴 장바구니
> GROUP_CONCAT

#### 날짜: 0215

### 문제
데이터 분석 팀에서는 우유(Milk)와 요거트(Yogurt)를 동시에 구입한 장바구니가 있는지 알아보려 합니다. 우유와 요거트를 동시에 구입한 장바구니의 아이디를 조회하는 SQL 문을 작성해주세요. 이때 결과는 장바구니의 아이디 순으로 나와야 합니다.

### 정답 쿼리

```sql
WITH A AS (
    SELECT
        CART_ID,
        GROUP_CONCAT(NAME SEPARATOR ', ') AS NAME
    FROM CART_PRODUCTS
    GROUP BY CART_ID
    )

SELECT
    CART_ID
FROM A
WHERE NAME LIKE '%Milk%'
    AND NAME LIKE '%Yogurt%'
ORDER BY CART_ID;
```

### 문제 풀이 과정
![6.1](/2025_W/img/6-1.PNG)

GROUP BY를 했을 때 하나의 값만 남기고 나머지 값들은 삭제된 것을 확인할 수 있음.

✅ GROUP BY를 했을 때 서로 다른 값들을 하나의 행에 합쳐서 표시하는 방법: `GROUP_CONCAT()`   
→ **하나의 그룹 내에서 여러 값을 쉼표(또는 다른 구분자)로 합쳐서 하나의 문자열로 출력**할 수 있다.


[예제]

같은 `category` 그룹의 `product_name`들을 `,`로 연결하여 하나의 행에 출력

```sql
SELECT 
    category, 
    GROUP_CONCAT(product_name SEPARATOR ', ') AS products
FROM food_product
GROUP BY category;
```
[출력 결과]
| category | products |
|----------|---------|
| 과자      | 초코파이, 새우깡, 감자칩 |
| 김치      | 배추김치, 깍두기, 총각김치 |
| 음료      | 콜라, 사이다, 오렌지주스 |


![6.2](/2025_W/img/6-2.PNG)


## Q5. 주문량이 많은 아이스크림들 조회하기
> 

#### 날짜: 0215

### 문제
7월 아이스크림 총 주문량과 상반기의 아이스크림 총 주문량을 더한 값이 큰 순서대로 상위 3개의 맛을 조회하는 SQL 문을 작성해주세요.

### 정답 쿼리

JULY 테이블에 FLAVOR이 중복되므로 먼저 그룹화 해준 뒤 JOIN

```sql
SELECT
    FLAVOR
FROM FIRST_HALF AS A
JOIN
    (SELECT
        FLAVOR,
        SUM(TOTAL_ORDER) AS TOTAL_ORDER
    FROM JULY
    GROUP BY FLAVOR) AS B
USING (FLAVOR)
GROUP BY FLAVOR
ORDER BY (A.TOTAL_ORDER + B.TOTAL_ORDER) DESC
LIMIT 3;
```


## Q6. 연간 평가점수에 해당하는 평가 등급 및 성과금 조회하기
> 

#### 날짜: 0215

### 문제
HR_DEPARTMENT, HR_EMPLOYEES, HR_GRADE 테이블을 이용해 사원별 성과금 정보를 조회하려합니다. 평가 점수별 등급과 등급에 따른 성과금 정보가 아래와 같을 때, 사번, 성명, 평가 등급, 성과금을 조회하는 SQL문을 작성해주세요.

평가등급의 컬럼명은 GRADE로, 성과금의 컬럼명은 BONUS로 해주세요.   
결과는 사번 기준으로 오름차순 정렬해주세요.

### 정답 쿼리

```sql
SELECT
    E.EMP_NO,
    EMP_NAME,
    CASE
        WHEN SCORE >= 96 THEN 'S'
        WHEN SCORE >= 90 THEN 'A'
        WHEN SCORE >= 80 THEN 'B'
        ELSE 'C'
    END AS GRADE,
    CASE
        WHEN SCORE >= 96 THEN (SAL*0.2)
        WHEN SCORE >= 90 THEN (SAL*0.15)
        WHEN SCORE >= 80 THEN (SAL*0.1)
        ELSE 0
    END AS BONUS
FROM HR_EMPLOYEES AS E
JOIN (
    SELECT
        EMP_NO,
        AVG(SCORE) AS SCORE
    FROM HR_GRADE
    GROUP BY EMP_NO) AS G
USING (EMP_NO)
ORDER BY E.EMP_NO;
```


## Q7. 그룹별 조건에 맞는 식당 목록 출력하기
> OVER

#### 날짜: 0215

### 문제
MEMBER_PROFILE와 REST_REVIEW 테이블에서 리뷰를 가장 많이 작성한 회원의 리뷰들을 조회하는 SQL문을 작성해주세요. 회원 이름, 리뷰 텍스트, 리뷰 작성일이 출력되도록 작성해주시고, 결과는 리뷰 작성일을 기준으로 오름차순, 리뷰 작성일이 같다면 리뷰 텍스트를 기준으로 오름차순 정렬해주세요.

### 정답 쿼리

```sql
WITH R AS (
    SELECT
        MEMBER_ID,
        REVIEW_TEXT,
        REVIEW_DATE,
        COUNT(*) OVER (PARTITION BY MEMBER_ID) AS CNT
    FROM REST_REVIEW
    )

SELECT
    MEMBER_NAME,
    REVIEW_TEXT,
    DATE_FORMAT(REVIEW_DATE, '%Y-%m-%d') AS REVIEW_DATE
FROM MEMBER_PROFILE AS M
JOIN R
USING (MEMBER_ID)
WHERE CNT = (
    SELECT MAX(CNT)
    FROM R)
ORDER BY REVIEW_DATE, REVIEW_TEXT;
```

[출력 결과]   
![6.3.](/2025_W/img/6-3.PNG)


## Q8. 상품을 구매한 회원 비율 구하기
> 

#### 날짜: 0215

### 문제
USER_INFO 테이블과 ONLINE_SALE 테이블에서 2021년에 가입한 전체 회원들 중 상품을 구매한 회원수와 상품을 구매한 회원의 비율(=2021년에 가입한 회원 중 상품을 구매한 회원수 / 2021년에 가입한 전체 회원 수)을 년, 월 별로 출력하는 SQL문을 작성해주세요. 상품을 구매한 회원의 비율은 소수점 두번째자리에서 반올림하고, 전체 결과는 년을 기준으로 오름차순 정렬해주시고 년이 같다면 월을 기준으로 오름차순 정렬해주세요.

### 정답 쿼리

```sql
WITH A AS (
    SELECT
        *
    FROM USER_INFO
    WHERE YEAR(JOINED)=2021)

SELECT
    YEAR(SALES_DATE) AS YEAR,
    MONTH(SALES_DATE) AS MONTH,
    COUNT(DISTINCT USER_ID) AS PURCHASED_USERS,
    ROUND(COUNT(DISTINCT USER_ID)/(SELECT COUNT(DISTINCT USER_ID) FROM A),1) AS PUCHASED_RATIO
FROM ONLINE_SALE AS B
WHERE USER_ID IN (SELECT USER_ID FROM A)
GROUP BY MONTH(SALES_DATE)
ORDER BY YEAR, MONTH;
```
