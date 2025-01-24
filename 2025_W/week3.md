# Third Study Week

2주에 걸쳐 LV.2(+ 정답률 낮은 LV.1) 클리어하기

#### [문제1~19_링크](https://school.programmers.co.kr/learn/challenges?order=acceptance_desc&page=1&levels=2&languages=mysql)

## Q1. 중복 제거하기
> 서브쿼리, COUNT, GROUP BY

#### 날짜: 0124

### 문제설명
ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 문제
동물 보호소에 들어온 동물의 이름은 몇 개인지 조회하는 SQL 문을 작성해주세요. 이때 이름이 NULL인 경우는 집계하지 않으며 중복되는 이름은 하나로 칩니다.

### 정답 쿼리
```sql
SELECT
    COUNT(*) AS count
FROM(SELECT
    COUNT(*) AS cnt
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
GROUP BY NAME) AS A;
```

### 문제 풀이 과정

#### 1. NULL이 아닌 NAME을 기준으로 그룹화

```sql
SELECT
    NAME,
    COUNT(*) AS cnt
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
GROUP BY NAME
```

#### 2. 1번 쿼리에서 NAME 부분을 지우고 서브쿼리화

```sql
SELECT
    COUNT(*) AS count
FROM(SELECT
    COUNT(*) AS cnt
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
GROUP BY NAME) AS A;
```


## Q2. 최솟값 구하기
> ORDER BY, LIMIT

#### 날짜: 0124

### 문제설명
ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 문제
동물 보호소에 가장 먼저 들어온 동물은 언제 들어왔는지 조회하는 SQL 문을 작성해주세요.

### 정답 쿼리

DATETIME을 오름차순 정렬하여 가장 위에 있는 하나의 값만 남겨야겠다.
```sql
SELECT
    DATETIME AS 시간
FROM ANIMAL_INS
ORDER BY
    DATETIME ASC
LIMIT 1;
```


## Q3. 동명 동물 수 찾기
> 서브쿼리

#### 날짜: 0124

### 문제설명
ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 문제
동물 보호소에 들어온 동물 이름 중 두 번 이상 쓰인 이름과 해당 이름이 쓰인 횟수를 조회하는 SQL문을 작성해주세요. 이때 결과는 이름이 없는 동물은 집계에서 제외하며, 결과는 이름 순으로 조회해주세요.

### 정답 쿼리

```sql
SELECT
    NAME,
    cnt AS COUNT
FROM(
    SELECT
        NAME,
        COUNT(*) AS cnt
    FROM ANIMAL_INS
    WHERE NAME IS NOT NULL
    GROUP BY NAME
) AS A
WHERE cnt >= 2
ORDER BY NAME ASC;
```

### 문제 풀이 과정

#### 1. NULL이 아닌 NAME을 기준으로 그룹화

```sql
SELECT
    NAME,
    COUNT(*) AS cnt
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
GROUP BY NAME
```

#### 2. 1번 쿼리문을 서브쿼리화하고, 조건 걸기

조건: 두 번 이상 쓰인 이름 / 결과는 이름 순

```sql
SELECT
    NAME,
    cnt AS COUNT
FROM(
    SELECT
        NAME,
        COUNT(*) AS cnt
    FROM ANIMAL_INS
    WHERE NAME IS NOT NULL
    GROUP BY NAME
) AS A
WHERE cnt >= 2
ORDER BY NAME ASC;
```


## Q4. 이름에 el이 들어가는 동물 찾기
> LIKE

#### 날짜: 0124

### 문제설명
ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 문제
보호소에 돌아가신 할머니가 기르던 개를 찾는 사람이 찾아왔습니다. 이 사람이 말하길 할머니가 기르던 개는 이름에 'el'이 들어간다고 합니다. 동물 보호소에 들어온 동물 이름 중, 이름에 "EL"이 들어가는 개의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 이름 순으로 조회해주세요. 단, 이름의 대소문자는 구분하지 않습니다.

### 정답 쿼리

조건: 개 / 이름에 'EL'이 들어감 -> LIKE

