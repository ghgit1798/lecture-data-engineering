프로그래머스 데이터 엔지니어링 강의를 수강하며 학습한 내용을 정리하는 저장소입니다.
# ⭐ 강의 요약

## 1️⃣ 주차 강의 내용

지난 1주차 강의 내용을 간략히 정리해보자면 다음과 같다.

1. **새로운 분야를 학습하는 태도**
    1. 남과 비교하지 않되, 나도 학습하면 저렇게 발전해나갈 수 있다라는 마인드를 갖자
    2. 무엇을 모르는 지 정의하고, 조사하여 정리한다.
    3. 작은 성공을 반복해서 자신감을 높이자.
2. **Agile 방법론에 익숙해질 것**
    1. 빠르게 요구조건 파악 후 반영할 것
    2. 짧은 사이클을 빠르고 반복적으로 구현하는 것 중요
3. **데이터 팀과 데이터 엔지니어의 가치**
    1. 바람직한 데이터 팀 구조는 무엇일까?
        1. Centralized, Uncentrialized, **Hybrid**
    2. 일의 성공과 실패를 어떻게 측정할 것이냐?
        1. ⭐ 본인의 **성공을 입증할 수 있는 지표** 설정
        2. ⭐ A/B Test를 먼저 설계 후 개선된 결과 제시
        3. ⭐ ex) 데이터 수집 중 fail한 Percent가 어떻게 변하는가? 마케터들의 Mood 관리 등등

## 2️⃣ 주차 강의 내용

2주차의 주요 내용은 크게 3가지였다.

1. **데이터 엔지니어의 한 주**
    1. 주 초엔 저번 주차를 피드백
        1. 잘 된 작업은 무엇인가?
        2. 더 잘할 수 있었던 작업은 무엇인가?
    2. On-Call 엔지니어 지정
        1. 한 주간 data pipeline 실패 관련 이슈 해결
        2. 100%가 아닌 가장 중요한 20%를 반드시 성공
        3. 상시 모니터링, 데이터 엔지니어링 == 노가다?
    3. 데이터 파이프라인 개발 최적화
        1. Metrics & Quarterly Goals 리뷰
        2. 30-40% 시간은 인프라 코드의 refactoring에 사용
2. **클라우드 컴퓨팅의 장단점과 주의사항**
    1. 장점
        1. 초기 투자 비용 감소
        2. 리소스 준비를 위한 대기시간 감소
        3. 사용한만큼 금액 지불
    2. 단점
        1. Variable Cost Option의 경우 요금 폭탄
        2. 더욱 신중한 SQL 작성 요구
        3. 연간 비용 → 월간 비용
3. **Database best practice**
    1. 사내 데이터 공개 정책 숙지
    2. 신중한 SQL 쿼리 작성 필요
    3. 초기엔 Consistency를 유지하는 것이 중요
        1. **Naming convetion의 중요성**
    4. DB는 마스터/슬레이브 아키텍처 적용

## 3️⃣ 주차 강의 내용

3주차의 강의 내용은 SQL 활용에 대한 내용이었고, 실습위주로 진행되었다.
1. **DDL/DML 기본 문법**
    1. SELECT/UPDATE/INSERT/DELETE
    2. CREATE/DROP/RENAME
    3. WHERE, CASE WHEN, GROUP BY, ORDER BY
    4. ROW NUMBER() OVER(), LEFT, TO_CHAR, CTAS
2. **중복 레코드 확인하기**
    1. 📌 Count 함수
        1. COUNT(1), COUNT(value), COUNT(DISTINCT value) 차이❗
        2. COUNT(1)은 NULL이 포함된 행도 COUNT
        3. COUNT(value)는 NULL 제외
        4. COUNT(DISTINCT value)는 중복된 value 제외
    2. SELECT DISTINCT
3. **NULL 확인하기**
    1. IS NULL/IS NOT NULL
    2. Boolean과 NULL의 차이
        1. IS NOT TRUE ↔ True가 아닌 것, False와 NULL 모두 해당
        2. IS NOT FALSE ↔ False가 아닌 것, True와 NULL 모두 해당
    3. NULL과의 사칙연산 결과는 모두 NULL
        1. 값이 존재하지 않음을 의미하므로!
