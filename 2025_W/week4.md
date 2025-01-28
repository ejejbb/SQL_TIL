# Fourth Study Week

LV.2(+ 정답률 낮은 LV.1) 클리어, LV.3 4문제

#### [문제1~14_링크](https://school.programmers.co.kr/learn/challenges?order=acceptance_desc&languages=mysql&page=2&levels=2)

## LV.2 ~ Q14

## Q1. 가격대 별 상품 개수 구하기
> FLOOR

#### 날짜: 0128

### 문제설명
다음은 어느 의류 쇼핑몰에서 판매중인 상품들의 정보를 담은 PRODUCT 테이블입니다. PRODUCT 테이블은 아래와 같은 구조로 되어있으며, PRODUCT_ID, PRODUCT_CODE, PRICE는 각각 상품 ID, 상품코드, 판매가를 나타냅니다.

상품 별로 중복되지 않는 8자리 상품코드 값을 가지며 앞 2자리는 카테고리 코드를 나타냅니다.

### 문제
PRODUCT 테이블에서 만원 단위의 가격대 별로 상품 개수를 출력하는 SQL 문을 작성해주세요. 이때 컬럼명은 각각 컬럼명은 PRICE_GROUP, PRODUCTS로 지정해주시고 가격대 정보는 각 구간의 최소금액(10,000원 이상 ~ 20,000 미만인 구간인 경우 10,000)으로 표시해주세요. 결과는 가격대를 기준으로 오름차순 정렬해주세요.

### 정답 쿼리

CASE WHEN 을 사용하면 범위를 어디까지 설정해야 할지 모르는 문제가 발생하므로 FLOOR 함수를 사용

```sql
SELECT
    FLOOR(PRICE/10000)*10000 AS PRICE_GROUP,
    COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY PRICE_GROUP
ORDER BY PRICE_GROUP;
```


## Q2. 연도 별 평균 미세먼지 농도 조회하기
> YEAR, ROUND, AVG

#### 날짜: 0128

### 문제설명
AIR_POLLUTION 테이블은 전국의 월별 미세먼지 정보를 담은 테이블입니다. AIR_POLLUTION 테이블의 구조는 다음과 같으며 LOCATION1, LOCATION2, YM, PM_VAL1, PM_VAL2은 각각 지역구분1, 지역구분2, 측정일, 미세먼지 오염도, 초미세먼지 오염도를 의미합니다.

### 문제
AIR_POLLUTION 테이블에서 수원 지역의 연도 별 평균 미세먼지 오염도와 평균 초미세먼지 오염도를 조회하는 SQL문을 작성해주세요. 이때, 평균 미세먼지 오염도와 평균 초미세먼지 오염도의 컬럼명은 각각 PM10, PM2.5로 해 주시고, 값은 소수 셋째 자리에서 반올림해주세요.
결과는 연도를 기준으로 오름차순 정렬해주세요.

### 정답 쿼리

* 소수 셋째 자리에서 반올림 -> ROUND( , 2)
* 수원 지역
* 칼럼명에 .이 있는 경우 ""사용
* GROUP BY가 SELECT 보다 먼저 작동

```sql
SELECT
    YEAR(YM) AS YEAR,
    ROUND(AVG(PM_VAL1),2) AS PM10,
    ROUND(AVG(PM_VAL2),2) AS "PM2.5"
FROM AIR_POLLUTION
WHERE LOCATION2 = '수원'
GROUP BY YEAR(YM)
ORDER BY YEAR;
```


## Q3. 물고기 종류 별 잡은 수 구하기
> GROUP BY

#### 날짜: 0128

### 문제설명
낚시앱에서 사용하는 FISH_INFO 테이블은 잡은 물고기들의 정보를 담고 있습니다. FISH_INFO 테이블의 구조는 다음과 같으며 ID, FISH_TYPE, LENGTH, TIME은 각각 잡은 물고기의 ID, 물고기의 종류(숫자), 잡은 물고기의 길이(cm), 물고기를 잡은 날짜를 나타냅니다. 단, 잡은 물고기의 길이가 10cm 이하일 경우에는 LENGTH 가 NULL 이며, LENGTH 에 NULL 만 있는 경우는 없습니다.

FISH_NAME_INFO 테이블은 물고기의 이름에 대한 정보를 담고 있습니다. FISH_NAME_INFO 테이블의 구조는 다음과 같으며, FISH_TYPE, FISH_NAME 은 각각 물고기의 종류(숫자), 물고기의 이름(문자) 입니다.

