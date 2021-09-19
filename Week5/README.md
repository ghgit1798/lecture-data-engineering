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

## ⭐ 과제

### Workflow DAG 변경하기

1. weather_foreacast 테이블 생성하기
    1. api key 발급 후, DAG Variable 등록
    2. 앞으로 updated_date으로 primary key uniqueness 보장
    3. 이유는 사용하는 api에서 execution_date을 지정할 수 없기 때문

```sql
-- weather_forecast 테이블 생성

CREATE TABLE ghgoo1798.weather_forecast(
	date date primary key,
	temp float,
	min_temp float,
	max_temp float,
	updated_date timestamp default GETDATE()
);
```

1. API를 활용해 데이터 수집하기

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.models import Variable
from airflow.hoooks.postgres_hook import PostgresHook

from datetime import datetime
from datetime import timedelta

import requests
import logging
import psycopg2
import json

def get_Redshift_connection():
	# autocommit is False by default
	hook = PostgresHook(postgres_conn_id = 'redshift_dev_db')
	return hook.get_conn().cursor()

def etl(**context):
    api_key = Variable.get("open_weather_api_key")
    # 서울의 위도/경도
    lat = 37.5665
    lon = 126.9780

    # https://openweathermap.org/api/one-call-api
    url = "https://api.openweathermap.org/data/2.5/onecall?lat=%s&lon=%s&appid=%s&units=metric" % (lat, lon, api_key)
    response = requests.get(url)
    data = json.loads(response.text)

    """
    {'dt': 1622948400, 'sunrise': 1622923873, 'sunset': 1622976631, 'moonrise': 1622915520, 'moonset': 1622962620, 'moon_phase': 0.87, 'temp': {'day': 26.59, 'min': 15.67, 'max': 28.11, 'night': 22.68, 'eve': 26.29, 'morn': 15.67}, 'feels_like': {'day': 26.59, 'night': 22.2, 'eve': 26.29, 'morn': 15.36}, 'pressure': 1003, 'humidity': 30, 'dew_point': 7.56, 'wind_speed': 4.05, 'wind_deg': 250, 'wind_gust': 9.2, 'weather': [{'id': 802, 'main': 'Clouds', 'description': 'scattered clouds', 'icon': '03d'}], 'clouds': 44, 'pop': 0, 'uvi': 3}
    """
    ret = []
    for d in data["daily"]:
        day = datetime.fromtimestamp(d["dt"]).strftime('%Y-%m-%d')
        ret.append("('{}',{},{},{})".format(day, d["temp"]["day"], d["temp"]["min"], d["temp"]["max"]))

    cur = get_Redshift_connection()
    insert_sql = """DELETE FROM ghgoo1798.weather_forecast;INSERT INTO ghgoo1798.weather_forecast VALUES """ + ",".join(ret)
    logging.info(insert_sql)
    try:
        cur.execute(insert_sql)
        cur.execute("Commit;")
    except Exception as e:
        cur.execute("Rollback;")

dag = DAG(
    dag_id = 'Weather_to_Redshift',
    start_date = datetime(2021,9,3), # 날짜가 미래인 경우 실행이 안됨
    schedule_interval = '0 2 * * *',  # 적당히 조절
    max_active_runs = 1,
    catchup = False,
    default_args = {
        'retries': 1,
        'retry_delay': timedelta(minutes=3),
    }
)