```sql
SELECT
    ANIMAL_ID,
    NAME
FROM ANIMAL_INS
WHERE ANIMAL_TYPE = 'DOG'
    AND NAME LIKE '%el%'
ORDER BY
    NAME ASC;
```


## Q5. NULL 처리하기
> IFNULL 

#### 날짜: 0124

### 문제설명
ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 문제
입양 게시판에 동물 정보를 게시하려 합니다. 동물의 생물 종, 이름, 성별 및 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요. 이때 프로그래밍을 모르는 사람들은 NULL이라는 기호를 모르기 때문에, 이름이 없는 동물의 이름은 "No name"으로 표시해 주세요.

### 정답 쿼리

NAME이 NULL이면 "No name"으로 변경 -> IFNULL 사용

```sql
SELECT
    ANIMAL_TYPE,
    IFNULL(NAME, 'No name') AS NAME,
    SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY
    ANIMAL_ID ASC;
```



## Q6. DATETIME에서 DATE로 형 변환
> DATE_FORMAT

#### 날짜: 0124

### 문제설명
ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 문제
ANIMAL_INS 테이블에 등록된 모든 레코드에 대해, 각 동물의 아이디와 이름, 들어온 날짜1를 조회하는 SQL문을 작성해주세요. 이때 결과는 아이디 순으로 조회해야 합니다.

### 정답 쿼리

```sql
SELECT
    ANIMAL_ID,
    NAME,
    DATE_FORMAT(DATETIME, '%Y-%m-%d') AS 날짜
FROM ANIMAL_INS
ORDER BY
    ANIMAL_ID ASC;
```

```
DATE(DATETIME)
CONVERT(DATETIME, DATE)
CAST(DATETIME AS DATE)

위의 세 가지 방법을 모두 사용해봤지만,
출력결과가 TIME부분이 0으로 바뀌는 형태로 출력됨.

출력결과를 확실히 DATE만 반환하도록 하려면 결과를 VARCHAR로 반환하여야 함을 알게됨.
=> DATE_FORMAT(DATETIME, '%Y-%m-%d') 사용

※주의할 점: '%Y-%M-%D'로 입력하면 '2018-January-22nd' 이렇게 반환됨
```

## Q7. 가격이 제일 비싼 식품의 정보 출력하기
> ORDER BY, LIMIT

#### 날짜: 0124

### 문제설명
다음은 식품의 정보를 담은 FOOD_PRODUCT 테이블입니다. FOOD_PRODUCT 테이블은 다음과 같으며 PRODUCT_ID, PRODUCT_NAME, PRODUCT_CD, CATEGORY, PRICE는 식품 ID, 식품 이름, 식품 코드, 식품분류, 식품 가격을 의미합니다.

### 문제
FOOD_PRODUCT 테이블에서 가격이 제일 비싼 식품의 식품 ID, 식품 이름, 식품 코드, 식품분류, 식품 가격을 조회하는 SQL문을 작성해주세요.

### 정답 쿼리

```sql
SELECT
    *
FROM FOOD_PRODUCT
ORDER BY
    PRICE DESC
LIMIT 1;
```


## Q8. 조건에 맞는 아이템들의 가격의 총합 구하기
> SUM

#### 날짜: 0124

### 문제설명
다음은 어느 한 게임에서 사용되는 아이템들의 아이템 정보를 담은 ITEM_INFO 테이블입니다. ITEM_INFO 테이블은 다음과 같으며, ITEM_ID, ITEM_NAME, RARITY, PRICE는 각각 아이템 ID, 아이템 명, 아이템의 희귀도, 아이템의 가격을 나타냅니다.

### 문제
ITEM_INFO 테이블에서 희귀도가 'LEGEND'인 아이템들의 가격의 총합을 구하는 SQL문을 작성해 주세요. 이때 컬럼명은 'TOTAL_PRICE'로 지정해 주세요.

### 정답 쿼리

```sql
SELECT
    SUM(PRICE) AS TOTAL_PRICE
FROM ITEM_INFO
WHERE RARITY = 'LEGEND'
```


