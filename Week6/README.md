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

## ⭐ 과제1

- execution_date 기준 Incremental update 수행
1. 과거에 실행된 dag를 backfill하기
    1. airflow dags backfill —start-date 2021-09-16 —end-date 2021-09-17 MySQL_to_Redshift
    2. 다음 단계는 레코드들의 중복을 제거하여 S3에서 Redshift로 Copy하는 것
2. 레코드 중복 제거하기
    1. S3에서 중복을 제거한 다음 Redshift로 Copy하는 것이 Best practice라고 생각했으나, 뜻대로 잘 되지 않음
    2. 따라서 S3에서 Redshift로 옮긴 후, SQL문을 수행해서 중복을 제거함

```python
# MySQL_to_Redshift.py 코드
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.providers.amazon.aws.transfers.mysql_to_s3 import MySQLToS3Operator
from airflow.providers.amazon.aws.operators.s3_delete_objects import S3DeleteObjectOperator
from airflow.providers.amazon.aws.transfers.s3_to_redshift import S3ToRedshiftOperator
from airflow.models import Variable
from airflow.hooks.postgres_hook import PostgresHook

from datetime import datetime
from datetime import timedelta

import requests
import logging
import psycopg2
import json

dag = DAG(
	dag_id = 'MySQL_to_Redshift',
	start_date = datetime(2021, 9, 9),
	schedule_interval = '0 9 * * *',
	max_active_runs = 1,
	default_args = {
		'retries' : 1,
		'retry_delay': timedelta(minutes=3),
	}
)

# Redshift connection
def get_Redshift_connection():
	hook = PostgresHook(postgres_conn_id='redshift_dev_db')
	return hook.get_conn().cursur()

# deduplication fucntion
def remove(**context):
	cur = get_Redshift_connection()
	sql = """DROP TABLE IF EXISTS temp_nps;
	CREATE TABLE temp_nps AS (SELECT DISTINCT * from ghgoo1798.nps);
	DROP TABLE ghgoo1798.nps;
	ALTER TABLE temp_nps RENAME TO nps;"""
	logging.info(sql)
	try:
		cur.execute(sql)
		cur.execute("COMMIT;")
	except Exception as e:
		cur.execute("Rollback;")

schema = "ghgoo1798"
table = "nps"
s3_bucket = "grepp-data-enginnering"
s3_key = schema + "-" + table

# s3_bucket이 이미 존재할 경우 삭제해주는 Operator
delete_s3_object = S3DeleteObjectOperator(
	task_id = "delete_s3_object",
	bucket = s3_bucket,
	keys = s3_key,
	aws_conn_id = "aws_conn_id",
	verify = False
)

mysql_to_nps = MySQLToS3Operator(
	task_id = 'mysql_to_s3_nps',
	query = "SELECT * FROM prod.nps",
	s3_bucket = s3_bucket,
	s3_key = s3_key,
	mysql_conn_id = "mysql_conn_id",
	aws_conn_id = "aws_conn_id",
	verify = False,
	dag = dag
)

s3_to_redshift_nps = S3ToRedshiftOperator(
	task_id = 's3_to_redshift_nps',
	s3_bucket = s3_bucket,
	s3_key = s3_key,
	schema = schema,
	table = table,
	copy_options = ['csv'],
	redshift_conn_id = 'redshift_dev_db',
	dag = dag
)

remove_duplicated_records = PythonOperator(
	task_id = 'remove_duplicated_records',
	python_callable = remove,
	dag = dag
)

delete_s3_object >> mysql_to_s3_nps >> s3_to_redshift_nps >> remove_duplicated_records

```

### 피드백 반영

- s3_to_redshift_nps와 remove_duplicated_records 사이에 누군가 접근한다면 중복 레코드가 액세스 될 수 있음
- mysql_to_s3_nps의 query 수정 → 필요한 날짜만 업데이트

```sql
mysql_to_s3_nps = MySQLToS3Operator(
	task_id = 'mysql_to_s3_nps',
	query = "SELECT * FROM prod.nps WHERE DATE(created_at) = '{{ execution_date }}'",
	s3_bucket = s3_bucket,
	s3_key = s3_key,
	mysql_conn_id = "mysql_conn_id",
	aws_conn_id = "aws_conn_id",
	verify = False,
	dag = dag
)
```

