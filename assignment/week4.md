# SQL 4주차

* 문법 오류
* 데이터 타입: 문자열, 날짜 및 시간 데이터


## 3-4. 오류를 디버깅하는 방법
오류가 나타나면?   
'길잡이가 나를 더 좋은 길로 나아가게 하려는구나!'라고 생각하기🍀

### SyntaxError(문법오류)
- 문법을 지키지 않아 생기는 오류
- ErrorMessage를 보고 **번역 또는 해석**한 후, 해결방법 찾아보기
- 구글에 검색 or ChatGPT에 질문 or 지인에게 질문(커뮤니티 등)
- 밑줄이 있다면 밑줄의 위 또는 아래에서 오류가 발생했을 가능성이 높음. (위 아래를 잘 확인하기)

### 주요 문법 오류
- SELECT와 FROM 사이가 비어 있으면 안됨.
- COUNT 함수에는 1개의 인자만 들어갈 수 있음.
- SELECT list expression references column type1 a which is neither grouped nor aggregated   
  : 이 오류는 GROUP BY에 적절한 컬럼을 명시하지 않았을 경우 발생하는 오류임.
- 하나의 쿼리에는 SELECT가 1개만 있어야 한다. 2개 이상의 SELECT문을 사용하고 싶다면, 쿼리가 끝나는 부분에 ;을 붙여서 두 개 모두 실행하거나, 실행할 부분만 드래그 앤 드랍 해서 실행하기
- LIMIT은 맨 아래에 입력되어야 함.
- Expected ")" but got end of script at [8:11]   
  : 괄호 열기는 했지만 닫기를 입력하지 않은 경우


## 4-2. 데이터 타입과 데이터 변환
### 기본적인 데이터 타입
=> 숫자, 문자, 시간.날짜, 부울 등
```
#숫자: 1, 2, 3, 3.14 등
#문자: “나”, “데이터” 등
#시간.날짜: 2024-01-01, 2024-01-01 23:59:10 등
#부울: 참/거짓
```

### 데이터 타입이 중요한 이유
=> 보이는 것과 저장된 것에 차이가 존재할 수 있기 때문
- 엑셀에서 보면 빈 값 => “”일 수도 있고, NULL일 수도 있음
- 1이라고 작성된 경우 => 숫자 1일 수도 있고, 문자 1일 수도 있음
- 2023-12-31 => DATE 2023-12-31일 수도 있고, 문자 2023-12-31일 수도 있음
- 내 생각과 다른 경우 데이터의 타입을 서로 변경해야 함(CAST함수 사용)
```
#예시: 숫자 1을 문자 1로 변경
SELECT
 CAST(1 AS STRING)
```


## 4-3. **문자열** 함수

<img width="778" alt="문자열 함수" src="https://github.com/user-attachments/assets/58db0a63-79a4-4d6c-bacc-d2d513a893c5">

### 문자열 함수 실습하기
#### 1. CONCAT

<img width="409" alt="문자열1" src="https://github.com/user-attachments/assets/e3597c33-0cc7-48e6-b32d-24bad5892ca1">

#### 2. SPLIT

<img width="261" alt="문자열2" src="https://github.com/user-attachments/assets/bc5909ce-e3e1-44b3-8426-9812483d3da4">

=> 결과가 배열(array) 타입으로 나옴.

#### 3. REPLACE

<img width="283" alt="문자열3" src="https://github.com/user-attachments/assets/5cdeebca-2322-4812-b9d0-a5bec3f1af35">

#### 4. TRIM

<img width="244" alt="문자열4" src="https://github.com/user-attachments/assets/8edc744c-26bc-4615-bd04-3459c0cec49a">

#### 5. UPPER

<img width="228" alt="문자열5" src="https://github.com/user-attachments/assets/1680a45e-64a4-4907-ab23-2ed611b81d9d">


## 4-4. **날짜 및 시간** 데이터 이해하기(1)
### 날짜 및 시간 데이터의 핵심
- 날짜 및 시간 데이터 **타입** 파악하기: *DATE*, *DATETIME*, *TIMESTAMP*
```
# DATE: DATE만 표시하는 데이터, 2023-12-31
# DATETIME: DATE와 TIME까지 표시하는 데이터, Time Zone 정보 없음, 2023-12-31 14:00:00
# TIME: 날짜와 무관하게 시간만 표시하는 데이터, 23:59:59.00
```
- 날짜 및 시간 데이터 관련 알면 좋은 내용: *UTC*, *Millisecond*
- 날짜 및 시간 데이터 **타입 변환**하기
- 시간**함수**(두 시간의 차이, 특정 부분 추출하기)
```
# 타임존
GMT: 영국의 그리니치 천문대(경도 0도)를 기준으로 지역에 따른 시간의 차이를 조정하기 위해 생긴 시간의 구분선
UTC: 국제적인 표준 시간

한국시간: GMT+9, UTC+9

TIMESTAMP: Time Zone 정보 있음, 2023-12-31 14:00:00 UTC
```
```
# millisecond, microsecond
Millisecond: 1/1,000sec
Microsecond: 1/1,000,000sec
```
**예시**: 1704176819711ms => 2024-01-02 15:26:59 (DATETIME)

<img width="510" alt="타임스탬프" src="https://github.com/user-attachments/assets/16747d57-a176-4d5e-9f65-e6298437d8ce">

- TIMESTAMP와 DATETIME 비교

<img width="687" alt="시간비교" src="https://github.com/user-attachments/assets/76c35a73-371b-4b73-bc13-51d82fbc0c28">


#### 수강 인증

<img width="345" alt="수강인증" src="https://github.com/user-attachments/assets/f93bf5c5-e90f-427f-82f9-17e6f5153ab4">
