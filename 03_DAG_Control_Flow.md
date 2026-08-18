## 목차

* [1. Control Flow 개요](#1-control-flow-개요)
* [2. branching](#2-branching)
* [3. Depends on Past](#3-depends-on-past)
  * [3-1. Trigger Rules](#3-1-trigger-rules)

## 1. Control Flow 개요

**Control Flow** 는 DAG의 task가 **이전 task가 모두 성공해야 진행되는 것** 이 아니라, **다른 규칙에 따라 진행** 되도록 하는 것이다.

* Control Flow 유형
  * [branching](#2-branching)
  * [Depend on Past](#3-depends-on-past)
  * Latest Only

## 2. branching

**branching** 은 DAG의 task 진행이 **여러 갈래로 나뉘도록** 하는 것을 말한다.

* 이때 ```@task.branch``` 데코레이터를 사용한다.

## 3. Depends on Past

Airflow는 기본적으로 **모든 upstream task (direct parents) 가 성공** 해야 다음 task를 진행하지만, 다음과 같이 ```trigger_rule``` 을 이용하여 이를 바꿀 수 있다.

### 3-1. Trigger Rules

Airflow의 대표적인 Trigger Rule은 다음과 같다.

| Trigger Rule                    | 설명                                                              |
|---------------------------------|-----------------------------------------------------------------|
| ```all_success``` **(default)** | 모든 upstream task 가 성공한 경우 실행                                    |
| ```all_failed```                | 모든 upstream task가 실패 또는 ```그 task의 upstream task가 실패``` 한 경우 실행 |
| ```all_done```                  | 모든 upstream task가 ```done``` 상태인 경우 실행                          |
| ```all_skipped```               | 모든 upstream task가 ```skipped``` 상태인 경우 실행                       |
| ```one_failed```                | 최소 1개의 upstream task가 실패                                        |
| ```one_success```               | 최소 1개의 upstream task가 성공                                        |
| ```one_done```                  | 최소 1개의 upstream task가 ```성공``` 또는 ```실패``` 한 경우 실행              |