### 문제
FISH_NAME_INFO에서 물고기의 종류 별 물고기의 이름과 잡은 수를 출력하는 SQL문을 작성해주세요.

물고기의 이름 컬럼명은 FISH_NAME, 잡은 수 컬럼명은 FISH_COUNT로 해주세요.
결과는 잡은 수 기준으로 내림차순 정렬해주세요.

### 정답 쿼리

GROUP BY 에 `A.FISH_TYPE` 만 사용하여 오류가 발생함.

MySQL의 ONLY_FULL_GROUP_BY 모드가 활성화된 경우, SELECT에 포함된 모든 열은 GROUP BY에 명시적으로 포함되거나, 집계 함수(COUNT, SUM, 등)로 처리되어야 한다.

이 문제의 경우 FISH_TYPE 하나 당 FISH_NAME 이 1:1 대응되는 상황이므로 GROUP BY 에 FISH_NAME 도 넣어주면 해결됨.

만일 하나의 FISH_TYPE에 여러 개의 FISH_NAME 이 대응되는 경우라면 어떤 FISH_NAME을 반환할지 MySQL이 결정할 수 없으므로 집계 함수(MAX, MIN 등)를 사용하여 특정 기준에 따라 값을 반환하거나, 모든 값을 하나로 결합(GROUP_CONCAT)해야 한다.


```sql
SELECT
    COUNT(*) AS FISH_COUNT,
    FISH_NAME
FROM FISH_INFO AS A
LEFT JOIN FISH_NAME_INFO AS B
ON A.FISH_TYPE = B.FISH_TYPE
GROUP BY A.FISH_TYPE, FISH_NAME
ORDER BY
    FISH_COUNT DESC;
```


## Q4. ROOT 아이템 구하기
> IS NULL

#### 날짜: 0128

### 문제설명
어느 한 게임에서 사용되는 아이템들은 업그레이드가 가능합니다.
PARENT 아이템이 없는 아이템을 ROOT 아이템이라고 합니다.

다음은 해당 게임에서 사용되는 아이템 정보를 담은 `ITEM_INFO` 테이블과 아이템 관계를 나타낸 `ITEM_TREE` 테이블입니다.

`ITEM_INFO` 테이블은 다음과 같으며, ITEM_ID, ITEM_NAME, RARITY, PRICE는 각각 아이템 ID, 아이템 명, 아이템의 희귀도, 아이템의 가격을 나타냅니다.

`ITEM_TREE` 테이블은 다음과 같으며, ITEM_ID, PARENT_ITEM_ID는 각각 아이템 ID, PARENT 아이템의 ID를 나타냅니다. 단, 각 아이템들은 오직 하나의 PARENT 아이템 ID를 가지며, ROOT 아이템의 PARENT 아이템 ID는 NULL 입니다. ROOT 아이템이 없는 경우는 존재하지 않습니다.

### 문제
ROOT 아이템을 찾아 아이템 ID(ITEM_ID), 아이템 명(ITEM_NAME)을 출력하는 SQL문을 작성해 주세요. 이때, 결과는 아이템 ID를 기준으로 오름차순 정렬해 주세요.

### 정답 쿼리

```sql
SELECT
    A.ITEM_ID,
    ITEM_NAME
FROM ITEM_INFO AS A
LEFT JOIN ITEM_TREE AS B
ON A.ITEM_ID = B.ITEM_ID
WHERE PARENT_ITEM_ID IS NULL
ORDER BY ITEM_ID;
```


## Q5. 월별 잡은 물고기 수 구하기
> MONTH, GROUP BY

#### 날짜: 0128

### 문제설명
낚시앱에서 사용하는 FISH_INFO 테이블은 잡은 물고기들의 정보를 담고 있습니다. FISH_INFO 테이블의 구조는 다음과 같으며 ID, FISH_TYPE, LENGTH, TIME은 각각 잡은 물고기의 ID, 물고기의 종류(숫자), 잡은 물고기의 길이(cm), 물고기를 잡은 날짜를 나타냅니다.

단, 잡은 물고기의 길이가 10cm 이하일 경우에는 LENGTH 가 NULL 이며, LENGTH 에 NULL 만 있는 경우는 없습니다.

