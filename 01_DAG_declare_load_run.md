
## 목차

* [1. DAG 선언](#1-dag-선언)
  * [1-1. task 간 의존성](#1-1-task-간-의존성) 
* [2. DAG 로딩](#2-dag-로딩)
* [3. DAG 실행](#3-dag-실행)

## 1. DAG 선언

DAG의 선언은 다음과 같이 한다.

* DAG을 이루는 **각 task를 Operator 를 이용하여 실행** 하는 구조이다.

```python
 import datetime

 from airflow.sdk import DAG
 from airflow.providers.standard.operators.empty import XXXOperator

 with DAG(
     dag_id="{dag_id}",
     start_date=datetime.datetime(2026, 1, 1),
     schedule="@daily",
 ):
     XXXOperator(task_id="{task_id}")
```

* 또는 다음과 같이 **Operator 안에 DAG을 넣을 수도 있다.**

```python
XXXOperator(task_id="{task_id}", dag=my_dag)
```

* 다음과 같이 **함수 안에서 DAG을 정의** 하는 방법을 사용할 수도 있다.

```python
@dag(start_date=datetime.datetime(2026, 1, 1), schedule="@daily")
def generate_dag():
    XXXOperator(task_id="{task_id}")

generate_dag()
```

### 1-1. task 간 의존성

DAG의 각 task 간 의존성은 ```>>``` 또는 ```<<``` 을 이용하여 나타낼 수 있다.

* **downstream** task 는 ```first_task >> second_task``` 형태로 나타낸다.
* **upstream** task 는 ```third_task << fourth_task``` 형태로 나타낸다.

또는 다음과 같이 ```set_upsteam```, ```set_downstream``` 함수를 이용하여 나타낼 수도 있다.

```python
first_task.set_downstream(second_task)
third_task.set_upstream(fourth_task)
```

다음과 같이 **여러 개의 task를 동시에 수행** 하도록 할 수 있다.

* ```first_task >> [second_task, third_task]```
* ```first_task.set_downstream([second_task, third_task])```

## 2. DAG 로딩

* Airflow는 Python 코드에 정의된 **모든 DAG object** 를 load 할 수 있다.
  * 이것은 **1개의 Python 파일 당 여러 개의 DAG을 정의** 할 수 있다는 것을 나타낸다.
* 단, **Python 파일의 top level에 정의된 DAG object** 만을 로딩한다.
  * 즉, 다음과 같은 경우 ```dag_1``` 만 load 된다.

```python
dag_1 = DAG(...)

def load_dag_test():
    dag_2 = DAG(...)

load_dag_test()
```

## 3. DAG 실행

다음과 같이 DAG 실행 스케줄을 지정하여 DAG을 실행할 수 있다.

| 스케줄 설정                     | 설명                     |
|----------------------------|------------------------|
| ```schedule="@daily"```    | 매일 (```daily```) 자정 실행 |
| ```schedule="0 0 * * *"``` | 매일 자정 (```0 0```) 실행   |
| ```schedule="@once"```     | 한 번만 실행                |
