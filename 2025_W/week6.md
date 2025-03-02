# Sixth Study Week

LV.4 7문제 + LV.5 1문제

#### [문제1~8_링크](https://school.programmers.co.kr/learn/challenges?order=acceptance_desc&languages=mysql&page=1&levels=4%2C5)

## LV.4 ~ Q7, LV.5 Q8

## Q1. 서울에 위치한 식당 목록 출력하기
> 

#### 날짜: 0215

### 문제
REST_INFO와 REST_REVIEW 테이블에서 서울에 위치한 식당들의 식당 ID, 식당 이름, 음식 종류, 즐겨찾기수, 주소, 리뷰 평균 점수를 조회하는 SQL문을 작성해주세요. 이때 리뷰 평균점수는 소수점 세 번째 자리에서 반올림 해주시고 결과는 평균점수를 기준으로 내림차순 정렬해주시고, 평균점수가 같다면 즐겨찾기수를 기준으로 내림차순 정렬해주세요.

### 정답 쿼리

```sql
SELECT
    A.REST_ID,
    REST_NAME,
    FOOD_TYPE,
    FAVORITES,
    ADDRESS,
    ROUND(AVG(REVIEW_SCORE),2) AS SCORE
FROM REST_INFO AS A
JOIN REST_REVIEW AS B
USING (REST_ID)
WHERE ADDRESS LIKE '서울%'
GROUP BY A.REST_ID
ORDER BY
    SCORE DESC,
    FAVORITES DESC;
```

## Q2. 저자 별 카테고리 별 매출액 집계하기
> 

#### 날짜: 0215

### 문제
2022년 1월의 도서 판매 데이터를 기준으로 저자 별, 카테고리 별 매출액(TOTAL_SALES = 판매량 * 판매가) 을 구하여, 저자 ID(AUTHOR_ID), 저자명(AUTHOR_NAME), 카테고리(CATEGORY), 매출액(SALES) 리스트를 출력하는 SQL문을 작성해주세요.   
결과는 저자 ID를 오름차순으로, 저자 ID가 같다면 카테고리를 내림차순 정렬해주세요.

### 정답 쿼리

```sql
SELECT
    AUTHOR_ID,
    AUTHOR_NAME,
    CATEGORY,
    SUM(SALES * PRICE) AS TOTAL_SALES
FROM BOOK AS B
JOIN AUTHOR AS A
USING (AUTHOR_ID)
JOIN BOOK_SALES AS S
USING (BOOK_ID)
WHERE SALES_DATE LIKE '2022-01-%'
GROUP BY A.AUTHOR_ID, CATEGORY
ORDER BY
    A.AUTHOR_ID,
    CATEGORY DESC;
```

## Q3. 년, 월, 성별 별 상품 구매 회원 수 구하기
> DISTINCT

#### 날짜: 0215

### 문제
USER_INFO 테이블과 ONLINE_SALE 테이블에서 년, 월, 성별 별로 상품을 구매한 회원수를 집계하는 SQL문을 작성해주세요. 결과는 년, 월, 성별을 기준으로 오름차순 정렬해주세요. 이때, 성별 정보가 없는 경우 결과에서 제외해주세요.

### 정답 쿼리

COUNT 함수 안에 DISTINCT 넣어야 옳은 답이 되는 문제였음. 어떻게 하면 USER_ID를 중복하여 세지 않도록 할 수 있을까 고민해서 처음엔 GROUP BY에 USER_ID를 넣어보기도 했으나 그렇게 하면 완전히 다 분리되어 버림.

```sql
SELECT
    YEAR(SALES_DATE) AS YEAR,
    MONTH(SALES_DATE) AS MONTH,
    GENDER,
    COUNT(DISTINCT S.USER_ID) AS USERS
FROM USER_INFO AS I
JOIN ONLINE_SALE AS S
USING (USER_ID)
WHERE GENDER IS NOT NULL
GROUP BY YEAR(SALES_DATE), MONTH(SALES_DATE), GENDER
ORDER BY YEAR, MONTH, GENDER;
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
