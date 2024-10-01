# 2-6. 연습문제 1~17번

**Q1**. 포켓몬 중에 type2가 없는 포켓몬의 수를 작성하는 쿼리를 작성해주세요.

<img width="250" alt="1번 문제" src="https://github.com/user-attachments/assets/236aae42-03e3-4465-ac84-35fd56e79512">

<img width="60" alt="1번 해답" src="https://github.com/user-attachments/assets/422c9657-73bc-447a-89b9-a2c13bd27430">

<img width="158" alt="1번 추가" src="https://github.com/user-attachments/assets/779e8337-628f-4867-95c3-95ab64a92757">

**Q2**. type2가 없는 포켓몬의 type1과, type1의 포켓몬의 수를 작성하는 쿼리를 작성해주세요.
단, type1의 포켓몬 수가 큰 순으로 정렬해주세요.

<img width="312" alt="2번 문제" src="https://github.com/user-attachments/assets/0b9b9337-f548-45de-8646-6d53450e4a63">

<img width="327" alt="2번 해답" src="https://github.com/user-attachments/assets/bca9b7ea-b746-4436-a12d-386eec4e3403">

**Q3**. type2 상관없이 type1의 포켓몬 수를 알 수 있는 쿼리를 작성해주세요.

<img width="161" alt="3번 문제" src="https://github.com/user-attachments/assets/7bc5060b-9e6d-48dd-b84c-b0f7870af2a4">

<img width="77" alt="3번 해답" src="https://github.com/user-attachments/assets/e3067fc5-5d90-4cc9-a3c0-22d857279e2b">

'**DISTINCT**를 사용해야 하지 않을까?' 에 대한 대답

<img width="297" alt="3번 추가" src="https://github.com/user-attachments/assets/5610f44a-be1a-4dc7-ba84-49cf89a5ba6d">

**Q4**. 전설 여부에 따른 포켓몬 수를 알 수 있는 쿼리를 작성해주세요.

<img width="139" alt="4번 문제" src="https://github.com/user-attachments/assets/d2f5d591-5544-4bbe-acd4-d2e392ae66e4">

<img width="194" alt="4번 해답" src="https://github.com/user-attachments/assets/d0728d6e-c3d6-46e1-ba81-cc963876ee41">

**Q5**. 동명이인이 있는 이름은 무엇일까요?

<img width="196" alt="5번 문제" src="https://github.com/user-attachments/assets/85351071-9f4b-4ee7-b3d2-b7e3d4f11db9">

**HAVING** 사용하여 풀이>

<img width="139" alt="5번 해답" src="https://github.com/user-attachments/assets/15af6f96-48c0-44d6-ba2b-abe7e881f0f9">

**WHERE** 사용하여 풀이>

<img width="85" alt="5번 해답2" src="https://github.com/user-attachments/assets/577956af-bad1-48bd-8d6c-e1c8f261c16e">

=> **HAVING**을 사용하는 것이 쿼리문의 길이를 줄여주므로 HAVING을 사용해서 쿼리문을 작성하는 것을 추천!!


**Q6**. trainer 테이블에서 "Iris" 트레이너의 정보를 알 수 있는 쿼리를 작성해주세요.

<img width="178" alt="6번 문제" src="https://github.com/user-attachments/assets/bc5dad8d-734c-4a9c-b96e-524cf578a845">

<img width="53" alt="6번 해답" src="https://github.com/user-attachments/assets/fc927002-6fc0-405a-8235-385a79489b35">


**Q7**. trainer 테이블에서 "Iris", "Whitney", "Cynthia" 트레이너의 정보를 알 수 있는 쿼리를 작성해주세요.

<img width="231" alt="7번 문제" src="https://github.com/user-attachments/assets/bc39cca0-e077-46ac-9360-21c8bb8235e1">

<img width="69" alt="7번 해답" src="https://github.com/user-attachments/assets/50a50783-9a3e-4d2f-96cd-58020e19d691">

**OR**을 사용해서 작성하면 쿼리문이 너무 길어짐!!
**IN**을 사용해서 짧게 작성해보자~

<img width="207" alt="7번 추가" src="https://github.com/user-attachments/assets/adebf98e-968e-4da7-b1bb-2ea60c485ac3">


**Q8**. 전체 포켓몬 수는 얼마나 되나요?

<img width="89" alt="8번 문제" src="https://github.com/user-attachments/assets/97b2fd1a-ce07-4d54-8dfd-3074744e3e3c">

<img width="181" alt="8번 해답" src="https://github.com/user-attachments/assets/35c237c1-9c6e-4b6f-9805-503a6eb948b6">


