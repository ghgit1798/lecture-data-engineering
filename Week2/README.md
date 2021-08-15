# Week2

## ⭐ 데이터를 처리 방식

1. 데이터가 작을 때, Pandas로 처리
2. 데이터가 클 때, Spark로 처리
    1. Data의 크기에 제약이 덜 함
3. ETL, ELT의 경우 데이터 처리 시 Coding

## ⭐ ETL 프레임워크 (ex. Airflow)

1. Job간 의존성 걸기 쉬움
2. 스케줄링 용이
3. 코드를 새로 짤 필요 없이 과거의 데이터를 일련의 메커니즘으로 처리할 수 있음

### 🔹 커리어 이야기

오라클은 프로덕션 데이터베이스로 주로 사용한다.

**해당 기업의 데이터 팀의 조직 구조를 알 수 있는 질문**

- **데이터팀이 중앙에 있는지, 데이터팀이 각 부서에 흩어져서 업무를 하느냐?**
- 데이터팀에는 다양한 백그라운드를 가진 사람들이 많다.

**▶ 정말 하고 싶은 것이 있으면, 100% 심혈을 기울여서 열심히 해본 뒤 결정하자**

## ⭐ 데이터팀의 한 주

### 💠 월요일

📌  **Sprint 활동**

1. Sprint 데모 미팅
    - 데이터팀은 보통 칸반, 백엔드는 스크럼을 보통 사용 (deploy 방식 차이)
    - 칸반 : 태스크 별로 끝나는 경우 바로 deploy
    - 스크럼 : 2주 끝날 때마다 프로덕션에 deploy
2. Sprint 회고 미팅
    - What went well
    - What could have gone better
    - Any discusstion points
3. Sprint 플래닝 미팅
    - 40%의 시간은 인프라 코드의 refactoring에 사용
    - 미팅 제외 하루 5시간 일한다고 가정

**📌 On-Call 엔지니어 지정 (하는 일)**

1. 다음 일주일간 (주말 포함) 모든 **data pipeline 실패와 관련한 이슈 해결**
    - 100% 성공 시키는 것이 아니라, 가장 중요한 20%정도만 잡아서 반드시 성공시키는 것
    - 주말에도 보고 있어야하며, 상시로 보고 있어야함
    - **데이터 엔지니어링이 노가다라고 하는 이유**
2. Data pipline failure rate을 Key metrics로 관리
3. Focus is on key summary tables

### 💠 화요일

**1. Daily standup**

- 오래 X. 아주 짧게 자신의 상황 얘기하고 넘어가는 것

**2. 데이터 엔지니어링 == 노가다?**

- Data pipeline의 경우 Fail된 job을 직접 확인해서 복구해야함

### 💠 수/목요일

1. Daily standup

**2. Office Hour**

3. 데이터 파이프라인 개발 최적화

### 💠 금요일

1. Daily standup

2. 데이터 파이프라인 개발 최적화

3. 주간 스태프 미팅

- Metrics & Quarterly Goals 리뷰
- SLA(Service Layer Attention)란 정해진 시간 내에 에러가 복구되면 SLA 달성, 그렇지 않으면 실패
- Recruiting & Retention (잘하는 사람은 금방 이직함)

## ⭐ 클라우드 컴퓨팅

### 🔹 장점

1. **초기 투자 비용**이 줄어든다.
2. **리소스 준비를 위한 대기시간 감소**
    - 기다릴 필요 없다. 보통은 서버 준비하는데 시간이 오래걸림
3. 사용한만큼 돈을 지불한다.
    1. **노는 리소스 제거**로 비용 감소
    2. 연간비용이 월간비용으로 넘어감
    3. 재무팀이 싫어함... 재무팀CEO가 반대하는 경우 많음

## ⭐ AWS

1. EC2 - Elastic Compute Cloud

1. On-Demand
2. Reserved
3. Spot Instance

2. S3 - Simple Storage Service

## ⭐ D/W SQL

1. Hive/SparkSQL/Presto
    1. 맵 리듀스를 적용할 수 있는 SQL
    2. 속도는 낮으나, 매우 편리함
2. OLAP vs OLTP
    1. 네트워크상의 여러 이용자가 실시간으로 데이터베이스의 데이터를 갱신하거나 조회하는 등의 단위 작업을 처리하는 방식을 말한다. 주로 신용카드 조회 업무나 자동 현금 지급 등 금융 전산 관련 부문에서 많이 발생하기 때문에 ‘온라인 거래처리’라고도 한다.
3. DW는 Interal용이지, Production DB가 아니다.
4. Summary table은 데이터분석가들이 생성해서 사용
    1. **모두가 한 데이터를 맞춰서 사용하는 것이 중요함**
    2. Consistency가 처음에 훨씬 중요하다. incorrect하더라도.

## ⭐ Data Stoarge

### Fixed Cost option vs Variable Cost Option

1. BigQuery and Snowflake
    1. 요금이 왕창 나올 수 있음
    2. SQL를 더 신중하게 작성해야함
    3. 데이터가 커질수록 사용하기 편함
    4. Variable Cost option
2. Redshift
    1. Fixed Cost option
    2. 비용 계산 시 정해놓은만큼 지불
    3. 데이터가 커지면 사용하기 불편함
3. 빅데이터 DW는 Primary key의 유일성을 보장하지 않음
    - 어떻게 DW에서 보장하도록 할 수 있는지? → 5-6주차

## ⭐ Redshift

- Table design을 잘하는 것이 중요함
- JSON, TEXT 타입은 없음

## ⭐ Database best practice

1. 데이터 공개 정책
    1. 유데미는 모든 직원들에게 공개
    2. 이상한 SQL날리는 경우 발생..
2. raw data vs analytics vs adhoc
    1. 이상한 raw data를 읽어서 결론 내리는 경우 있음..
    2. raw : 외부에서 그대로 Dump 떠온 데이터
    3. analytics : 요약 데이터
    4. adhoc : 플레이 그라운드
3. **Naming convention을 잘 만들 것**
    1. 명사와 명사 사이에 _를 둘 것이냐, Camel Case로 할 거냐, 첫 문자를 대문자로 할 것이냐
    2. 필드, 테이블 이름을 단수로 할 것이냐, 복수로 할 것이냐
        - user였나? users였나?
        - _가 들어갔냐, 안들어갔냐?
        - 다 기억할 수 없으므로 2번 생각해야 함
4. DB는 마스터/슬레이브 아키텍처를 적용
    1. 데이터팀은 slave만 읽도록
    2. 그래가 엄청난 트래픽을 읽어갔는지, 아닌지 확인할 수 있음

## ⭐ 느낀점

1. **SQL을 좀 더 집중해서 정리할 것**
2. Vue - Django - Airflow - EC2 - AWS를 활용한 식단만족도 웹사이트 제작해보기
