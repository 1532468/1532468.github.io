---
title : "[Data Infra] Airflow install"
categories:
  - Data Infra
---
<figure>
  <img src="/assets/images/2019-04-25-Airflow/logo.png">
  <figcaption></figcaption>
</figure>

conda env 환경에서 진행하겠다.


설치하기
```
1. conda create -n airflow python=3.6

2. conda activate airflow 또는 source activate airflow

3. conda install -y -c conda-forge airflow
```

LocalExecutor를 이용하는 방법

```
LocalExecutor를 이용하기 위해서는 RDBMS를 이용하여야 한다.


> airflow initdb
  홈 밑에 airflow 폴더가 생성된다.

> vi ~/airflow/airflow.cfg

  executor = LocalExecutor
  sql_alchemy_conn = mysql://id:password@host:port/db

- mysql에 접속하여 DB를 만든다.
```

```
> airflow initdb

  위의 명령을 실행하면 만들어 두었던 DB에 많은 테이블이 생성된다.

ModuleNotFoundError: No module named 'MySQLdb'
  > conda install -y -c anaconda mysqlclient 


> airflow scheduler
> airflow webserver

Exception: Global variable explicit_defaults_for_timestamp needs to be on (1) for mysql

  > vi /etc/my.cnf
  
  [mysqld]
  explicit_defaults_for_timestamp = 1

  > service mysql restart


이게 끝이다.
```
http://host:8080에 접속하면 많은 example을 나온다.
```
> vi ~/airflow/airflow.cfg

   load_examples = False

> airflow restart
```


CeleryExecutor를 이용하는 방법
```
일단, rabbitMQ 부터..
> conda install -c conda-forge rabbitmq-server
> rabbitmq-server -detached
> rabbitmqctl start_app
> rabbitmqctl add_user ID PASSWORD
> rabbitmqctl set_user_tags ID administrator
> rabbitmqctl add_vhost airflow
> rabbitmq-plugins enable rabbitmq_management 

마지막 명령어는 web을 통해 관리를 하고자 합니다. 이유는..

  
    rabbitmqctl set_permissions -p airflow ID “.*” “.*” “.*”
    rabbitmqctl set_permissions -p airflow ID＇.*＇ ＇.*＇ ＇.*＇ 
    rabbitmqctl set_permissions -p airflow ID ´.*´ ´.*´ ´.*´
    rabbitmqctl set_permissions -p airflow ID ｀.*｀ ｀.*｀ ｀.*｀
    rabbitmqctl set_permissions -p airflow ID ˝.*˝ ˝.*˝ ˝.*˝
    rabbitmqctl set_permissions -p airflow ID ＂.*＂ ＂.*＂ ＂.*＂
    rabbitmqctl set_permissions -p airflow ID ″.*″ ″.*″ ″.*″
    rabbitmqctl set_permissions -p airflow ID ′.*′ ′.*′ ′.*′
    rabbitmqctl set_permissions -p airflow ID ‘.*’ ‘.*’ ‘.*’
    rabbitmqctl set_permissions -p airflow ID ＂.*＂ ＂.*＂ ＂.*＂


정말 많은 시도를 하였지만.. 다 실패하였다.
```
http://host:15672 접속

<figure>
  <img src="/assets/images/2019-04-25-Airflow/rabbitmq_0.PNG">
  <figcaption></figcaption>
</figure>

<figure>
  <img src="/assets/images/2019-04-25-Airflow/rabbitmq_1.PNG">
  <figcaption></figcaption>
</figure>

```
> vi ~/airflow/airflow.cfg

  executor = CeleryExecutor
  sql_alchemy_conn = mysql://id:password@host:port/db
  broker_url = amqp://airflow_ID:airflow_PASSWORD@rabbitmq_server_ip:5672@vhost_name
  result_backend = db+mysql://id:password@host:port/db_backend
```
```
> airflow worker

 ModuleNotFoundError: No module named 'celery'
  > conda install -y -c conda-forge celery
```

