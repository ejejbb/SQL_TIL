# Fifth Study Week

LV.3 클리어 + LV.4 2문제

#### [문제1~13_링크](https://school.programmers.co.kr/learn/challenges?order=acceptance_desc&languages=mysql&page=1&levels=3)

## LV.2 ~ Q13

⭐⭐ 문제 다시 풀어보기

## Q1. 카테고리 별 도서 판매량 집계하기
> SUM, GROUP BY, LIKE

#### 날짜: 0207

### 문제
2022년 1월의 카테고리 별 도서 판매량을 합산하고, 카테고리(CATEGORY), 총 판매량(TOTAL_SALES) 리스트를 출력하는 SQL문을 작성해주세요.
결과는 카테고리명을 기준으로 오름차순 정렬해주세요.

### 정답 쿼리

```sql
SELECT
    CATEGORY,
    SUM(SALES) AS TOTAL_SALES
FROM BOOK AS A
JOIN BOOK_SALES AS B
ON A.BOOK_ID = B.BOOK_ID
WHERE SALES_DATE LIKE '2022-01-%'
GROUP BY CATEGORY
ORDER BY CATEGORY;
```


## Q2. 오랜 기간 보호한 동물(2)
> WITH, DATEDIFF, LIMIT

#### 날짜: 0207

### 문제
입양을 간 동물 중, 보호 기간이 가장 길었던 동물 두 마리의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 기간이 긴 순으로 조회해야 합니다.

### 정답 쿼리
여기서는 보호 기간이 길었던 동물 순으로 조회하기 위해 DATEDIFF()만 사용했지만 만일 보호기간도 출력해야 한다면 'DATEDIFF+1'을 해야겠죠?

```sql
WITH C AS(
    SELECT
        B.ANIMAL_ID,
        B.NAME,
        DATEDIFF(B.DATETIME, A.DATETIME) AS PERIOD
    FROM ANIMAL_INS AS A
    JOIN ANIMAL_OUTS AS B
    ON A.ANIMAL_ID = B.ANIMAL_ID
)

SELECT
    ANIMAL_ID,
    NAME
FROM C
ORDER BY PERIOD DESC
LIMIT 2;
```


## Q3. 대여 기록이 존재하는 자동차 리스트 구하기
> DISTINCT

#### 날짜: 0207

### 문제
CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 자동차 종류가 '세단'인 자동차들 중 10월에 대여를 시작한 기록이 있는 자동차 ID 리스트를 출력하는 SQL문을 작성해주세요. 자동차 ID 리스트는 중복이 없어야 하며, 자동차 ID를 기준으로 내림차순 정렬해주세요.

### 정답 쿼리
자동차 ID 리스트는 중복이 없어야 -> DISTINCT

```sql
SELECT
    DISTINCT(A.CAR_ID)
FROM CAR_RENTAL_COMPANY_CAR AS A
JOIN CAR_RENTAL_COMPANY_RENTAL_HISTORY AS B
ON A.CAR_ID = B.CAR_ID
WHERE
    CAR_TYPE = '세단'
    AND MONTH(START_DATE) = 10
ORDER BY
    A.CAR_ID DESC;
```


## Q4. 즐겨찾기가 가장 많은 식당 정보 출력하기 ⭐⭐
> WHERE절 서브쿼리

#### 날짜: 0207

### 문제
REST_INFO 테이블에서 음식종류별로 즐겨찾기수가 가장 많은 식당의 음식 종류, ID, 식당 이름, 즐겨찾기수를 조회하는 SQL문을 작성해주세요. 이때 결과는 음식 종류를 기준으로 내림차순 정렬해주세요.

### 정답 쿼리

```sql
SELECT
    FOOD_TYPE,
    REST_ID,
    REST_NAME,
    FAVORITES
FROM REST_INFO
WHERE (FOOD_TYPE, FAVORITES) IN (
    SELECT
        FOOD_TYPE, MAX(FAVORITES)
    FROM REST_INFO
    GROUP BY FOOD_TYPE)
ORDER BY
    FOOD_TYPE DESC;
```

### 원래 작성한 쿼리