4. ⭐ **DW에서는 Primary Key를 지정해도 Uniqueness를 보장하지 않는다.**
5. **JOIN**
    1. INNER/LEFT/RIGHT/SELF/FULL/CROSS JOIN
6. **DELETE FROM과 TRUNCATE의 차이❓**
    1. DELETE FROM
        1. DELETE FROM은 테이블의 모든 레코드를 삭제한다.
        2. 단, 테이블은 남아있다는 점이 DROP TABLE과 다른 점이다.
        3. WHERE 사용해 특정 레코드만 삭제 가능하다.
    2. TRUNCATE
        1. DELETE FROM과 마찬가지로 테이블의 모든 레코드를 삭제한다.
        2. DELETE FROM에 비해 삭제 속도가 빠르다.
        3. 하지만, TRUNCATE는 WHERE를 지원하지 않으며, Transaction 또한 지원하지 않는다.
7. **COALESCE와 NULLIF**
    1. COALESCE(value, 1)
        1. value가 NULL인 경우 1로 대체할 수 있다.
    2. NULLIF(value, 0)
        1. value가 0이면 NULL을 리턴한다.
8. **UNION과 UNION ALL 차이점**
    1. UNION
        1. 합집합으로, 중복을 제거한다.
    2. UNION ALL
        1. 중복을 허용한 합집합이다.

위 내용을 바탕으로 3가지 Assingment를 수행했고, 받은 피드백을 바탕으로 SQL을 익숙하게 사용할 수 있게 되었다.

## 4️⃣ 주차 강의 내용

3주차 Assignment 리뷰와 Airflow에 대한 학습을 진행했다.

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

## 5️⃣ 주차 강의 내용
- Airflow 시 혼란을 겪는 부분을 중점적으로 학습하였다.

## ⭐ Autocommit 설정

1. **autocommit = False일 때**
    1. 내가 명시적으로 commit을 하기 전까지는 나에게만 그 변화가 보인다. 다른 사람들에게는 그 변화가 보이지 않는다.
    2. 예를 들어, 테이블을 삭제하고 조회하면 테이블이 없는 것으로 나온다. 하지만 다른 사람들에게는 여전히 테이블이 있는 것으로 조회가 됨
    3. PostgresHook은 현재 autocommit = False를 반환함
2. **autocommit = False일 때 주의할 점**
    1. 쿼리문 실행 후 파이썬 try ~ exception와 commit;을 함께 사용하는 것이 best practice
    2. try ~ exception 시 ETL 운영상 관점에서 raise를 사용하는 것이 좋음
3. **autocommit = True 일 때 주의할 점**
    1. try ~ exception ~ finally과 BEGIN; END; 로 트랜잭션 처리
        1. 테이블 삭제와 삽입을 실행할 때
    2. 일반적으로 autocommit = True를 사용
4. **transaction 중간에 에러가 발생할 때 주의할 점**
    1. exception 처리 시 ROLLBACK;을 수행해줘야함

## ⭐ Start_date 이해하기

1. 실제 실행 시점은 start_date + interval이다.
2. 데이터 기준 start_date으로 execution_date을 의미하는 것이 아니다.
3. 보통 back fill을 유용하게 하기 위해 execution_date 변수를 생성한다.
4. **execution_date은 현재 시점보다 이전 시간**이 나온다.
    1. execution_date은 현 시점 - duration으로 설정해야 back fill이 쉽다.
    2. full refresh할 경우 back fill이 아무 의미 없다.
    3. 현재 시점으로 execution_date을 설정하지 않아야 한다.

## ⭐ DELETE FROM과 TRUNCATE

1. 공통점
    1. 테이블 내용을 삭제한다는 일은 정확히 똑같다.
2. 차이점
    1. DELETE FROM
        1. WHERE절로 삭제할 행을 지정할 수 있다.
        2. TRUNCATE보다 속도가 약간 느리다.
    2. TRUNCATE
        1. 전체를 한 번에 삭제하며 바로 COMMIT한다.
        2. 확실한 상황에서만 사용해야 한다.
        3. 속도가 빠르다.

## ⭐ Terminal 실행하기

- airflow tasks test my_first_dag print_hello {execution}
- airflow tasks run my_first_dag print_hello  {execution}
- execution_date은 과거 시점이어야 동작함
- test 명령어로 돌리면 실제 반영되지 않음
- airflow 실행은 터미널로 하는 것이 디버깅하기 쉬움