### 문제
월별 잡은 물고기의 수와 월을 출력하는 SQL문을 작성해주세요.

잡은 물고기 수 컬럼명은 FISH_COUNT, 월 컬럼명은 MONTH로 해주세요.   
결과는 월을 기준으로 오름차순 정렬해주세요.   
단, 월은 숫자형태 (1~12) 로 출력하며 9 이하의 숫자는 두 자리로 출력하지 않습니다. 잡은 물고기가 없는 월은 출력하지 않습니다.

### 정답 쿼리

```sql
SELECT
    COUNT(*) AS FISH_COUNT,
    MONTH(TIME) AS MONTH
FROM FISH_INFO
GROUP BY MONTH(TIME)
ORDER BY MONTH;
```


## Q6. 조건에 맞는 사원 정보 조회하기
> 서브쿼리

#### 날짜: 0128

### 문제설명
`HR_DEPARTMENT` 테이블은 회사의 부서 정보를 담은 테이블입니다. HR_DEPARTMENT 테이블의 구조는 다음과 같으며 DEPT_ID, DEPT_NAME_KR, DEPT_NAME_EN, LOCATION은 각각 부서 ID, 국문 부서명, 영문 부서명, 부서 위치를 의미합니다.

`HR_EMPLOYEES` 테이블은 회사의 사원 정보를 담은 테이블입니다. HR_EMPLOYEES 테이블의 구조는 다음과 같으며 EMP_NO, EMP_NAME, DEPT_ID, POSITION, EMAIL, COMP_TEL, HIRE_DATE, SAL은 각각 사번, 성명, 부서 ID, 직책, 이메일, 전화번호, 입사일, 연봉을 의미합니다.

`HR_GRADE` 테이블은 2022년 사원의 평가 정보를 담은 테이블입니다. HR_GRADE의 구조는 다음과 같으며 EMP_NO, YEAR, HALF_YEAR, SCORE는 각각 사번, 연도, 반기, 평가 점수를 의미합니다.

### 문제
HR_DEPARTMENT, HR_EMPLOYEES, HR_GRADE 테이블에서 2022년도 한해 평가 점수가 가장 높은 사원 정보를 조회하려 합니다. 2022년도 평가 점수가 가장 높은 사원들의 점수, 사번, 성명, 직책, 이메일을 조회하는 SQL문을 작성해주세요.

2022년도의 평가 점수는 상,하반기 점수의 합을 의미하고, 평가 점수를 나타내는 컬럼의 이름은 SCORE로 해주세요.

### 정답 쿼리

```sql
SELECT
    SCORE,
    C.EMP_NO,
    EMP_NAME,
    POSITION,
    EMAIL
FROM HR_EMPLOYEES AS B
LEFT JOIN (
    SELECT
        EMP_NO,
        SUM(SCORE) AS SCORE
    FROM HR_GRADE
    GROUP BY EMP_NO) AS C
ON B.EMP_NO = C.EMP_NO
ORDER BY
    SCORE DESC
LIMIT 1;
```

### 문제 풀이 과정

#### 1. SCORE은 상반기와 하반기의 점수 합으로 나타내야 한다. 따라서 `HR_GRADE` 테이블을 사용하여 필요에 맞게 변형한다.

```sql
SELECT
    EMP_NO,
    SUM(SCORE) AS SCORE
FROM HR_GRADE
GROUP BY EMP_NO
```
![결과](/2025_W/img/4-1.PNG)

#### 2. 1번에서 만든 테이블을 C로 별칭을 지정하여 활용

```sql
SELECT
    SCORE,
    C.EMP_NO,
    EMP_NAME,
    POSITION,
    EMAIL
FROM HR_EMPLOYEES AS B
LEFT JOIN (
    SELECT
        EMP_NO,
        SUM(SCORE) AS SCORE
    FROM HR_GRADE
    GROUP BY EMP_NO) AS C
ON B.EMP_NO = C.EMP_NO
ORDER BY
    SCORE DESC
LIMIT 1;
```


## Q7. 재구매가 일어난 상품과 회원 리스트 구하기
> GROUP BY, HAVING

#### 날짜: 0128

### 문제설명
다음은 어느 의류 쇼핑몰의 온라인 상품 판매 정보를 담은 ONLINE_SALE 테이블 입니다. ONLINE_SALE 테이블은 아래와 같은 구조로 되어있으며 ONLINE_SALE_ID, USER_ID, PRODUCT_ID, SALES_AMOUNT, SALES_DATE는 각각 온라인 상품 판매 ID, 회원 ID, 상품 ID, 판매량, 판매일을 나타냅니다.

