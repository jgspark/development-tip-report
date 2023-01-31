

# 데드락에 자주 걸리는 쿼리문 

## 개요

* insert into select 시 데드락이 자주 발생이 된다. 

* 해당 쿼리 처럼 작성을 하는 것을 권장 하지 않는다. (보통 통계 솔루션에서 많이 사용하는 방법의 쿼리문)

* select 의 경우 lock 이 걸리지는 않지만,  insert into 와 같이 상용을 하면 `X 락` 에 걸리게 된다. 

```sql
INSERT INTO table2
SELECT * FROM table1
WHERE condition;
```

* 간략하게 설명을 하면 T1 에서 이미 해당 table1 의 값을 가지고 insert 문을 실행을 하게 되면 Row 레벨에서 락이 걸리게 된다. 
* 그렇지만 T2 에서 또한 동일한 구절로 select insert 를 하게 되면 이미 T1 에서 락이 잡혀 있어서 T2 에서 락이 걸리게 된는데 이때 pk 값이나 유니크 한 값이 아닐 경우 Gap Lock 이 발생하게 된다. 


## 해결 방법 

* 해당 쿼리의 경우 `예측을 하고 회피하는 방법` 으로 변경으로 진행을 하였다. 

* (상황에 맞게) 데드락에 걸렸을 때 해결하는 방법은 많지만 필자가 생각하기에는 해당 방법이 해당 로직에는 어울리는 방법이라고 판단을 하게 되었다.

```sql
### T1 에서 Select 를 먼저 조회 후 Insert 구문으로 작성 

select * from table1 

insert into table2 (val1 , val2)
values(xxx,xxx)
```

* 해당 쿼리 처럼 변경을 하게 되면 , T1 에서 Select 절로 웹 어플리케이션 메모리에 올려서 Insert 시킨다. 
* 또한 T2 가 Select 절에 접근을 해도 `X 락` 이 걸리지 않아서 데드락에 회피 할 수 있다.