각 그룹(FOOD_TYPE 별로)에서 MAX(FAVORITES)를 만족하는 행을 선택해야 하므로 문제 목적에 맞지 않음.

```SQL
SELECT
    FOOD_TYPE,
    REST_ID,
    REST_NAME,
    FAVORITES
FROM REST_INFO
GROUP BY FOOD_TYPE
HAVING MAX(FAVORITES)
ORDER BY
    FOOD_TYPE DESC;
```

### GROUP BY 없이 쿼리문 작성하기

> Q) WHERE FOOD_TYPE = R.FOOD_TYPE가 왜 GROUP BY FOOD_TYPE처럼 동작하는 것처럼 보이는가?

> A) 서브쿼리가 먼저 실행되어 각 행을 기준으로 해당 FOOD_TYPE에 대한 MAX(FAVORITES) 값을 서브쿼리에서 계산하고, 그 값과 일치하는 행만 선택하기 때문임.

```SQL
SELECT
    FOOD_TYPE,
    REST_ID,
    REST_NAME,
    FAVORITES
FROM REST_INFO AS R
WHERE FAVORITES = (
    SELECT MAX(FAVORITES)
    FROM REST_INFO
    WHERE FOOD_TYPE = R.FOOD_TYPE
)
ORDER BY FOOD_TYPE DESC;
```


## Q5. 없어진 기록 찾기
> 차집합

#### 날짜: 0207

### 문제
천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.

### 정답 쿼리

![차집합](/2025_W/img/5-1.PNG)   
A IS NULL을 사용하면 위의 그림과 같은 차집합 형식으로 JOIN하는 효과를 볼 수 있다.

```sql
SELECT
    B.ANIMAL_ID,
    B.NAME
FROM ANIMAL_INS AS A
RIGHT JOIN ANIMAL_OUTS AS B
ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE A.ANIMAL_ID IS NULL
ORDER BY B.ANIMAL_ID;
```


## Q6. 대장균의 크기에 따라 분류하기 1
> CASE WHEN END

#### 날짜: 0207

### 문제
대장균 개체의 크기가 100 이하라면 'LOW', 100 초과 1000 이하라면 'MEDIUM', 1000 초과라면 'HIGH' 라고 분류합니다. 대장균 개체의 ID(ID) 와 분류(SIZE)를 출력하는 SQL 문을 작성해주세요.이때 결과는 개체의 ID 에 대해 오름차순 정렬해주세요.

### 정답 쿼리

⭐ CASE WHEN 사용할 때 END 잊지 말기!!!!

```sql
SELECT
    ID,
    CASE
        WHEN SIZE_OF_COLONY <= 100 THEN 'LOW'
        WHEN SIZE_OF_COLONY <= 1000 THEN 'MEDIUM'
        ELSE 'HIGH'
    END AS SIZE
FROM ECOLI_DATA
ORDER BY ID;
```


## Q7. 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기 ⭐⭐
> CASE WHEN 안에 CASE WHEN

#### 날짜: 0207

### 문제
CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 2022년 10월 16일에 대여 중인 자동차인 경우 '대여중' 이라고 표시하고, 대여 중이지 않은 자동차인 경우 '대여 가능'을 표시하는 컬럼(컬럼명: AVAILABILITY)을 추가하여 자동차 ID와 AVAILABILITY 리스트를 출력하는 SQL문을 작성해주세요. 이때 반납 날짜가 2022년 10월 16일인 경우에도 '대여중'으로 표시해주시고 결과는 자동차 ID를 기준으로 내림차순 정렬해주세요.

### 정답 쿼리

```sql
SELECT
    CAR_ID,
    CASE
        WHEN MAX(CASE
                    WHEN '2022-10-16' BETWEEN START_DATE AND END_DATE THEN 1
                ELSE 0 END) = 1 THEN '대여중'
        ELSE '대여 가능'
    END AS AVAILABILITY
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
ORDER BY
    CAR_ID DESC;
```

### 문제 풀이 과정

CAR_ID 별로 한 번이라도 '대여중'인 것이 있다면 '대여중'으로 출력하는 것이 논리적으로 옳을 것임.   
따라서 아래와 같이 조건을 입력

