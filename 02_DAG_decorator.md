
## 목차

* [1. DAG Decorator 개요](#1-dag-decorator-개요)
* [2. DAG Decorator 예시 코드](#2-dag-decorator-예시-코드)
* [3. 참고](#3-참고)

## 1. DAG Decorator 개요

DAG 의 decorator 는 **Airflow version 2.0** 에 추가된 것으로, **Decorator 를 이용하여 DAG 을 생성** 하는 방법이다.

## 2. DAG Decorator 예시 코드

```python
@dag(
    schedule="1 0 * * *",
    start_date=pendulum.datetime(2021, 12, 1),
    catchup=False,
    tags=["ive", "dive"]
)
def example_dag_decorator():
    """Example docstring."""
    ...
```

## 3. 참고

* [Python Decorator 설명](https://github.com/WannaBeSuperteur/Python-study-2026/blob/main/Python_Clean_Code_2nd_Edition/05_Python_Decorator.md)
