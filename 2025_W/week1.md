# First Study Week

## Q1. 잡은 물고기 중 가장 큰 물고기의 길이 구하기
> SUM, MIN, MAX 사용

#### [문제1_링크](https://school.programmers.co.kr/learn/courses/30/lessons/298515)

#### 날짜: 0111

### 문제 설명
낚시앱에서 사용하는 FISH_INFO 테이블은 잡은 물고기들의 정보를 담고 있습니다. FISH_INFO 테이블의 구조는 다음과 같으며 ID, FISH_TYPE, LENGTH, TIME은 각각 잡은 물고기의 ID, 물고기의 종류(숫자), 잡은 물고기의 길이(cm), 물고기를 잡은 날짜를 나타냅니다.

단, 잡은 물고기의 길이가 10cm 이하일 경우에는
LENGTH가 NULL 이며, LENGTH 에 NULL 만 있는 경우는 없습니다.

### 문제
FISH_INFO 테이블에서 잡은 물고기 중 가장 큰 물고기의 길이를 'cm' 를 붙여 출력하는 SQL 문을 작성해주세요.

이 때 컬럼명은 'MAX_LENGTH' 로 지정해주세요.

### 정답 쿼리
```sql
SELECT
    CONCAT(MAX(LENGTH), "cm") AS MAX_LENGTH
FROM FISH_INFO
```

### 문제 풀이 과정

#### 1. 컬럼명을 'MAX_LENGTH' 로 지정하여 가장 큰 물고기의 길이를 출력

```sql
SELECT
    MAX(LENGTH) AS MAX_LENGTH
FROM FISH_INFO
```

#### 2. "cm"를 붙여서 출력

```sql
SELECT
    CONCAT(MAX(LENGTH), "cm") AS MAX_LENGTH
FROM FISH_INFO
```

#### 2-1. 문자열 데이터로 할 수 있는 대표적인 연산
![a](././img/1.1.PNG)



## Q2. 잡은 물고기의 평균 길이 구하기
> IS NULL 사용

#### [문제2_링크](https://school.programmers.co.kr/learn/courses/30/lessons/293259)

#### 날짜:

### 문제 설명


### 문제


## Q3-1. 대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기
> Group By 사용

#### [문제3.1_링크](https://school.programmers.co.kr/learn/courses/30/lessons/151139)

#### 날짜:

### 문제 설명


### 문제


## Q3-2. 특정 옵션이 포함된 자동차 리스트 구하기
> String, Date 사용

#### [문제3.2_링크](https://school.programmers.co.kr/learn/courses/30/lessons/157343)

#### 날짜:

### 문제 설명


### 문제