```SQL
CASE
    WHEN MAX(CASE
                WHEN '2022-10-16' BETWEEN START_DATE AND END_DATE THEN 1
            ELSE 0 END) = 1 THEN '대여중'
    ELSE '대여 가능'
END AS AVAILABILITY
.
GROUP BY CAR_ID
```


## Q8. 헤비 유저가 소유한 장소 ⭐⭐
> IN, 서브쿼리

#### 날짜: 0207

### 문제
이 서비스에서는 공간을 둘 이상 등록한 사람을 "헤비 유저"라고 부릅니다. 헤비 유저가 등록한 공간의 정보를 아이디 순으로 조회하는 SQL문을 작성해주세요.

### 정답 쿼리
서브쿼리가 여러 개의 값을 반환하므로 = 대신 IN을 사용해야 함.

```sql
SELECT
    ID,
    NAME,
    HOST_ID
FROM PLACES
WHERE HOST_ID IN (
    SELECT HOST_ID
    FROM PLACES
    GROUP BY HOST_ID
    HAVING COUNT(*) >= 2)
ORDER BY ID;
```


## Q9. 특정 조건을 만족하는 물고기별 수와 최대 길이 구하기 ⭐⭐
> 

#### 날짜: 0209

### 문제
FISH_INFO에서 평균 길이가 33cm 이상인 물고기들을 종류별로 분류하여 잡은 수, 최대 길이, 물고기의 종류를 출력하는 SQL문을 작성해주세요. 결과는 물고기 종류에 대해 오름차순으로 정렬해주시고, 10cm이하의 물고기들은 10cm로 취급하여 평균 길이를 구해주세요.

컬럼명은 물고기의 종류 'FISH_TYPE', 잡은 수 'FISH_COUNT', 최대 길이 'MAX_LENGTH'로 해주세요.

### 정답 쿼리
정답으로 나오긴 했지만 테스트 6개 중 4개만 통과됨. 아마 NULL값이 10으로 처리되지 않았기 때문으로 예상됨.

```sql
WITH A AS (
     SELECT
        COUNT(*) AS FISH_COUNT,
        MAX(LENGTH) AS MAX_LENGTH,
        FISH_TYPE,
        AVG(CASE
                WHEN LENGTH <= 10 THEN 10
                ELSE LENGTH
            END) AS AVG_LENGTH
    FROM FISH_INFO
    GROUP BY FISH_TYPE
     )

SELECT
    FISH_COUNT,
    MAX_LENGTH,
    FISH_TYPE
FROM A
WHERE AVG_LENGTH >= 33
ORDER BY FISH_TYPE;
```

### WITH 문을 사용하지 않고 풀어보기
> GREATEST, IFNULL, HAVING

```SQL
SELECT
    COUNT(*) AS FISH_COUNT,
    MAX(LENGTH) AS MAX_LENGTH,
    FISH_TYPE
FROM FISH_INFO
GROUP BY FISH_TYPE
HAVING AVG(GREATEST(IFNULL(LENGTH, 10), 10)) >= 33
ORDER BY FISH_TYPE;
```

IFNULL(COLUMN, 숫자)은 COLUMN이 NULL값이면 '숫자'를 반환하고 그렇지 않으면 원래의 값을 반환한다. 이처럼 특정값 이하이면 특정값을 반환하고 초과하면 원래값을 반환하는 함수로는 GREATEST(COLUMN, 숫자)가 있다.

IFNULL 대신 COALESCE 를 사용해도 동일한 결과가 나온다.

📌 그렇다면 IFNULL과 COALESCE는 어떻게 다를까?
```
[IFNULL]
- IFNULL(column_name, '대체값')
- NULL이면 대체값을 반환, 아니면 원래 값을 반환.
- 오직 2개의 인자만 가질 수 있음
- MySQL에서만 사용 가능

[COALESCE]
- COALESCE(column1, column2, column3, '대체값')
- 여러 개의 값을 순차적으로 확인하고, NULL이 아닌 첫 번째 값을 반환.
- MySQL, PostgreSQL, SQL Server, Oracle 등 거의 모든 DBMS에서 사용 가능.
- 2개 이상의 인자를 가질 수 있음
```