**Q9**. 세대별로 포켓몬 수가 얼마나 되는지 알 수 있는 쿼리를 작성해주세요.

<img width="183" alt="9번 문제" src="https://github.com/user-attachments/assets/105cdfa1-c97c-4eb3-8b54-fc19ee940243">

<img width="73" alt="9번 해답" src="https://github.com/user-attachments/assets/618e44a0-6998-4f33-bf33-628c500116ee">


**Q10**. type2가 존재하는 포켓몬의 수는 얼마나 되나요?

<img width="121" alt="10번 문제" src="https://github.com/user-attachments/assets/8dc00ecc-513f-476e-9d89-2efeb30a63dc">

<img width="73" alt="10번 해답" src="https://github.com/user-attachments/assets/630faa87-de22-4d23-8450-be197baecb77">


**Q11**. type2가 있는 포켓몬 중에 제일 많은 type1은 무엇인가요?

<img width="141" alt="11번 문제" src="https://github.com/user-attachments/assets/cc3e7b77-4fea-41a2-8ff8-cc6f4cd09798">

이미 내림차순 정렬 되어 있기 때문에 'LIMIT 1'(맨 위 행)로 설정하면 됨.

<img width="73" alt="11번 해답" src="https://github.com/user-attachments/assets/5fbc727a-9f87-40d5-ac11-2ae991b2810c">


**Q12**. 단일 타입 포켓몬 중 많은 type1은 무엇일까요?

<img width="165" alt="12번 문제" src="https://github.com/user-attachments/assets/9e534b60-0f8a-4962-900e-8545eb1d7815">

<img width="75" alt="12번 해답" src="https://github.com/user-attachments/assets/9175219a-a6a3-4fa3-8f97-21bc68ab02aa">


**Q13**. 포켓몬의 이름에 "파"가 들어가는 포켓몬은 어떤 포켓몬이 있을까요?

<img width="155" alt="13번 문제" src="https://github.com/user-attachments/assets/3e492b58-d2d6-4b5e-ad12-6d6b1450cb8f">

<img width="184" alt="13번 해답" src="https://github.com/user-attachments/assets/a330d277-2a43-4e06-89df-7ec8dc8dbece">

오호! **LIKE**함수는 아주 편리하네요!


**Q14**. 뱃지가 6개 이상인 트레이너는 몇 명이 있나요?

<img width="116" alt="14번 문제" src="https://github.com/user-attachments/assets/350b8ca2-1828-4c7b-9592-a138670dcf89">

<img width="74" alt="14번 해답" src="https://github.com/user-attachments/assets/5bbb3d2a-1b96-4a67-9cc7-2e677f67d584">


**Q15**. 트레이너가 보유한 포켓몬이 가장 많은 트레이너는 누구일까요?

<img width="184" alt="15번 문제" src="https://github.com/user-attachments/assets/4aa63114-9f3b-4af2-b4c2-7f1d83a7397b">

<img width="123" alt="15번 해답" src="https://github.com/user-attachments/assets/c3c102ae-49ec-4d49-8e2d-51cff2b083b0">

포켓몬이 중복될 수 있기 때문에, DISTICT를 사용하는 것과 그렇지 않을 경우에 포켓몬 개수 차이 있음!



**Q16**. 포켓몬을 많이 풀어준 트레이너는 누구일까요?

<img width="108" alt="16번 문제" src="https://github.com/user-attachments/assets/806ea0cd-9365-49bb-889f-ab462e23f8ab">

<img width="95" alt="16번 해답" src="https://github.com/user-attachments/assets/717650ad-9079-4481-a46d-8e8bb0fd3064">


**Q17**. 트레이너 별로 풀어준 포켓몬의 비율이 20%가 넘는 포켓몬 트레이너는 누구일까요?(어려움)

<img width="177" alt="17번 문제" src="https://github.com/user-attachments/assets/4e07c53a-1679-43e2-8da7-61db1e0050a0">

<img width="176" alt="17번 해답" src="https://github.com/user-attachments/assets/17e2c810-3772-4cbd-a2d6-6764010a1f2b">


# 2-7. 정리

<img width="542" alt="정리" src="https://github.com/user-attachments/assets/f8231a30-4b06-459e-be0b-1ca656342dd2">


# 2-8. 새로운 집계 함수

**GROUP BY ALL**
컬럼을 명시하지 않아도 됨!!



**수강 인증**

<img width="317" alt="수강인증" src="https://github.com/user-attachments/assets/ef89593b-800c-480a-aa7d-aacf77b52ee6">



