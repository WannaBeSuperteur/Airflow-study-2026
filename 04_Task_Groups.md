## 목차

* [1. TaskGroup 개요](#1-taskgroup-개요)
* [2. TaskGroup 의 작동 원리 및 설명](#2-taskgroup-의-작동-원리-및-설명)
* [3. TaskGroup 의 코드 작성 방법](#3-taskgroup-의-코드-작성-방법)

## 1. TaskGroup 개요

```TaskGroup``` (또는 ```TaskGroups```) 은 **DAG의 task를 계층적 그룹으로 나눈** 것이다.

* Airflow의 UI의 Graph View에서 그룹으로 묶인 task들을 직관적으로 확인할 수 있다.
* **반복되는 패턴** 이 나타나는 task를 나타낼 때 유용하다.

## 2. TaskGroup 의 작동 원리 및 설명

* ```TaskGroups``` 내부의 task들은 **같은 원래 DAG에 속하며, 해당 DAG의 설정값을 그대로 갖는다.**
* ```TaskGroup``` 내부의 task 간 의존 관계 역시 ```>>``` 와 ```<<``` operator 를 이용하여 나타낼 수 있다.

## 3. TaskGroup 의 코드 작성 방법

```TaskGroup``` 을 나타내기 위해서는 ```@task_group()``` 이라는 데코레이터를 사용한다.

* 예를 들어 다음 코드의 작동 원리는 다음과 같다.
  * ```task1```, ```task2```, ```task3``` 을 ```task_group1``` 에 속하게 한다.
  * 이어지는 task ```task4``` 를 ```task_group1``` 이후에 이어지는 task 로 한다.

```python
from airflow.sdk import task_group

@task_group()
def task_group1():
    task1 = XXXOperator(task_id="task1", ...)
    task2 = OOOOperator(task_id="task2", ...)
    task3 = ABCOperator(task_id="task3", ...)

task4 = OOOOperator(task_id="task4", ...)

task_group1() > task4
```