## Q10. 대장균들의 자식의 수 구하기 ⭐
> 셀프조인

#### 날짜: 0209

### 문제
대장균 개체의 ID(ID)와 자식의 수(CHILD_COUNT)를 출력하는 SQL 문을 작성해주세요. 자식이 없다면 자식의 수는 0으로 출력해주세요. 이때 결과는 개체의 ID 에 대해 오름차순 정렬해주세요.

### 정답 쿼리

```sql
SELECT
    A.ID,
    COUNT(B.ID) AS CHILD_COUNT
FROM ECOLI_DATA AS A
LEFT JOIN ECOLI_DATA AS B
ON A.ID = B.PARENT_ID
GROUP BY A.ID
ORDER BY A.ID
```


## Q11. 업그레이드 할 수 없는 아이템 구하기
> 10번과 유사

#### 날짜: 0209

### 문제
더 이상 업그레이드할 수 없는 아이템의 아이템 ID(ITEM_ID), 아이템 명(ITEM_NAME), 아이템의 희귀도(RARITY)를 출력하는 SQL 문을 작성해 주세요. 이때 결과는 아이템 ID를 기준으로 내림차순 정렬해 주세요.

### 정답 쿼리

```sql
SELECT
    A.ITEM_ID,
    ITEM_NAME,
    RARITY
FROM ITEM_INFO AS A
LEFT JOIN ITEM_TREE AS B
ON A.ITEM_ID = B.PARENT_ITEM_ID
WHERE PARENT_ITEM_ID IS NULL
ORDER BY
    A.ITEM_ID DESC;
```

### 문제 풀이 과정

```sql
SELECT
    *
FROM ITEM_INFO AS A
LEFT JOIN ITEM_TREE AS B
ON A.ITEM_ID = B.PARENT_ITEM_ID
```

위의 쿼리를 실행하면 아래와 같은 결과가 출력됨.

![결과](/2025_W/img/5-2.PNG)


## Q12. 물고기 종류 별 대어 찾기 ⭐
> JOIN USING, WHERE IN

#### 날짜: 0209

### 문제
물고기 종류 별로 가장 큰 물고기의 ID, 물고기 이름, 길이를 출력하는 SQL 문을 작성해주세요.

물고기의 ID 컬럼명은 ID, 이름 컬럼명은 FISH_NAME, 길이 컬럼명은 LENGTH로 해주세요.
결과는 물고기의 ID에 대해 오름차순 정렬해주세요.
단, 물고기 종류별 가장 큰 물고기는 1마리만 있으며 10cm 이하의 물고기가 가장 큰 경우는 없습니다.

### 정답 쿼리
USING을 사용할 경우 컬럼 네임을 명시할 때 괄호를 꼭 사용해주어야 한다!

```sql
SELECT
    ID,
    FISH_NAME,
    LENGTH
FROM FISH_INFO AS A
JOIN FISH_NAME_INFO AS B
USING (FISH_TYPE)
WHERE (A.FISH_TYPE, LENGTH)
    IN (
        SELECT
            FISH_TYPE,
            MAX(LENGTH)
        FROM FISH_INFO
        GROUP BY FISH_TYPE)
ORDER BY ID;
```


## Q13. 대장균의 크기에 따라 분류하기 2
> NTILE, OVER

#### 날짜: 0209

### 문제
대장균 개체의 크기를 내름차순으로 정렬했을 때 상위 0% ~ 25% 를 'CRITICAL', 26% ~ 50% 를 'HIGH', 51% ~ 75% 를 'MEDIUM', 76% ~ 100% 를 'LOW' 라고 분류합니다. 대장균 개체의 ID(ID) 와 분류된 이름(COLONY_NAME)을 출력하는 SQL 문을 작성해주세요. 이때 결과는 개체의 ID 에 대해 오름차순 정렬해주세요 . 단, 총 데이터의 수는 4의 배수이며 같은 사이즈의 대장균 개체가 서로 다른 이름으로 분류되는 경우는 없습니다.

### 정답 쿼리