## Q9. 카테고리 별 상품 개수 구하기
> COUNT, LEFT, GROUP BY

#### 날짜: 0124

### 문제설명
다음은 어느 의류 쇼핑몰에서 판매중인 상품들의 정보를 담은 PRODUCT 테이블입니다. PRODUCT 테이블은 아래와 같은 구조로 되어있으며, PRODUCT_ID, PRODUCT_CODE, PRICE는 각각 상품 ID, 상품코드, 판매가를 나타냅니다.   
상품 별로 중복되지 않는 8자리 상품코드 값을 가지며, 앞 2자리는 카테고리 코드를 의미합니다.

### 문제
PRODUCT 테이블에서 상품 카테고리 코드(PRODUCT_CODE 앞 2자리) 별 상품 개수를 출력하는 SQL문을 작성해주세요. 결과는 상품 카테고리 코드를 기준으로 오름차순 정렬해주세요.

### 정답 쿼리

```sql
SELECT
    상품코드 AS CATEGORY,
    COUNT(*) AS PRODUCTS
FROM(
    SELECT
        *,
        LEFT(PRODUCT_CODE, 2) AS 상품코드
    FROM PRODUCT) AS A
GROUP BY 상품코드
ORDER BY 상품코드 ASC;
```

### 문제 풀이 과정

#### 1. LEFT 함수를 사용하여 상품코드 뽑아보기
```SQL
SELECT
    *,
    LEFT(PRODUCT_CODE, 2) AS 상품코드
FROM PRODUCT
```
![실행결과](/2025_W/img/3.PNG)

#### 2. 상품코드별로 그룹화하고 COUNT 사용하여 카테고리 별 상품 개수 구하기
```SQL
SELECT
    상품코드 AS CATEGORY,
    COUNT(*) AS PRODUCTS
FROM(
    SELECT
        *,
        LEFT(PRODUCT_CODE, 2) AS 상품코드
    FROM PRODUCT) AS A
GROUP BY 상품코드
ORDER BY 상품코드 ASC;
```

## Q10. 동물 수 구하기
> COUNT

#### 날짜: 0124

### 문제설명
ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 문제
동물 보호소에 동물이 몇 마리 들어왔는지 조회하는 SQL 문을 작성해주세요.

### 정답 쿼리

동물 종류 수 구하는 줄 알고 헤맴....

```sql
SELECT
    COUNT(*) AS count
FROM ANIMAL_INS;
```

### 번외(동물 종류의 수 구하기)

```SQL
[방법1]
SELECT
    COUNT(DISTINCT ANIMAL_TYPE) AS COUNT
FROM ANIMAL_INS;
```

```SQL
[방법2]
SELECT
    COUNT(*) AS count
FROM(
    SELECT
        COUNT(ANIMAL_TYPE) AS cnt
    FROM ANIMAL_INS
    GROUP BY
        ANIMAL_TYPE) AS A
```

## Q11. 진료과별 총 예약 횟수 출력하기
> 

#### 날짜: 0124

### 문제설명
다음은 종합병원의 진료 예약정보를 담은 APPOINTMENT 테이블 입니다.   
APPOINTMENT 테이블은 다음과 같으며 APNT_YMD, APNT_NO, PT_NO, MCDP_CD, MDDR_ID, APNT_CNCL_YN, APNT_CNCL_YMD는 각각 진료예약일시, 진료예약번호, 환자번호, 진료과코드, 의사ID, 예약취소여부, 예약취소날짜를 나타냅니다.

### 문제
APPOINTMENT 테이블에서 2022년 5월에 예약한 환자 수를 진료과코드 별로 조회하는 SQL문을 작성해주세요. 이때, 컬럼명은 '진료과 코드', '5월예약건수'로 지정해주시고 결과는 진료과별 예약한 환자 수를 기준으로 오름차순 정렬하고, 예약한 환자 수가 같다면 진료과 코드를 기준으로 오름차순 정렬해주세요.

### 정답 쿼리

```sql

```
