## ⭐ 커리어 이야기

### Assignment Review
![image](https://user-images.githubusercontent.com/44918665/131233517-fe7ae300-b3c7-4ae2-8cf0-84aad55be7cf.png)


**❤ 느낀점**

1.  단순 지식 습득이 아닌, 매주 커리어에 관한 이야기를 함께 해줘서 좋았다. 어떻게 좋은 개발자로 성장해 나갈 수 있느냐, 직장을 고를 때 기준은 어떻게 고르냐?에 대한 이야기가 도움이 많이 됬다.
2. 목표: 우수 수강생으로 선발되기❗
    1. 매주 첫번째로 과제를 제출하고, 수업 내용을 복습한 뒤 질문을 올려야겠다.
3. DataLake, Dataware house란❓
    1. 소중한 회사 정보가 모이는 보물창고와 같다.
    2. 가치가 증명되지 않은 보석, 원석들을 유실없이 잘 적재하는 것이 엔지니어 역할이다.
    3. 그 원석들의 가치를 발견하고 증명해내는 것이 사이언티스트의 역할이다.

**💙 복습**

- 내 쿼리문

```sql
%%sql

CREATE TABLE ghgoo1798.monthly_active_user_summary AS
SELECT A.month, A.channel, A.uniqueUsers, B.paidUsers, ROUND(convert(float, B.paidUsers)/A.uniqueUsers*100, 2) AS conversionRate, grossRevenue, netRevenue
FROM (
  SELECT TO_CHAR(ts, 'YYYY-MM') AS month, channel, COUNT(DISTINCT userid) uniqueUsers
  FROM raw_data.session_timestamp A LEFT JOIN raw_data.user_session_channel B ON A.sessionid=B.sessionid
  GROUP BY month, channel
) AS A JOIN (
  SELECT TO_CHAR(ts, 'YYYY-MM') AS month, channel, COUNT(DISTINCT userid) paidUsers, SUM(amount) grossRevenue
  FROM raw_data.session_transaction C JOIN raw_data.user_session_channel B ON B.sessionid=C.sessionid JOIN raw_data.session_timestamp A ON B.sessionid=A.sessionid
  GROUP BY month, channel
) AS B ON A.month=B.month AND A.channel=B.channel LEFT JOIN
(
  SELECT TO_CHAR(ts, 'YYYY-MM') AS month, channel, SUM(amount) netRevenue
  FROM raw_data.session_transaction C JOIN raw_data.user_session_channel B ON B.sessionid=C.sessionid JOIN raw_data.session_timestamp A ON B.sessionid=A.sessionid
  WHERE refunded is False
  GROUP BY month, channel
) AS C ON A.month=C.month AND A.channel=C.channel
ORDER BY month, channel;
```

- Best Practice

```sql
%%sql

SELECT TO_CHAR(ts, 'YYYY-MM') AS month, channel, 
      COUNT(DISTINCT userid) uniqueUsers,
      COUNT(DISTINCT CASE WHEN amount is not NULL THEN userid END) paidUsers,
      paidUsers,
      ROUND(convert(float, paidUsers)/uniqueUsers*100, 2) AS conversionRate,
      SUM(amount) grossRevenue,
      SUM(CASE WHEN refunded is False THEN amount END) netRevenue
FROM raw_data.session_timestamp A 
JOIN raw_data.user_session_channel B ON A.sessionid=B.sessionid
LEFT JOIN raw_data.session_transaction C ON B.sessionid=C.sessionid
GROUP BY 1, 2
ORDER BY 1, 2;
```

1. SUM, COUNT 같은 Aggregation 함수 안에서 CASE WHEN 구문 사용이 가능하다.
2. 집계함수 안에서 CASE WHEN을 사용할 수 있는지 몰라서 고생했다..
3. Base가 되는 테이블을 잘 설정하는 것이 매우 중요하다!
4. Type 선언 시 paidUsers::float , uniqueUsers::float로 설정이 가능하다.
5. 한 번에 시도하기보다, 하나씩 Select해가며 테스트하자.
    1. **Unit Test 원리**
    2. 미리 하나씩 Select한 후 정답을 띄워 놓고, 내가 원하는 결과가 잘 나오고 있는지 확인

### 💎 IT 개발자로 오래 일하기

1. 최신 기술 따라가기보다는 내가 맡은 업무를 잘 할 수 있는 일 찾기
2. 한 방에 끝내려고 하지 않기
    1. Waterfall이 아닌, Agile 시대
3. 자기 검열하지 않기
    1. 일단 이력서 무조건 내기
    2. 신입도 뽑을 수 있지만, 공고에 내기 좀 그럼..

### 💎 One way door vs. Two way door

문 열고 나가면 끝인 것 vs 문을 다시 열고 돌아올 수 있는 것

1. 조금 위험해 보이지만 재미있어 보이는 일이 있다면 그걸 조금 더 안전하게 만들 방법을 찾아보자.
2. 시도해보고, 다시 돌아올 수 있느냐?!
    1. One-way door라면 다시 debate...
    2. Two-way door라면? 일단 빠르게 가보고, 원래대로 돌아오자
    3. 기존 회사에서 일을 잘해 놓았다면, 충분히 다시 돌아올 수 있음
        1. 요즘같이 사람 뽑기 어려운 세상 + 매니저라면 기분은 조금...
3. 대부분 하고 있은 게 먼저 있고, 그 대답에 대한 안전한 답을 듣고 싶어한다.
4. 📌 Max에게 상담 시 물어보자

### 💎 매니저 입장에서 주니어에게 바라는 점

1. 피드백을 잘 받아들이고 개선하려고 하느냐?

    → 배우는 속도를 본다.

2. 얼마나 발전 가능성이 있느냐?
    1. 처음에는 기술 습득(Skillset) 중심에서 나중에는 영향력 (Leadership)
3. 얼마나 긍정적인가?

    → 📌 매니저 관점에서 이를 어떻게 안전하게 검증할 수 있을까?

    - 인턴으로 미리 경험해 보는 것: 잘하면 정규직으로 전환
4. 회사를 고르기 보다는 같이 일하는 사람들과 매니저가 더 중요!
    1. 특히 커리어 초창기에 더 중요
    2. 네트워크 형성에 큰 영향을 끼침
    3. 좋은 매니저 밑에서 따라하는 건 쉬움. 나쁜 매니저 밑에선 어려움.

## ⭐ 학습내용

### ETL vs ELT

데이터 엔지니어 채용 공고 시 자주 나오는 ETL과 ELT의 차이에 대해 배웠다. 

- ETL : 데이터 추출 후 가공한 뒤 저장하는 것
- ELT : 데이터 추출 후 저장을 먼저 한 뒤 가공하는 것

### Data Lake

따라서 함께 나오는 개념이 Data Lake였다. Dataware house보다 더 큰 개념이다. AWS의 S3도 Data Lake로 정형/비정형 데이터를 함께 적재할 수 있고, 저장공간의 제약이 없다. 반면 Redshift, BigQuery, Snowflake등은 비용이 더 들어간다.

### Airflow 구성요소

1. Web Server (Flask)
2. Scheduler
3. Worker
4. Database
5. Queue (Executor)

### Scale Up vs Scale Out

1. Scale Up : 더 고성능의 서버로 확장하는 것
    1. 주로 Production DB는 Scaler Out이 어렵기 때문에 Scale Up 방식 채택
2. Scale Out : 여러 대의 서버를 확장하는 것
    1. 반면, DataWare house는 Scaler Out이 가능함

### Airflow의 단위

- DAG (Directed Acyclic Graph)

방향이 있고 순환하지 않는 그래프로 Airflow에서 DAG는 Task들로 구성된다.

- DAG example

```python
from datetime import datetime, timedelta
from airflow import DAG
default_args = {
	'owner' : 'ghgoo1798',
	'start_date' : datetime(2020, 8, 7, hour=0, minute=00),
	'end_date' : datetime(2020, 8, 31, hour=23, minute=00),
	'email' : ['ghgoo1798@gmail.com'],
	'retries' : 1,
	'retry_delay': timedelta(minutes=3),
 }
```

1. start_date 조심하기
    1. start_date은 데이터 기준 시점이다. 주기가 1일이라면 8월7일 데이터는 8월 8일에 수집된다.
2. catchup 옵션 조심하기
    1. chatchup이 True라면, DAG가 활성화되었고, 과거에 실행되지 않은 Job이 있다면?
    2. 현재 시점까지의 모든 Job이 연쇄적으로 실행된다.
