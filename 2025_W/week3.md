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
> IFNULL 

#### 날짜: 0124

### 문제설명
ANIMAL_INS 테이블은 동물 보호소에 들어온 동물의 정보를 담은 테이블입니다. ANIMAL_INS 테이블 구조는 다음과 같으며, ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE는 각각 동물의 아이디, 생물 종, 보호 시작일, 보호 시작 시 상태, 이름, 성별 및 중성화 여부를 나타냅니다.

### 문제
ANIMAL_INS 테이블에 등록된 모든 레코드에 대해, 각 동물의 아이디와 이름, 들어온 날짜1를 조회하는 SQL문을 작성해주세요. 이때 결과는 아이디 순으로 조회해야 합니다.

### 정답 쿼리


```sql

```
