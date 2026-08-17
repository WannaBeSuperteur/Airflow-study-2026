
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

## 2. DAG 로딩



## 3. DAG 실행