etl = PythonOperator(
    task_id = 'etl',
    python_callable = etl,
    dag = dag
)
```

1. Primary Key 유지하기 위해 temp 테이블 생성
    1. temp 테이블에 수집 데이터가 저장되도록 DAG 변경
    2. updated_date가 최신인 행 1개씩만 추출해서 원본 테이블에 저장
    3. ROW_NUMBER, Partition by, Order by 활용

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
# from airflow.operators import PythonOperator
from airflow.models import Variable
from airflow.hooks.postgres_hook import PostgresHook

from datetime import datetime
from datetime import timedelta

import requests
import logging
import psycopg2
import json

def get_Redshift_connection():
    # autocommit is False by default
    hook = PostgresHook(postgres_conn_id='redshift_dev_db')
    return hook.get_conn().cursor()

def etl(**context):
    api_key = Variable.get("open_weather_api_key")
    # 서울의 위도/경도
    lat = 37.5665
    lon = 126.9780

    # https://openweathermap.org/api/one-call-api
    url = "https://api.openweathermap.org/data/2.5/onecall?lat=%s&lon=%s&appid=%s&units=metric" % (lat, lon, api_key)
    response = requests.get(url)
    data = json.loads(response.text)

    """
    {'dt': 1622948400, 'sunrise': 1622923873, 'sunset': 1622976631, 'moonrise': 1622915520, 'moonset': 1622962620, 'moon_phase': 0.87, 'temp': {'day': 26.59, 'min': 15.67, 'max': 28.11, 'night': 22.68, 'eve': 26.29, 'morn': 15.67}, 'feels_like': {'day': 26.59, 'night': 22.2, 'eve': 26.29, 'morn': 15.36}, 'pressure': 1003, 'humidity': 30, 'dew_point': 7.56, 'wind_speed': 4.05, 'wind_deg': 250, 'wind_gust': 9.2, 'weather': [{'id': 802, 'main': 'Clouds', 'description': 'scattered clouds', 'icon': '03d'}], 'clouds': 44, 'pop': 0, 'uvi': 3}
    """
    ret = []
    for d in data["daily"]:
        day = datetime.fromtimestamp(d["dt"]).strftime('%Y-%m-%d')
        # Primary Key 유지를 위한 updated_date도 추가해줘야함
        now = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        ret.append("('{}',{},{},{},{})".format(day, d["temp"]["day"], d["temp"]["min"], d["temp"]["max"], now))

    ### 스키마 변경 + 수집할때마다 삭제하지 않고 계속 누적
    cur = get_Redshift_connection() 
    insert_sql = """INSERT INTO ghgoo1798.temp_weather_forecast VALUES """ + ",".join(ret)
    logging.info(insert_sql)
    try:
        cur.execute(insert_sql)
        cur.execute("Commit;")
    except Exception as e:
        cur.execute("Rollback;")

## 원본 테이블에 저장하는 save 함수 생성
def save():
  cur = get_Redshift_connection()
  sql = """
    BEGIN; 
    DELETE FROM ghgoo1798.weather_forecast; 
    INSERT INTO ghgoo1798.weather_forecast SELECT date, temp, min_temp, max_temp, updated_date 
    FROM (
      SELECT *, ROW_NUMBER() OVER (PARTITION BY date ORDER BY updated_date DESC) seq
      FROM ghgoo1798.temp_weather_forecast
    ) WHERE seq=1;
    END;
  """
  cur.execute(sql)

dag = DAG(
    dag_id = 'Weather_to_Redshift',
    start_date = datetime(2021,9,3), # 날짜가 미래인 경우 실행이 안됨
    schedule_interval = '0 2 * * *',  # 적당히 조절
    max_active_runs = 1,
    catchup = False,
    default_args = {
        'retries': 1,
        'retry_delay': timedelta(minutes=3),
    }
)

etl = PythonOperator(
    task_id = 'etl',
    python_callable = etl,
    dag = dag
)

## save task 생성
save = PythonOperator(
    task_id = 'save',
    python_callable = save,
    dag = dag
)

etl >> save
```

