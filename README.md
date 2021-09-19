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

### Autocommit 설정

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

### Start_date 이해하기

1. 실제 실행 시점은 start_date + interval이다.
2. 데이터 기준 start_date으로 execution_date을 의미하는 것이 아니다.
3. 보통 back fill을 유용하게 하기 위해 execution_date 변수를 생성한다.
4. **execution_date은 현재 시점보다 이전 시간**이 나온다.
    1. execution_date은 현 시점 - duration으로 설정해야 back fill이 쉽다.
    2. full refresh할 경우 back fill이 아무 의미 없다.
    3. 현재 시점으로 execution_date을 설정하지 않아야 한다.

### DELETE FROM과 TRUNCATE

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

### Terminal 실행하기

- airflow tasks test my_first_dag print_hello {execution}
- airflow tasks run my_first_dag print_hello  {execution}
- execution_date은 과거 시점이어야 동작함
- test 명령어로 돌리면 실제 반영되지 않음
- airflow 실행은 터미널로 하는 것이 디버깅하기 쉬움

## 6️⃣ 주차 강의 내용
- Backfill과 Incremental Update에 대해 학습하였다.

### Params 옵션

1. schema = context['params']['schema']
2. function 파라미터인 **context의 params를 익숙하게 사용해 볼 것

### 주의할 점

- 데이터 작업은 클린하게 Fail 하는 것이 좋다.
- try~exception 사용 시 raise를 사용할 것
- raise 없을 시 except 처리 후 흘러가 버리므로 파악이 디버깅 어려움

### DW 구축 업무 순서

1. 첫 번째는 프로덕션 DB를 DW로 복사해오는 것
2. 프로덕션 DB는 OLTP로 MYSQL, Postgres / 데이터 웨어하우스는 OLAP
    1. OLTP의 목적은 빠르게 처리해서 응답하는 것
    2. 따라서 분석용으로 못씀. 분석용 쿼리 시 시간 지체되면 문제 발생
    3. 고로, DW 구축하기 위해 프로덕션 DB를 DW로 복사

### 데엔-데싸 협업

1. 명확한 업무 분담이 필요함
2. Dag 파일 제공하거나, 파라미터에 해당하는 딕셔너리만 configuration으로 분리
3. 그렇지 않을 경우 데엔에게 추가 기능 요구하는 경우 발생

    → 바쁠 경우 사이가 틀어지는 경우 발생

### airflow.cfg

- 자주 백업을 해 놓아야하고, fernet_key가 꼭 필요함(encryption key)

### Incremental 조건

1. created_at (timestamp)
2. modified (timestamp)
3. deleted (boolean)
    1. 레코드 삭제 시, 실제 삭제하지 말고 boolean으로 표시할 것
    2. 실제 삭제해버리면 full refresh 하지 않는 이상 삭제 여부를 알 수 없음

### Backfill 유의사항

1. airflow dags backfill -s 2021-09-16 -e 2021-09-17 dag_id
    1. 단, dag_id는 반드시 catchUp = True
    2. catchUp = False일 경우 실패하지 않고 무한 루프 발생
2. full refresh는 Backfill이 의미 없다.
    1. Incremental Update를 수행한다.
3. 평소에 Backfill을 미리 구현해 놓아야 사고가 터졌을 때 바로 복구가 가능
    1. 기존 데이터 소스가 Incremental Update를 지원해야함
    2. execution_date을 기준으로 update할 데이터 결정

## 7️⃣ 주차 강의 내용

### ⭐ Superset

- 오픈소스 대시보드

### 1. 설치 과정

1. Superset Direct Installation (Ubuntu)
2. Superset Installation via Docker
    1. 깔끔하긴 하나, 좋은 사양의 서버 필요
3. [Preset.io](http://preset.io) 사용
    1. 간편하게 사용 가능함

### 2. 설정 순서

1. Database 연결
2. DataSet 업로드
3. 대시보드 시각화
    1. Metrics → SIMPLE → AGGREGATE 선택
    2. SQL 생각하며 설정하기
    3. GROUP BY - SQL의 GROUP BY와 같음
4. 시각화 후 Save
    1. Chart뿐만 아니라, Dashboard에도 저장할 수 있음
    2. 대시보드 화면에서 Edit 설정
        1. 가로, 세로, 헤더 등 필요한 정보 추가 가능

### ⭐ 배운 것

1. 데이터 웨어하우스를 기반으로 데이터 인프라를 만드는 것
2. 파이썬, SQL & Data Warehouse, ETL (Airflow)
3. 여기서 더 알아야 하는 것 → 대규모 분산처리 (Spark)
    1. 대규모 분산처리를 아는 지 질문하기 위해 맵 리듀스 개념
4. Data Analayst와의 협업하는 방법 (Summary Table & Dashboard)
    1. Feature 계산
        1. 큰 스케일에서는 Spark 사용
        2. 작은 스케일에서는 Pandas 사용
    2. Model Serving
        1. Data Scientist가 만든 모델을 어떻게 프로덱션으로 deploy할 것인가?
    3. 모델 성능에 대한 A/B Test

### ⭐ 리마인드

1. 배움의 정체는 무조건 온다.
    1. 나보다 잘하는 사람은 나보다 똑똑한 것이 아니다.
    2. **그 사람은 나보다 더 오래 이 분야에 대해 노력해왔구나라고 Thinking**
    3. 내가 무엇을 모르는 지 나에게 계속 물어봐야함
        1. 무엇을 모르는 지 계속 생각하고, 쓰고, 주변에 물어봐라.
        2. 주변에 물어보는 과정에서 구체화되고 내가 무엇을 모르는 지 깨닫게 됨
        3. 이 시기가 지나면, Tipping Point를 넘어 실력 상승을 경험하게 됨
2. 준비된 상태는 없다는 걸 명심
    1. **자기 검열하지 않기**
        1. 기술 스택 10개 중 1개만 일치, 그것도 내가 잘한다고 할 수 없다할 때 → 이게 자기 검열
        2. **내 이력서가 적합한 지는 그 사람이 판단할 문제, 내가 판단할 문제가 아니다.**
        3. 공부를 하면서 면접을 봐야 내가 올바르게 공부하고 있는지 파악할 수 있음
        4. 그리고 면접을 보기 위해서는 이력서를 내는 것이 시작
            1. 면접을 할수록 늘기 때문에, 가고 싶은 곳을 미루고 닥치는 대로 넣기
            2. 그리고 가지 않을 회사라도 인터뷰 동안은 최선을 다하기
            3. 한 번에 모든 것(취업 등)을 끝내려고 하지 마라
    2. Agile 기반 커리어 개발
        1. 20대는 30대가, 30대는 40대가 끝인 것 같지만 절대 그렇지 않다.
        2. 한 곳에 잘 가서 평생 살려고 하기보다 사람이 맞는 곳, 성장할 수 있는 곳을 찾는 것
