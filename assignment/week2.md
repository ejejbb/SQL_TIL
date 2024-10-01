# 2-3. 데이터 탐색(SELECT -> FROM -> WHERE)

## SELECT
테이블의 어떤 컬럼을 선택(출력)할 것인가?

포켓몬 여행을 시작할 때 함께할 포켓몬을 선택하기
<img width="402" alt="포켓몬select" src="https://github.com/user-attachments/assets/6e959b87-f23b-4e82-85d1-42222c066d2a">

## FROM
Dataset.Table : 어떤 테이블에서 데이터를 확인할 것인가?

## WHERE
원하는 조건이 있다면, 어떤 조건인가?
`Col1=1:조건문` 형태로 작성

## 실습하기
실제 sql 실습 시, 다음 쿼리를 작성하여 **원하는 포켓몬**을 선택할 수 있다.
```
*
FROM basic.pokemon
WHERE
  type1="Fire"
```
'*'의 의미: 모든 컬럼을 출력하겠다.

<img width="350" alt="설명1" src="https://github.com/user-attachments/assets/c1d7a552-c71b-4490-84f5-62888c11c149">

<img width="322" alt="설명2" src="https://github.com/user-attachments/assets/e209e056-eb20-4331-8000-6952493c7b72">

**정리**
<img width="440" alt="정리" src="https://github.com/user-attachments/assets/d9f4db60-d363-4444-a549-4957abae8ab0">



# 2-4. SELECT 연습문제
1) trainer 테이블에 있는 모든 데이터를 보여주는 SQL 쿼리 작성하기
<img width="369" alt="문젭" src="https://github.com/user-attachments/assets/e51ba4bb-a7fe-4a8a-bb40-2e8e03c72188">

2) trainer 테이블에 있는 트레이너의 name을 출력하는 쿼리 작성하기
<img width="198" alt="문제2" src="https://github.com/user-attachments/assets/48d9bfe7-ca0f-4892-84e1-d135767d9942">

3) trainer 테이블에 있는 트레이너의 name, age를 출력하는 쿼리 작성하기(2번과 달리 column이 **2개**!)
<img width="212" alt="문제3" src="https://github.com/user-attachments/assets/2ffe182b-2c62-465b-8b62-e6e2bf46a2f8">

4) trainer 테이블에서 id가 3인 트레이너의 name, age, hometown을 출력하는 쿼리를 작성해주세요.
<img width="265" alt="문제4" src="https://github.com/user-attachments/assets/47ce55ad-639e-40c9-979f-8820a73280d9">

5) pokemon 테이블에서 "피카츄"의 공격력(attack)과 체력(hp)을 확인할 수 있는 쿼리를 작성해주세요.
<img width="227" alt="문제5" src="https://github.com/user-attachments/assets/64aaddbb-1c4b-4642-9a7a-ebaf1bb4339e">



# 2-5. 집계(Group by + having + sum/count)

집계하다: 모아서(그룹화해서) 계산하다.

## GROUP BY
: 같은 값끼리 모아서 그룹화한다.
<img width="436" alt="그룹화" src="https://github.com/user-attachments/assets/29d8ce3c-389b-42b8-bdd2-fe33392d1b29">

**포켓몬 예시**
<img width="327" alt="포켓몬 그룹화" src="https://github.com/user-attachments/assets/8ed3535b-f9f3-4e1c-b6ef-23e7386813af">

### DISTINCT
: 고유값을 알고 싶은 경우 사용, `unique()`와 유사

## WHERE, HAVING
: 조건을 설정하고 싶은 경우
```
# WHERE: Table에 바로 조건을 설정하고 싶은 경우에 사용
# HAVING: GROUP BY 한 후 조건을 설정하고 싶은 경우에 사용
```

## 서브 쿼리
<img width="360" alt="서브쿼리" src="https://github.com/user-attachments/assets/606b3828-20e5-43e6-b8cd-677e090c255f">

## ORDER BY
<img width="289" alt="order by" src="https://github.com/user-attachments/assets/033d3a1d-9a00-4fd1-871c-743ae9b39065">

## LIMIT
: ORDER BY와 같이 쿼리의 마지막에 사용
<img width="310" alt="limit" src="https://github.com/user-attachments/assets/f696eb2a-2aca-4ada-ac75-268440255b41">

## 예시
1) pokemon 테이블에 있는 포켓몬 수를 구하는 쿼리를 작성해주세요.
<img width="236" alt="group by 1" src="https://github.com/user-attachments/assets/386399af-bb8a-4b95-ad0c-2f4aa18864af">

2) 포켓몬의 수가 세대별로 얼마나 있는지 알 수 있는 쿼리를 작성해주세요.
<img width="239" alt="group by 2" src="https://github.com/user-attachments/assets/c8543f42-455f-4f7a-a576-f95b0e41421a">

3) 포켓몬의 수를 타입 별로 집계하고, 포켓몬의 수가 10 이상인 타입만 남기는 쿼리를 작성해주세요. 포켓몬의 수가 많은 순으로 정렬해주세요.
<img width="434" alt="또문제" src="https://github.com/user-attachments/assets/66a7dd73-0303-4525-85a7-5a388e0fbed9">
<img width="88" alt="문제33" src="https://github.com/user-attachments/assets/b79a146f-153a-4f8f-b297-b23ad647290b">

**정리**
<img width="495" alt="정리2" src="https://github.com/user-attachments/assets/c8d078e4-6017-4b8a-a358-93358de09202">



**수강 인증**
<img width="281" alt="수강인증" src="https://github.com/user-attachments/assets/36fb75c8-e077-4e6f-8694-6ceb7d6acf2c">