```sql
SELECT
    ID,
    CASE
        WHEN NTILE(4) OVER (ORDER BY SIZE_OF_COLONY DESC) = 1 THEN 'CRITICAL'
        WHEN NTILE(4) OVER (ORDER BY SIZE_OF_COLONY DESC) = 2 THEN 'HIGH'
        WHEN NTILE(4) OVER (ORDER BY SIZE_OF_COLONY DESC) = 3 THEN 'MEDIUM'
        ELSE 'LOW'
    END AS COLONY_NAME
FROM ECOLI_DATA
ORDER BY ID;
```

### 새로운 함수 공부하기

✅ SQL에서 행의 순서를 정의하는 함수

| 함수명 | 특징 |
|--------|------|
| **`ROW_NUMBER()`** | 중복된 값과 상관없이 연속적인 행 번호 부여 |
| **`RANK()`** | 동일한 값이면 같은 순위를 부여하고 다음 순위는 건너뜀 |
| **`DENSE_RANK()`** | 동일한 값이면 같은 순위를 부여하지만, 건너뛰지 않고 연속적인 순위 유지 |
| **`NTILE(n)`** | 데이터를 `n`개의 그룹으로 균등하게 나눔 |


## **1️⃣ `ROW_NUMBER()`**
✔ **각 행에 고유한 순번(1, 2, 3...)을 부여하는 함수**  
✔ **동일한 값이 있어도 무조건 고유한 번호를 매김**  

```sql
SELECT 
    column_name,
    ROW_NUMBER() OVER (ORDER BY column_name ASC) AS row_num
FROM table_name;
```
✔ **`ORDER BY` 기준으로 행 번호(`row_num`)가 부여됨**  
✔ **동일한 값이 있어도 고유한 번호를 가짐**

---

## **2️⃣ `RANK()`**
✔ **같은 값이 있으면 동일한 순위를 부여하지만, 그다음 순위는 건너뜀**  
✔ **1, 2, 2, 4 같은 방식으로 순위가 부여됨**  

```sql
SELECT 
    column_name,
    RANK() OVER (ORDER BY column_name DESC) AS ranking
FROM table_name;
```
---

## **3️⃣ `DENSE_RANK()`**
✔ **`RANK()`와 비슷하지만, 순위를 건너뛰지 않음**  
✔ **1, 2, 2, 3 같은 방식으로 순위가 부여됨**  

```sql
SELECT 
    column_name,
    DENSE_RANK() OVER (ORDER BY column_name DESC) AS ranking
FROM table_name;
```
---

## **4️⃣ `NTILE(n)`**
✔ **데이터를 `n`개의 그룹으로 나누고, 그룹 번호를 부여**  
✔ **예를 들어, `NTILE(4)`를 사용하면 데이터를 4개의 그룹으로 균등하게 나눔**  

```sql
SELECT 
    column_name,
    NTILE(4) OVER (ORDER BY column_name ASC) AS group_num
FROM table_name;
```
---
---

✅ GROUP BY vs. OVER()

**`OVER()` 함수는 특정 그룹(윈도우) 내에서 연산을 수행**할 때 사용할 수 있다.

`OVER()` 없이 `GROUP BY`를 사용하면 전체 그룹에 대해 하나의 값만 남지만,  
**`OVER()`를 사용하면 개별 행을 유지하면서 집계 연산을 수행할 수 있다.**  

---

## **1️⃣ `OVER()`의 기본 구조**
```sql
윈도우_함수() OVER (
    [PARTITION BY 컬럼]  -- (선택 사항) 특정 그룹으로 나누기
    ORDER BY 컬럼        -- (선택 사항) 정렬 기준 지정
)
```
✔ `OVER()`는 `PARTITION BY`와 `ORDER BY`를 조합하여 사용할 수 있다.  
✔ 윈도우 함수(`ROW_NUMBER()`, `RANK()`, `SUM()`, `AVG()`, `NTILE()`)와 함께 사용된다.

---

## **2️⃣ `OVER()` 없이 `GROUP BY`를 사용할 때 문제점**
만약 `OVER()` 없이 `SUM()`을 사용하면 **집계 결과만 남고 개별 행이 사라진다.**
```sql
SELECT
    category,
    SUM(sales)
FROM sales_data
GROUP BY category;
```
[결과] 
| category | SUM(sales) |
|----------|------------|
| A        | 300        |
| B        | 400        |