1. S3ToRedshiftOperator의 truncate_table 옵션 사용
    1. S3ToRedshiftOperator는 현재 Full refresh or 중복 저장만 허용..

```sql
s3_to_redshift_nps = S3ToRedshiftOperator(
	task_id = 's3_to_redshift_nps',
	s3_bucket = s3_bucket,
	s3_key = s3_key,
	schema = schema,
	table = table,
	copy_options = ['csv'],
	truncate_table = True,
	redshift_conn_id = "redshift_dev_db",
	dag = dag
)
```

1. Best practice : plugins.s3_to_redshift_operator 변경 (상속 및 재작성)

```sql
#from airflow.providers.amazon.aws.transfers.s3_to_redshift import S3ToRedshiftOperator
from plugins.s3_to_redshift_operator import S3ToRedshiftOperator

s3_to_redshift_nps = S3ToRedshiftOperator(
	task_id = 's3_to_redshift_nps',
	s3_bucket = s3_bucket,
	s3_key = s3_key,
	schema = schema,
	table = table,
	copy_options = ['csv'],
	redshift_conn_id = "redshift_dev_db",
	primary_key = "id",
	order_key = "created_at",
	dag = dag
)

s3_folder_cleanup >> mysql_to_s3_nps >> s3_to_redshift_nps
```

```sql
# s3_to_redshift_operator.py

from typing import List, Optional, Union

from airflow.models import BaseOperator
from airflow.providers.amazon.aws.hooks.s3 import S3Hook
from airflow.providers.amazon.aws.utils.redshift import build_credentials_block
from airflow.providers.postgres.hooks.postgres import PostgresHook

class S3ToRedshiftOperator(BaseOperator):
    """
    Executes an COPY command to load files from s3 to Redshift
    .. seealso::
        For more information on how to use this operator, take a look at the guide:
        :ref:`howto/operator:S3ToRedshiftOperator`
    :param schema: reference to a specific schema in redshift database
    :type schema: str
    :param table: reference to a specific table in redshift database
    :type table: str
    :param s3_bucket: reference to a specific S3 bucket
    :type s3_bucket: str
    :param s3_key: reference to a specific S3 key
    :type s3_key: str
    :param redshift_conn_id: reference to a specific redshift database
    :type redshift_conn_id: str
    :param aws_conn_id: reference to a specific S3 connection
        If the AWS connection contains 'aws_iam_role' in ``extras``
        the operator will use AWS STS credentials with a token
        https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-authorization.html#copy-credentials
    :type aws_conn_id: str
    :param verify: Whether or not to verify SSL certificates for S3 connection.
        By default SSL certificates are verified.
        You can provide the following values:
        - ``False``: do not validate SSL certificates. SSL will still be used
                 (unless use_ssl is False), but SSL certificates will not be
                 verified.
        - ``path/to/cert/bundle.pem``: A filename of the CA cert bundle to uses.
                 You can specify this argument if you want to use a different
                 CA cert bundle than the one used by botocore.
    :type verify: bool or str
    :param column_list: list of column names to load
    :type column_list: List[str]
    :param copy_options: reference to a list of COPY options
    :type copy_options: list
    :param truncate_table: whether or not to truncate the destination table before the copy
    :type truncate_table: bool
    """

    template_fields = ('s3_bucket', 's3_key', 'schema', 'table', 'column_list', 'copy_options')
    template_ext = ()
    ui_color = '#99e699'

    def __init__(
        self,
        *,
        schema: str,
        table: str,
        s3_bucket: str,
        s3_key: str,
        redshift_conn_id: str = 'redshift_default',
        aws_conn_id: str = 'aws_default',
        verify: Optional[Union[bool, str]] = None,
        column_list: Optional[List[str]] = None,
        copy_options: Optional[List] = None,
        autocommit: bool = False,
        truncate_table: bool = False,
        primary_key: str = '',
        order_key: str = '',
        **kwargs,
    ) -> None:
        super().__init__(**kwargs)
        self.schema = schema
        self.table = table
        self.s3_bucket = s3_bucket
        self.s3_key = s3_key
        self.redshift_conn_id = redshift_conn_id
        self.aws_conn_id = aws_conn_id
        self.verify = verify
        self.column_list = column_list
        self.copy_options = copy_options or []
        self.autocommit = autocommit
        self.truncate_table = truncate_table
        self.primary_key = primary_key
        self.order_key = order_key

    def _build_copy_query(self, credentials_block: str, copy_options: str) -> str:
        column_names = "(" + ", ".join(self.column_list) + ")" if self.column_list else ''
        return f"""
                    COPY {self.schema}.{self.table} {column_names}
                    FROM 's3://{self.s3_bucket}/{self.s3_key}'
                    with credentials
                    '{credentials_block}'
                    {copy_options};
        """

    def get_columns_from_table(self, hook):
        sql = f"""SELECT column_name
        FROM information_schema.columns
        WHERE table_name = '{self.table}' and table_schema = '{self.schema}'
        ORDER BY ordinal_position"""
        results = hook.get_records(sql)
        cols = []
        for r in results:
            cols.append(r[0])
        return ",".join(cols)

    def generate_after_query(self, postgres_hook):
        if self.primary_key is not None and self.order_key is not None:
            columns = self.get_columns_from_table(postgres_hook)
            return f"""
                CREATE TEMPORARY TABLE T AS SELECT {columns}
                FROM (
                    SELECT *, ROW_NUMBER() OVER (PARTITION BY {self.primary_key} ORDER BY {self.order_key} DESC) n
                    FROM {self.schema}.{self.table}
                )
                WHERE n = 1;
                DELETE FROM {self.schema}.{self.table};
                INSERT INTO {self.schema}.{self.table} SELECT * FROM T;
            """
        else:
            return ''

    def execute(self, context) -> None:
        postgres_hook = PostgresHook(postgres_conn_id=self.redshift_conn_id)
        s3_hook = S3Hook(aws_conn_id=self.aws_conn_id, verify=self.verify)
        credentials = s3_hook.get_credentials()
        credentials_block = build_credentials_block(credentials)
        copy_options = '\n\t\t\t'.join(self.copy_options)

        copy_statement = self._build_copy_query(credentials_block, copy_options)
        after_statement = self.generate_after_query(postgres_hook)

        if self.truncate_table:
            delete_statement = f'DELETE FROM {self.schema}.{self.table};'
            sql = f"""
            BEGIN;
            {delete_statement}
            {copy_statement}
            COMMIT
            """
        elif after_statement != '':
            sql = f"""
            BEGIN;
            {copy_statement}
            {after_statement}
            COMMIT
            """
        else:
            sql = copy_statement

        self.log.info('Executing COPY command...')
        postgres_hook.run(sql, self.autocommit)
        self.log.info("COPY command complete...")
```

