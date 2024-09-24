# SQL BASIC 학습목표
* 쿼리문을 완성하는 것에 초점 맞추기
* 데이터 추출 방법 고민해보기
* 문제를 꼭 풀고 다음 강의로 넘어가기


# 1-1. Big Query 기초지식
* 데이터 저장 형태: 데이터베이스(DB)의 테이블 등에 저장

* 데이터베이스: MySQL, Oracle, PostgreSQL 등
   - OLTP거래를 하기 위해 사용되는 데이터베이스의 특징   
     ① 데이터가 무결하다(ex. 주문을 완료하거나 안하거나)
     ② 데이터의 추가나 변경(조리중->배달중->배달완료)이 많이 발생
     ③ 분석을 위해 만든 데이터베이스가 아니기 때문에 쿼리 속도가 느릴 수 있음.

* SQL이란?
 : 데이터베이스에서 데이터를 가지고 올 때 사용하는 언어

* 테이블에 저장된 데이터의 형태   
  테이블 구조: row*column   
  Row: 하나의 row가 하나의 고유한 데이터   
  Column: 각 데이터의 특정 속성 값

* OLAP와 데이터 웨어하우스(DW)의 등장
  - OLTP: 속도, 기능 부족 -> OLAP 등장: 분석을 위한 기능 제공   
  - DW: 데이터를 한 곳에 모아서 저장하는 일종의 창고 역할

* BigQuery란?   
 : google cloud의 OLAP+DW   
  - 장점   
  ① 난이도가 쉬움   
  ② OLAP도구이므로, 속도 빠름   
  ③ 앱 개발에 사용되는 도구(Firebase, Google Analytics 4)의 데이터를 쉽게 추출 가능(별도 로깅 필요)   
  ④ DW를 사용하기 위해 별도의 서버를 띄울 필요 없음   
  ⑤ 구글에서 인프라 관리

# 1-2. BigQuery 환경 설정
* BigQuery의 환경 구성 요소(포함관계 알기)   
 1)프로젝트: 하나의 프로젝트에 여러 데이터셋이 존재할 수 있음   
 2)데이터셋: 프로젝트 안에 있는 창고. 하나의 데이터셋에 다양한 테이블이 존재할 수 있음.   
 3)테이블: 상품의 세부 정보가 저장. 행과 열로 이루어진 데이터들이 저장됨.   

# 2-1. 데이터 활용 Overview
* 데이터를 활용하는 과정
<img width="391" alt="w1_sql_캡처1" src="https://github.com/user-attachments/assets/d14b653e-5680-44cc-983a-deec03cf4ac3">


# 2-2. 저장된 데이터 확인하기
* 데이터 확인하기   
1) ERD
<img width="446" alt="w1_sql_캡처2" src="https://github.com/user-attachments/assets/2cdb6c3f-1acf-4808-92ee-ba5a2a6f8862">
2) 모든 데이터베이스 직접 보면서 탐색
<img width="318" alt="w1_sql_캡처3" src="https://github.com/user-attachments/assets/69d7d9ea-aaf4-4456-9b57-210c37342465">


* 데이터 예시
1) 서비스 데이터베이스 – “결과”
2) 앱/웹 로그 데이터 – “과정”
3) 공공 데이터(날씨), 서드파티 데이터(페이스북 광고)

* 포켓몬으로 이해하는 SQL
  - 포켓몬 데이터 확인하기   
  : 포켓몬/트레이너/트레이너가 잡은 포켓몬/트레이너가 도전한 유저 배틀/트레이너가 도전한 체육관 배틀/NPC/상점/상점 별로 판매하는 제품
  - 포켓몬 데이터로 보는 이커머스 데이터
    <img width="247" alt="w1_sql_캡처4" src="https://github.com/user-attachments/assets/bf76942c-c212-4513-8640-7d61bbba5d8e">
  - 포켓몬 데이터 ERD
    <img width="544" alt="w1_sql_캡처5" src="https://github.com/user-attachments/assets/d2202a45-994c-4106-bdf5-de438d5d80a0">


**활동 인증**
<img width="361" alt="w1_sql_캡처6" src="https://github.com/user-attachments/assets/f85a91d8-2aa8-4e31-b2e1-0824ab4782a2">
