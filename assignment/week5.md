# 4-4. 날짜 및 시간 데이터 이해하기(2)(EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FORMAT_DATETIME)

### 1. DATETIME 함수
<img width="518" alt="1" src="https://github.com/user-attachments/assets/fab2a4a5-77ce-424e-bad7-30336ce282ad">   
-> UTC기준 시간과 한국 시간은 9시간 차이가 남. 어떤 시간을 출력해야할지 항상 주의할 것.

### 2. EXTRACT 함수
- DATETIME에서 특정 부분만 추출하고 싶은 경우에 사용
- 주문 날짜에서 일자별 주문, 월별 주문 등을 뽑아보고 싶을 때 사용

### 3. DATETIME_TRUNC 함수
- 시간을 자르고 싶을 때 사용
- HOUR로 자르면, 분과 초 부분이 잘림.
<img width="640" alt="2" src="https://github.com/user-attachments/assets/c8c7df74-da47-4b5b-aee9-99e7dc4e7aa2">

### 4. PARSE_DATETIME
- 문자열로 저장된 DATETIME을 DATETIME 타입으로 바꾸고 싶은 경우 사용
- 반대로: FORMAT_DATETIME 사용

### 5. LAST_DAY
- 자동으로 월의 마지막 값을 계산해서 특정 연산을 할 경우 사용
- default는 '월'이지만, '주' 기준으로 마지막 일자(토요일)를 계산할 수도 있음.

### 6. DATETIME_DIFF
- 두 DATETIME의 차이를 알고 싶은 경우 사용
- 형식: DATETIME_DIFF(첫 DATETIME,두번째 DATETIME, 궁금한 차이)
- 궁금한 차이에서는 DAY, MONTH, WEEK 등을 사용할 수 있음.
<img width="447" alt="3" src="https://github.com/user-attachments/assets/20250262-096f-4507-be73-2499139c3a4e">


# 4-5. 시간데이터 연습문제 1~2번

### Q1.트레이너가 포켓몬을 포획한 날짜(catch_date)를 기준으로, 2023년 1월에 포획한 포켓몬의 수를 계산해주세요.

#### 쿼리 작성 목표, 확인할 지표: 포켓몬의 수
#### 쿼리 계산 방법: COUNT
#### 데이터의 기간: 2023년 1월
#### 사용할 테이블: trainer_pokemon
#### Join KEY: 없음
#### 데이터 특징: 봐야함
```
# catch_date: DATE 타입
=> KR기준? UTC기준? 확인 필요!
=> catch_date와 catch_datetime을 비교
=> catch_date != DATE(DATETIME(catch_datetime, "Asia/Seoul")) 가 있다면 catch_date는 사용하기 어려울 수 있음.

# catch_datetime: datetime은 T, TIMESTAMP타입은 UTC.
                  UTC로 표시되었으므로 칼럼의 이름은 datetime이지만, TIMESTAMP 타입으로 저장되어 있음을 확인할 수 있다.
                  데이터가 잘못 저장된 경우가 있을 수 있으므로 컬럼명만 보고 쿼리를 작성해서는 안됨.
```

#### 데이터 검증을 위한 쿼리
<img width="434" alt="q1" src="https://github.com/user-attachments/assets/a7566a00-cb57-4e52-9dcd-d1b626c40282">

#### 문제 풀이
<img width="401" alt="q1-1" src="https://github.com/user-attachments/assets/01ed1585-6faa-4693-b2c3-f0856f1298c6">


### Q2. 배틀이 일어난 시간(battle_datetime)을 기준으로, 오전 6시에서 오후 6시 사이에 일어난 배틀의 수를 계산해주세요.

#### 쿼리 작성 목표, 확인할 지표: 오전 6시~ 오후 6시 사이에 일어난 배틀의 수
#### 쿼리 계산 방법: COUNT
#### 데이터의 기간: 일자는 상관없고, 오전 6시~ 오후 6시
#### 사용할 테이블: battle
#### Join KEY: 없음
#### 데이터 특징: 봐야함
```
battle_date: battle_datetime 기반으로 만들어진 DATE
battle_datetime: DATETIME
battle_timestamp: TIMESTAMP
battle_datetime과 DATETIME(battle_timestamp, "Asia/Seoul")가 같은지 확인!
```

#### 데이터 검증을 위한 쿼리
<img width="573" alt="q2-1" src="https://github.com/user-attachments/assets/cb06a123-16b9-4b19-a75b-ce02631c9165">   
-> 같으므로 battle_datetime을 사용


#### 문제 풀이
<img width="332" alt="q2-2" src="https://github.com/user-attachments/assets/96922b16-b30e-4268-8332-b17fe29d1196">   
-> 숫자는 between을 사용하여 쿼리를 짧게 작성할 수 있음.

#### 추가 문제_시간대별로 몇 건이 있는가?
<img width="287" alt="q2-3" src="https://github.com/user-attachments/assets/dd07be09-f2c6-4832-ae77-db3de1290380">



# 4-5. 시간 데이터 연습문제 3~5번

### Q3. 각 트레이너 별로 그들이 포켓몬을 포획한 첫 날(catch_date)을 찾고, 그 날짜를 'DD/MM/YYYY'형식으로 출력해주세요. (2024-01-01 => 01/01/2024)


### Q4. 배틀이 일어난 날짜(battle_date)를 기준으로, 요일 별로 배틀이 얼마나 자주 일어났는지 계산해주세요.

### Q5. 트레이너가 포켓몬을 처음으로 포획한 날짜와 마지막으로 포획한 날짜의 간격이 큰 순으로 정렬하는 쿼리를 작성해주세요.



# 4-6. 조건문(CASE WHEN, IF)





# 4-7.  조건문 연습 문제





# 4-8. 정리





# 4-9. BigQuery 공식 문서 확인하는 법