## ⭐과제 2

- Summary Table 만들기
1. NPS란? Net Promoter Score
2. 10, 9점 추천하겠다는 고객 비율에서 0-6점 불평 고객의 비율을 뺀 값

```sql
# nps 테이블 생성

DROP TABLE IF EXISTS ghgoo1798.nps;
CREATE TABLE ghgoo1798.nps (
	id INT NOT NULL primary key,
	created_at timestamp,
	score smallint
);

## nps_summary 테이블 생성

DROP TABLE IF EXISTS ghgoo1798.nps_summary;
CREATE TABLE nps_summary AS (
	SELECT LEFT(created_at, 10) Date,
	(ROUND(COUNT(CASE WHEN score IN (10, 9) THEN score END)/COUNT(1)*100, 0) -
	ROUND(COUNT(CASE WHEN score IN (0,1,2,3,4,5,6) THEN score END)/COUNT(1)*100, 0) NPS
	FROM nps
	GROUP BY LEFT(created_at, 10)
);
```
### Best practice

- Case 1

```sql
SELECT month, ROUND((promoters-detractors)::float/total_count*100, 2) AS overall_nps
FROM (
	SELECT LEFT(created_at, 7) AS month,
		COUNT(CASE WHEN score >= 9 THEN 1 END) AS promotors,
		COUNT(CASE WHEN score <= 6 THEN 1 END) AS detractors,
		COUNT(CASE WHEN score > 6 AND score < 9 THEN 1 END) AS passives,
		COUNT(1) AS total_count
	FROM ghgoo1798.nps
	GROUP BY 1
	ORDER BY 1
);
```

- Case 2

```sql
SELECT LEFT(created_ad, 7) AS month, 
	ROUND(SUM(CASE 
		WHEN score >= 9 THEN 1
		WHEN score <= 6 THEN -1 END)::float*100/COUNT(1), 2)
FROM ghgoo1798.nps
GROUP BY 1
ORDER BY 1;
```
