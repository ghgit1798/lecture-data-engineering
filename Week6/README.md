## ⭐ Params 옵션

1. schema = context['params']['schema']
2. function 파라미터인 **context의 params를 익숙하게 사용해 볼 것

## ⭐ 주의할 점

- 데이터 작업은 클린하게 Fail 하는 것이 좋다.
- try~exception 사용 시 raise를 사용할 것
- raise 없을 시 except 처리 후 흘러가 버리므로 파악이 디버깅 어려움

## ⭐ DW 구축 업무 순서

1. 첫 번째는 프로덕션 DB를 DW로 복사해오는 것
2. 프로덕션 DB는 OLTP로 MYSQL, Postgres / 데이터 웨어하우스는 OLAP
    1. OLTP의 목적은 빠르게 처리해서 응답하는 것
    2. 따라서 분석용으로 못씀. 분석용 쿼리 시 시간 지체되면 문제 발생
    3. 고로, DW 구축하기 위해 프로덕션 DB를 DW로 복사

## ⭐ 데엔-데싸 협업

1. 명확한 업무 분담이 필요함
2. Dag 파일 제공하거나, 파라미터에 해당하는 딕셔너리만 configuration으로 분리
3. 그렇지 않을 경우 데엔에게 추가 기능 요구하는 경우 발생

    → 바쁠 경우 사이가 틀어지는 경우 발생

## ⭐ airflow.cfg

- 자주 백업을 해 놓아야하고, fernet_key가 꼭 필요함(encryption key)

## ⭐ Incremental 조건

1. created_at (timestamp)
2. modified (timestamp)
3. deleted (boolean)
    1. 레코드 삭제 시, 실제 삭제하지 말고 boolean으로 표시할 것
    2. 실제 삭제해버리면 full refresh 하지 않는 이상 삭제 여부를 알 수 없음

## ⭐ Backfill 유의사항

1. airflow dags backfill -s 2021-09-16 -e 2021-09-17 dag_id
    1. 단, dag_id는 반드시 catchUp = True
    2. catchUp = False일 경우 실패하지 않고 무한 루프 발생
2. full refresh는 Backfill이 의미 없다.
    1. Incremental Update를 수행한다.
3. 평소에 Backfill을 미리 구현해 놓아야 사고가 터졌을 때 바로 복구가 가능
    1. 기존 데이터 소스가 Incremental Update를 지원해야함
    2. execution_date을 기준으로 update할 데이터 결정