동일한 날짜, 회원 ID, 상품 ID 조합에 대해서는 하나의 판매 데이터만 존재합니다.

### 문제
ONLINE_SALE 테이블에서 동일한 회원이 동일한 상품을 재구매한 데이터를 구하여, 재구매한 회원 ID와 재구매한 상품 ID를 출력하는 SQL문을 작성해주세요. 결과는 회원 ID를 기준으로 오름차순 정렬해주시고 회원 ID가 같다면 상품 ID를 기준으로 내림차순 정렬해주세요.

### 정답 쿼리

```sql
SELECT
    USER_ID,
    PRODUCT_ID
FROM ONLINE_SALE
GROUP BY USER_ID, PRODUCT_ID
HAVING COUNT(*) > 1
ORDER BY
    USER_ID,
    PRODUCT_ID DESC;
```


## Q8. 특정 물고기를 잡은 총 수 구하기
> IN

#### 날짜: 0128

### 문제설명
낚시앱에서 사용하는 FISH_INFO 테이블은 잡은 물고기들의 정보를 담고 있습니다. FISH_INFO 테이블의 구조는 다음과 같으며 ID, FISH_TYPE, LENGTH, TIME은 각각 잡은 물고기의 ID, 물고기의 종류(숫자), 잡은 물고기의 길이(cm), 물고기를 잡은 날짜를 나타냅니다.

단, 잡은 물고기의 길이가 10cm 이하일 경우에는 LENGTH 가 NULL 이며, LENGTH 에 NULL 만 있는 경우는 없습니다.

FISH_NAME_INFO 테이블은 물고기의 이름에 대한 정보를 담고 있습니다. FISH_NAME_INFO 테이블의 구조는 다음과 같으며, FISH_TYPE, FISH_NAME 은 각각 물고기의 종류(숫자), 물고기의 이름(문자) 입니다.

### 문제
FISH_INFO 테이블에서 잡은 BASS와 SNAPPER의 수를 출력하는 SQL 문을 작성해주세요. 컬럼명은 'FISH_COUNT`로 해주세요.

### 정답 쿼리

```sql
SELECT
    COUNT(*) AS FISH_COUNT
FROM FISH_INFO AS A
LEFT JOIN FISH_NAME_INFO AS B
ON A.FISH_TYPE = B.FISH_TYPE
WHERE B.FISH_NAME IN ('BASS', 'SNAPPER')
```


## Q9. 자동차 평균 대여 기간 구하기
> 

#### 날짜: 0128

### 문제설명
다음은 어느 자동차 대여 회사의 자동차 대여 기록 정보를 담은 CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블입니다. CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블은 아래와 같은 구조로 되어있으며, HISTORY_ID, CAR_ID, START_DATE, END_DATE 는 각각 자동차 대여 기록 ID, 자동차 ID, 대여 시작일, 대여 종료일을 나타냅니다.

### 문제
CAR_RENTAL_COMPANY_RENTAL_HISTORY 테이블에서 평균 대여 기간이 7일 이상인 자동차들의 자동차 ID와 평균 대여 기간(컬럼명: AVERAGE_DURATION) 리스트를 출력하는 SQL문을 작성해주세요. 평균 대여 기간은 소수점 두번째 자리에서 반올림하고, 결과는 평균 대여 기간을 기준으로 내림차순 정렬해주시고, 평균 대여 기간이 같으면 자동차 ID를 기준으로 내림차순 정렬해주세요.

### 정답 쿼리

DATEDIFF값에 +1을 해주어야 함.

```sql
SELECT
    CAR_ID,
    ROUND(AVG(DATEDIFF(END_DATE, START_DATE)+1),1) AS AVERAGE_DURATION
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
HAVING AVG(DATEDIFF(END_DATE, START_DATE)+1) >= 7
ORDER BY
    AVERAGE_DURATION DESC,
    CAR_ID DESC;
```

[DATEDIFF만 적용한 경우 나오는 결과]

+1을 추가해야 함을 알 수 있음

```SQL
SELECT
    *,
    DATEDIFF(END_DATE, START_DATE) AS DIFF
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
ORDER BY DIFF;
```
![결과](/2025_W/img/4-2.PNG)
