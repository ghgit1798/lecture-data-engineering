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

