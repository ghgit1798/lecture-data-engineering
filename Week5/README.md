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

- airflow tasks test my_first_dag print_hello {execution_date}
- airflow tasks run my_first_dag print_hello  {execution_date}
