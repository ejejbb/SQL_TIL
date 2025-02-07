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
> 

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