반면 `OVER()`을 사용하면 **개별 행이 유지**된다.
```sql
SELECT
    category,
    sales,
    SUM(sales) OVER (PARTITION BY category) AS total_sales
FROM sales_data;
```
[결과] 
| category | sales | total_sales |
|----------|------|------------|
| A        | 100  | 300        |
| A        | 200  | 300        |
| B        | 150  | 400        |
| B        | 250  | 400        |

---
## **3️⃣ `ORDER BY`를 사용하여 누적 값 계산**

```sql
SELECT 
    category,
    product,
    sales,
    SUM(sales) OVER (PARTITION BY category ORDER BY sales) AS running_total
FROM sales_data;
```
[결과]
| category | product | sales | running_total |
|----------|---------|------|--------------|
| A        | item1   | 100  | 100          |
| A        | item2   | 200  | 300          |
| B        | item3   | 150  | 150          |
| B        | item4   | 250  | 400          |
---
---


## Q14. 식품분류별 가장 비싼 식품의 정보 조회하기
> WHERE 서브쿼리

#### 날짜: 0209

### 문제
FOOD_PRODUCT 테이블에서 식품분류별로 가격이 제일 비싼 식품의 분류, 가격, 이름을 조회하는 SQL문을 작성해주세요. 이때 식품분류가 '과자', '국', '김치', '식용유'인 경우만 출력시켜 주시고 결과는 식품 가격을 기준으로 내림차순 정렬해주세요.

### 정답 쿼리

MAX(PRICE)와 정확히 매치되는 PRODUCT_NAME을 출력하려면 WHERE 절에 서브쿼리를 사용해야 함.

```sql
SELECT
    CATEGORY,
    PRICE AS MAX_PRICE,
    PRODUCT_NAME
FROM FOOD_PRODUCT
WHERE (CATEGORY, PRICE)
    IN (
        SELECT
            CATEGORY,
            MAX(PRICE)
        FROM FOOD_PRODUCT
        GROUP BY CATEGORY)
    AND CATEGORY IN ('과자', '국', '김치', '식용유')
GROUP BY CATEGORY
ORDER BY
    PRICE DESC;
```


## Q15. 5월 식품들의 총매출 조회하기
> SUM

#### 날짜: 0209

### 문제
FOOD_PRODUCT와 FOOD_ORDER 테이블에서 생산일자가 2022년 5월인 식품들의 식품 ID, 식품 이름, 총매출을 조회하는 SQL문을 작성해주세요. 이때 결과는 총매출을 기준으로 내림차순 정렬해주시고 총매출이 같다면 식품 ID를 기준으로 오름차순 정렬해주세요.

### 문제 풀이 과정

```sql
SELECT
    A.PRODUCT_ID,
    PRODUCT_NAME,
    PRICE * AMOUNT AS TOTAL_SALES
FROM FOOD_PRODUCT AS A
JOIN FOOD_ORDER AS B
USING (PRODUCT_ID)
WHERE PRODUCE_DATE LIKE '2022-05-%'
ORDER BY
    TOTAL_SALES DESC,
    A.PRODUCT_ID;
```

[출력 결과]   
![결과](/2025_W/img/5-3.PNG)

-> 맛있는 마요네즈가 중복


### 정답 쿼리

PRODUCT_NAME은 PRODUCT_ID와 일대일(1:1) 매칭되는 속성이므로 암묵적으로 결정된다고 보기 때문에 이 경우에는 GROUP BY에 PRODUCT_NAME이 포함되지 않고, 집계연산자도 사용되지 않았는데도 잘 실행됨.

```SQL
SELECT
    A.PRODUCT_ID,
    PRODUCT_NAME,
    SUM(PRICE * AMOUNT) AS TOTAL_SALES
FROM FOOD_PRODUCT AS A
JOIN FOOD_ORDER AS B
USING (PRODUCT_ID)
WHERE PRODUCE_DATE LIKE '2022-05-%'
GROUP BY A.PRODUCT_ID
ORDER BY
    TOTAL_SALES DESC,
    A.PRODUCT_ID;
```