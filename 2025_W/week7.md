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
> WITH RECURSIVE

#### 날짜: 0303

### 문제
3세대의 대장균의 ID(ID) 를 출력하는 SQL 문을 작성해주세요. 이때 결과는 대장균의 ID 에 대해 오름차순 정렬해주세요.

### 정답 쿼리

```SQL
[1번. 재귀식 사용하지 않고 JOIN 2번으로 해결]
SELECT D.ID
FROM ECOLI_DATA AS A
JOIN ECOLI_DATA AS B
    ON A.ID = B.PARENT_ID
JOIN ECOLI_DATA AS D
    ON B.ID = D.PARENT_ID
WHERE A.PARENT_ID IS NULL
ORDER BY ID;
```

```sql
[2번. 재귀식 사용]
WITH RECURSIVE C AS (
    SELECT ID, PARENT_ID, 1 AS LEVEL
    FROM ECOLI_DATA 
    WHERE PARENT_ID IS NULL
    UNION ALL
    SELECT E.ID, E.PARENT_ID, C.LEVEL + 1
    FROM ECOLI_DATA AS E
    JOIN C ON E.PARENT_ID = C.ID
)
SELECT ID
FROM C
WHERE LEVEL = 3
ORDER BY ID;
```

### 재귀식 작동 순서
최상위 부모에서 시작해서 자식 노드를 따라가면서 트리 구조를 탐색하는 방식

> ✅ **1단계: 최상위 부모 찾기 (Anchor Part)**
```sql
SELECT
    ID,
    PARENT_ID,
    1 AS LEVEL
FROM ECOLI_DATA 
WHERE PARENT_ID IS NULL
```
- `PARENT_ID IS NULL`인 데이터를 찾아서 **루트 노드(최상위 부모)**를 가져옴.
- `LEVEL = 1`을 부여해서 **이 노드가 1세대임을 표시**.

> ✅ **2단계: 부모-자식 관계를 재귀적으로 찾기 (Recursive Part)**
```sql
SELECT
    E.ID,
    E.PARENT_ID,
    C.LEVEL + 1
FROM ECOLI_DATA AS E
JOIN C
ON E.PARENT_ID = C.ID
```
- `E.PARENT_ID = C.ID`를 기준으로 **자식 노드를 찾음**.
- 찾은 자식 노드는 부모보다 한 단계 아래 (`LEVEL + 1`)로 설정됨.
- 이 과정이 계속 반복되어 **손자, 증손자 등 모든 세대를 찾아감**.

---

### 🔍 **`JOIN`, `UNION`, `UNION ALL`의 차이점 정리표**

| 연산자      | 기능 | 사용 목적 | 동작 방식 | 중복 처리 | 주요 특징 |
|------------|------|----------|----------|----------|----------|
| **JOIN** | 두 개 이상의 테이블을 결합 | 테이블 간 관계 조회 | `ON` 조건을 만족하는 행들만 결합 | 중복된 행이 있을 수 있음 | 한 번의 `JOIN`으로 한 단계의 관계만 조회 가능 |
| **UNION** | 여러 `SELECT` 결과를 합침 | 동일한 구조의 데이터를 합칠 때 | 두 개 이상의 `SELECT` 결과를 병합 | ✅ **중복 제거 (`DISTINCT`)** | 중복 제거로 인해 성능이 `UNION ALL`보다 느림 |
| **UNION ALL** | 여러 `SELECT` 결과를 합침 | 동일한 구조의 데이터를 합칠 때 | 두 개 이상의 `SELECT` 결과를 병합 | ❌ **중복 제거 없음** | `UNION`보다 빠르며, 재귀적 `WITH RECURSIVE`에서 데이터 누적 시 사용 |

---

> 🔥 **JOIN vs UNION vs UNION ALL 언제 사용해야 할까?**

| 사용 목적 | 추천 연산자 |
|-----------|------------|
| **두 개 이상의 테이블을 결합하여 데이터 조회** | `JOIN` |
| **여러 `SELECT` 결과를 하나로 합치되, 중복 제거가 필요** | `UNION` |
| **여러 `SELECT` 결과를 하나로 합치되, 중복을 유지해야 함** | `UNION ALL` |
| **재귀 쿼리에서 데이터를 계속 누적해야 함 (`WITH RECURSIVE`)** | `UNION ALL` |

---

## Q4. 자동차 대여 기록 별 대여 금액 구하기
> CASE WHEN, COALESCE

#### 날짜: 0302

### 문제
CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블과 CAR_RENTAL_COMPANY_DISCOUNT_PLAN 테이블에서 자동차 종류가 '트럭'인 자동차의 대여 기록에 대해서 대여 기록 별로 대여 금액(컬럼명: FEE)을 구하여 대여 기록 ID와 대여 금액 리스트를 출력하는 SQL문을 작성해주세요. 결과는 대여 금액을 기준으로 내림차순 정렬하고, 대여 금액이 같은 경우 대여 기록 ID를 기준으로 내림차순 정렬해주세요.

### 정답 쿼리

