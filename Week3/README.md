## ⭐ 커리어이야기 - 피드백 중요성

### 📌 행동 양식과 관련된 피드백

- 두루뭉실하다면 매니저에게 추가적인 질문할 것
- 새로운 레벨로 올라간다면 이전레벨에서 원하던 것이 아니다.
- **선한 의도로 받고, 내가 변해야 하는 상황임을 직시하자.**

### 📌 피드백을 줄 때 유의사항

1. 어떤 피드백을 줄 건가?
2. 이 피드백을 정말 줄 필요가 있는가?
3. 말하는 방식 : 내가 옳다가 아닌, 기대 → 관찰 → 그 후 발생한 Gap에 대해 상의
    - 신뢰가 기반이 되어야함

        → 그렇지 않으면 불편하게(부정적인 의도로) 받아들일 수 있음

    - 내가 선한 의도로 피드백을 주고 있는가

        → 내가 실수했을 때는, 인정하고 사과하는 것이 중요함 (인간적인 모습)

        → 감정 배제 : 사람을 배제하고 일에 포커스해서 피드백

        → 기대했던 건 이건데 → 관찰해보니 → 이런 갭(Gap)이 있다.

        💧 팀원에게 인기 있어야 하지 않아야한다.

        💧 따라서, 기대 → 관찰 → 갭(Gap) 후 팀원과 상의

        💧 내가 옳다가 아닌, 무언가 Gap이 있는데 무엇인가?

    - 긍정적인 피드백도 구체적으로 줘야한다.

### 📌 피드백과 해고 이야기 3가지

1. 자신의 관심 분야 찾기
2. 자신의 강점을 찾기
3. 일을 효율적으로 하기

## ⭐ 강의 내용

중요한 점

1. Airflow를 활용한 ETL 작성
2. SQL query문을 잘 작성
3. 1개의 DW에 대해 잘 아는 것

💠 **얻어갈 점**

1. Airlfow를 활용한 ETL 사용법 숙지
2. SQL Query문 추가 학습
3. Redshift에 대해 집중 공부

### 🔹 기억할 점

1. **현업에서 깨끗한 데이터란 존재 ❌**
    1. 데이터 한정, 의심병을 가지는 것은 좋은 습관
    2. 항상 무조건 데이터를 믿을 수 있는지 의심해라.
    3. 실제 레코드 몇 개를 살펴보는 것 만한 것이 없음 → **노가다**
2. 데이터 체크하는 법
    1. 중복된 레코드 체크
    2. 최근 데이터가 들어 왔는지 확인 (freshness)
    3. Primary key uniqueness가 지켜지는 지 체크 (있는지와 유니크한 지)
    4. 위 체크는 코딩의 unit test 형태로 만들어 매 번 쉽게 체크해 볼 수 있음
3. 중요한 테이블들이 무엇이고, 메타 정보를 어떻게 잘 관리할 것인가

❓ Data discovery 문제란?

→ Data discovery 프레임워크 설치해서 검색 엔진 형태로 찾아볼 수 있게 해야함

❗ Primary key를 지정해도, DW에서는 삽입할 시 보장하지 않음

→ 테이블을 만드는 사람이 PK uniqueness를 보장해야함

→ 어떻게 PK uniqueness를 보장할 것이냐? (6주차)

### 🔹 Tip

1. Time의 경우
    - 우선은 UTC로 지정 후 display 시에만 로컬로 변경
2. Join 시 고려할 점
    - ⭐ 중복 레코드가 없고, PK의 Uniqueness가 보장됨을 체크해야함
3. 타임 스탬프 필드가 있다면 최근에도 업데이트된 레코드가 있는지 꼭 확인. 언제부터 레코드가 생성되었는지도 확인. 월별로 레코드수를 확인해보는 것도 좋은 버릇

## ⭐ SQL 학습

SQL을 익숙하게 사용할 수 있도록 다음 내용에 대해 학습을 진행했다.

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

## ⭐ Assignment

1. 유저별 가장 먼저 방문한 채널과, 나중에 방문한 채널은?

```sql
%%sql

SELECT *
FROM (
  SELECT userid, channel, ROW_NUMBER() OVER(partition by userid order by ts) f_channel, ROW_NUMBER() OVER(partition by userid order by ts desc) l_channel
  FROM raw_data.user_session_channel usc
    JOIN raw_data.session_timestamp st ON usc.sessionid = st.sessionid
)
WHERE f_channel = 1 or l_channel = 1
LIMIT 10;
```

- Best Practice

```sql
%%sql

WITH cte AS (
    SELECT userid, channel, (ROW_NUMBER() OVER (PARTITION BY usc.userid ORDER BY st.ts acc)) AS rn1, (ROW_NUMBER() OVER (PARTITION BY usc.userid ORDER BY st.ts desc)) AS rn2
    FROM raw_data.user_session_channel usc
    JOIN raw_data.session_timestamp st ON usc.sessionid = st.sessionid
)

SELECT cte1.userid, cte1.channel AS first_touch, cte2.channel AS last_touch
FROM cte cte1
JOIN cte cte2 ON cte1.userid = cte2.userid
WHERE cte1.rn1 = 1 and cte2.rn2 = 1
ORDER BY 1;
```

With AS 구문이 익숙하지 않아 사용할 생각을 미처 못했다. 미리 SELECT한 테이블을 생성하고, 그걸 다시 Self Join하면 더 간단하게 해결할 수 있는 문제였다.

1. Gross Revenue 찾기

```sql
%%sql 
-- 상위 10개 유저 탐색

SELECT userid, sum(amount) total_amount
FROM raw_data.session_transaction st LEFT JOIN raw_data.user_session_channel usc ON st.sessionid = usc.sessionid
GROUP BY userid
ORDER BY total_amount desc
LIMIT 10;
```

이 문제는 의도한 대로 잘 풀어서 칭찬을 받았다. 👍

1. 채널별 월 매출액 테이블 만들기

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

정답은 구했지만... CASE WHEN 문법을 사용하지 못했다. 따라서 서브쿼리가 중첩되고 가독성이 떨어지는 SQL문으로 제출할 수 밖에 없었다.

→ CASE WHEN은 집계 함수인 COUNT, SUM 안에서도 사용할 수 있다.

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
