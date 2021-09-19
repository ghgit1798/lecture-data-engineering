## ⭐ Superset

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

## ⭐ 배운 것

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

## ⭐ 리마인드

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