`일일대여금액 * 대여일수 * (1-할인율)` 을 했어야 했는데 대여일수를 곱하지 않아 계속 오류가 떴음. 문제 잘 확인하기!! 

```sql
WITH C AS (
    SELECT
        A.CAR_ID,
        CAR_TYPE,
        DAILY_FEE,
        HISTORY_ID,
        START_DATE,
        END_DATE,
        CASE 
            WHEN DATEDIFF(B.END_DATE, B.START_DATE) + 1 >= 90 THEN '90일 이상'
            WHEN DATEDIFF(B.END_DATE, B.START_DATE) + 1 >= 30 THEN '30일 이상'
            WHEN DATEDIFF(B.END_DATE, B.START_DATE) + 1 >= 7 THEN '7일 이상'
            ELSE '할인 없음'
        END AS DURATION_TYPE
    FROM CAR_RENTAL_COMPANY_CAR AS A
    JOIN CAR_RENTAL_COMPANY_RENTAL_HISTORY AS B
    USING (CAR_ID)
    WHERE CAR_TYPE = '트럭'
)
SELECT
    HISTORY_ID,
    ROUND(DAILY_FEE * (DATEDIFF(C.END_DATE, C.START_DATE) + 1) * (1-COALESCE(DISCOUNT_RATE,0)/100), 0) AS FEE
FROM C
LEFT JOIN (
    SELECT *
    FROM CAR_RENTAL_COMPANY_DISCOUNT_PLAN
    WHERE CAR_TYPE = '트럭') AS D
USING (DURATION_TYPE)
ORDER BY
    FEE DESC,
    HISTORY_ID DESC;
```

## Q5. FrontEnd 개발자 찾기
> WHERE EXISTS, 비트연산자

#### 날짜: 0303

### 문제
DEVELOPERS 테이블에서 Front End 스킬을 가진 개발자의 정보를 조회하려 합니다. 조건에 맞는 개발자의 ID, 이메일, 이름, 성을 조회하는 SQL 문을 작성해 주세요.
결과는 ID를 기준으로 오름차순 정렬해 주세요.

### 정답 쿼리

비트연산자를 사용하려면 ANY 사용 불가.   
EXISTS를 사용해야 비트연산자 사용 가능.

```sql
SELECT
    ID,
    EMAIL,
    FIRST_NAME,
    LAST_NAME
FROM DEVELOPERS AS D
WHERE EXISTS (
    SELECT 1
    FROM SKILLCODES AS S
    WHERE S.CATEGORY = 'Front End'
    AND (D.SKILL_CODE & S.CODE) != 0
)
ORDER BY ID;
```
- EXISTS는 서브쿼리의 SELECT 부분이 아니라 WHERE 조건을 평가하여 TRUE 또는 FALSE를 반환함.
- 즉, 서브쿼리 내에서 어떤 값을 SELECT하든 관계없이 해당 조건을 만족하는 행이 존재하는지 여부만 확인함.
- 성능 최적화를 위해 일반적으로 SELECT 1을 사용하는 것이 권장됨.
- SQL에서는 `!=` 대신 `<>`도 사용 가능


### 시도한 코드

```sql
SELECT
    ID,
    EMAIL,
    FIRST_NAME,
    LAST_NAME
FROM DEVELOPERS
WHERE SKILL_CODE & ANY
    (SELECT CODE
    FROM SKILLCODES
    WHERE CATEGORY = 'Front End')
ORDER BY ID;
```

ANY를 사용할 때는 반드시 **비교 연산자(예: =, >, <, <>, >=, <=)** 와 함께 사용해야 함. 따라서 비트연산자와 사용 불가.


## Q6. 특정 기간동안 대여 가능한 자동차들의 대여비용 구하기
> CASE WHEN, MAX

#### 날짜: 0303

### 문제
CAR_RENTAL_COMPANY_CAR 테이블과 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블과 CAR_RENTAL_COMPANY_DISCOUNT_PLAN 테이블에서 자동차 종류가 '세단' 또는 'SUV' 인 자동차 중 2022년 11월 1일부터 2022년 11월 30일까지 대여 가능하고 30일간의 대여 금액이 50만원 이상 200만원 미만인 자동차에 대해서 자동차 ID, 자동차 종류, 대여 금액(컬럼명: FEE) 리스트를 출력하는 SQL문을 작성해주세요. 결과는 대여 금액을 기준으로 내림차순 정렬하고, 대여 금액이 같은 경우 자동차 종류를 기준으로 오름차순 정렬, 자동차 종류까지 같은 경우 자동차 ID를 기준으로 내림차순 정렬해주세요.

### 정답 쿼리

엄청 오래 걸림....
1. 한 번이라도 대여 중인 CAR_ID는 대여중인 것으로 취급되어야 할 것임. -> MAX 사용
2. 30일 간의 대여금액이 50만원 이상 200만원 미만인 경우를 찾아야 하므로 DURATION_TYPE이 `30일 이상`에 해당하는 할인율을 적용해야 할 것임