1. 피드백 반영
    1. PostgresHook 사용 시, autocommit이 False로 설정되므로 BEGIN; END;로 둘러싸지 않아도 된다.
        1. 고로 save의 경우 commit;이 실행되지 않아 며칠동안 동작 시 안정성을 보장할 수 없음
        2. 따라서 SQL 실행을 try/except로 넣고 commit; rollback 수행
    2. try/except 처리 시 exception이 발생했을 때, except로 처리하면 에러가 더 이상 전파되지 않음. ETL 운영 관점에서 에러가 난 걸 알 수가 없다.
        1. 따라서 try ~ exception 처리 후 뒤에 raise를 붙여 줄 것
        2. raise는 직접 에러를 발생시키는 명령어

    ```python
    from airflow import DAG
    from airflow.operators.python import PythonOperator
    # from airflow.operators import PythonOperator
    from airflow.models import Variable
    from airflow.hooks.postgres_hook import PostgresHook

    from datetime import datetime
    from datetime import timedelta

    import requests
    import logging
    import psycopg2
    import json

    def get_Redshift_connection():
        # autocommit is False by default
        hook = PostgresHook(postgres_conn_id='redshift_dev_db')
        return hook.get_conn().cursor()

    def etl(**context):
        api_key = Variable.get("open_weather_api_key")
        # 서울의 위도/경도
        lat = 37.5665
        lon = 126.9780

        # https://openweathermap.org/api/one-call-api
        url = "https://api.openweathermap.org/data/2.5/onecall?lat=%s&lon=%s&appid=%s&units=metric" % (lat, lon, api_key)
        response = requests.get(url)
        data = json.loads(response.text)

        """
        {'dt': 1622948400, 'sunrise': 1622923873, 'sunset': 1622976631, 'moonrise': 1622915520, 'moonset': 1622962620, 'moon_phase': 0.87, 'temp': {'day': 26.59, 'min': 15.67, 'max': 28.11, 'night': 22.68, 'eve': 26.29, 'morn': 15.67}, 'feels_like': {'day': 26.59, 'night': 22.2, 'eve': 26.29, 'morn': 15.36}, 'pressure': 1003, 'humidity': 30, 'dew_point': 7.56, 'wind_speed': 4.05, 'wind_deg': 250, 'wind_gust': 9.2, 'weather': [{'id': 802, 'main': 'Clouds', 'description': 'scattered clouds', 'icon': '03d'}], 'clouds': 44, 'pop': 0, 'uvi': 3}
        """
        ret = []
        for d in data["daily"]:
            day = datetime.fromtimestamp(d["dt"]).strftime('%Y-%m-%d')
            # Primary Key 유지를 위한 updated_date도 추가해줘야함
            now = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
            ret.append("('{}',{},{},{},{})".format(day, d["temp"]["day"], d["temp"]["min"], d["temp"]["max"], now))

        ### 스키마 변경 + 수집할때마다 삭제하지 않고 계속 누적
        cur = get_Redshift_connection() 
        insert_sql = """INSERT INTO ghgoo1798.temp_weather_forecast VALUES """ + ",".join(ret)
        logging.info(insert_sql)
        try:
            cur.execute(insert_sql)
            cur.execute("Commit;")
        except Exception as e:
            cur.execute("Rollback;")

    ## 원본 테이블에 저장하는 save 함수 생성
    def save():
      cur = get_Redshift_connection()
      sql = """
        DELETE FROM ghgoo1798.weather_forecast; 
        INSERT INTO ghgoo1798.weather_forecast SELECT date, temp, min_temp, max_temp, updated_date 
        FROM (
          SELECT *, ROW_NUMBER() OVER (PARTITION BY date ORDER BY updated_date DESC) seq
          FROM ghgoo1798.temp_weather_forecast
        ) WHERE seq=1;
      """
      try:
        cur.execute(sql)
        cur.execute("Commit;")
      except Exception as e:
        cur.execute("Rollback;")
        raise

    dag = DAG(
        dag_id = 'Weather_to_Redshift',
        start_date = datetime(2021,9,3), # 날짜가 미래인 경우 실행이 안됨
        schedule_interval = '0 2 * * *',  # 적당히 조절
        max_active_runs = 1,
        catchup = False,
        default_args = {
            'retries': 1,
            'retry_delay': timedelta(minutes=3),
        }
    )

    etl = PythonOperator(
        task_id = 'etl',
        python_callable = etl,
        dag = dag
    )

    ## save task 생성
    save = PythonOperator(
        task_id = 'save',
        python_callable = save,
        dag = dag
    )

    etl >> save
    ```