```sql
WITH A AS (
    SELECT CAR_ID
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
    GROUP BY CAR_ID
    HAVING MAX(CASE 
                 WHEN START_DATE <= '2022-11-30' AND END_DATE >= '2022-11-01' 
                 THEN 1 ELSE 0 
               END) = 0
)
SELECT 
    C.CAR_ID, 
    C.CAR_TYPE,
    ROUND(30 * C.DAILY_FEE * (100 - COALESCE(P.DISCOUNT_RATE, 0))/100) AS FEE
FROM CAR_RENTAL_COMPANY_CAR AS C
JOIN A
ON C.CAR_ID = A.CAR_ID
JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN AS P
ON C.CAR_TYPE = P.CAR_TYPE
AND P.DURATION_TYPE = '30일 이상'
WHERE 
    C.CAR_TYPE IN ('SUV', '세단')
    AND ROUND(30 * C.DAILY_FEE * (100 - COALESCE(P.DISCOUNT_RATE, 0))/100) BETWEEN 500000 AND 2000000
ORDER BY 
    FEE DESC,
    C.CAR_TYPE,
    C.CAR_ID DESC;
```


## Q7. 언어별 개발자 분류하기
> GROUP_CONCAT

#### 날짜: 0303

### 문제
DEVELOPERS 테이블에서 GRADE별 개발자의 정보를 조회하려 합니다. GRADE는 다음과 같이 정해집니다.
A : Front End 스킬과 Python 스킬을 함께 가지고 있는 개발자
B : C# 스킬을 가진 개발자
C : 그 외의 Front End 개발자
GRADE가 존재하는 개발자의 GRADE, ID, EMAIL을 조회하는 SQL 문을 작성해 주세요.
결과는 GRADE와 ID를 기준으로 오름차순 정렬해 주세요.

### 정답 쿼리

1. GROUP_CONCAT을 이용하여 해당하는 모든 것을 리스트 형태로 나열
2. SELECT에서 별칭으로 정의한 것을 GROUP BY에서 사용할 수 없기 때문에 HAVING에서 사용. -> 아주 기본적인 것이지만 쿼리 작성 시 자주 까먹음...

```sql
WITH A AS (
    SELECT
        ID,
        EMAIL,
        GROUP_CONCAT(S.NAME) AS NAME,
        GROUP_CONCAT(S.CATEGORY) AS CATEGORY
    FROM DEVELOPERS AS D
    JOIN SKILLCODES AS S
    ON (D.SKILL_CODE & S.CODE) = S.CODE
    GROUP BY D.ID, D.EMAIL
)
SELECT
    CASE
        WHEN (CATEGORY LIKE '%Front End%') AND (NAME LIKE '%Python%') THEN 'A'
        WHEN (NAME LIKE '%C#%') THEN 'B'
        WHEN (CATEGORY LIKE '%Front End%') THEN 'C'
        ELSE 'OTHER'
    END AS GRADE,
    ID,
    EMAIL
FROM A
HAVING GRADE != 'OTHER'
ORDER BY GRADE, ID;
```


## Q8. 멸종위기의 대장균 찾기
> WITH RECURSIVE, WHERE NOT EXISTS

#### 날짜: 0303

### 문제
각 세대별 자식이 없는 개체의 수(COUNT)와 세대(GENERATION)를 출력하는 SQL문을 작성해주세요. 이때 결과는 세대에 대해 오름차순 정렬해주세요. 단, 모든 세대에는 자식이 없는 개체가 적어도 1개체는 존재합니다.

### 정답 쿼리

```sql
WITH RECURSIVE C AS (
    SELECT ID, PARENT_ID, 1 AS GENERATION
    FROM ECOLI_DATA 
    WHERE PARENT_ID IS NULL
    UNION ALL
    SELECT E.ID, E.PARENT_ID, C.GENERATION + 1
    FROM ECOLI_DATA AS E
    JOIN C ON E.PARENT_ID = C.ID
)
SELECT
    COUNT(*) AS COUNT,
    GENERATION
FROM C
WHERE NOT EXISTS
    (SELECT 1
    FROM ECOLI_DATA AS E
    WHERE E.PARENT_ID = C.ID)
GROUP BY GENERATION
ORDER BY GENERATION;
```


### 문제 풀이 과정
3번과 유사했음.   
차이점은 `자식이 없는 개체`를 찾아야 한다는 것이었는데, 아래의 쿼리를 이용해서 찾을 수 있었음.
```SQL
WHERE NOT EXISTS
    (SELECT 1
    FROM ECOLI_DATA AS E
    WHERE E.PARENT_ID = C.ID)
```

5번에서 사용한 WHERE EXISTS
```SQL
WHERE EXISTS 
    (SELECT 1
    FROM SKILLCODES AS S
    WHERE S.CATEGORY = 'Front End'
    AND (D.SKILL_CODE & S.CODE) != 0)
```


7주차 결론: 난이도가 높은 문제로 가니까 재귀식, 기본 SELECT, FROM, WHERE에서 서브쿼리를 많이 사용하게 된다. 앞으로는 어떻게 해야 더 성능이 좋은 쿼리를 짤 수 있을까를 고민하는 것이 관건이겠다.