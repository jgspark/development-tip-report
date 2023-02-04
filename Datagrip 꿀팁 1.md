# Datagrip 꿀팁 1


## 그룹화 하는 방법

1. 데이터 항묵을 선택 한 뒤

![스크린샷 2022-08-30 오전 10 31 48](https://user-images.githubusercontent.com/53357210/216745301-6aa3aa10-76eb-400f-a1f8-307520ecb0be.png)

2. F6 혹은 우측 클릭 이로 폴더를 생성을 해서 그룹화

![스크린샷 2022-08-30 오전 10 31 55](https://user-images.githubusercontent.com/53357210/216745302-3d6950b8-7d60-418c-85b1-7fe6ca8d1a48.png)


## 실행 계획 view 변경 

1. 눈 모양 클릭 
2. transpose 클릭
3. header 가 좌측 1열로 이동


<img width="1235" alt="스크린샷 2023-02-04 오후 12 14 47" src="https://user-images.githubusercontent.com/53357210/216744984-e5cf945b-77d0-4020-a9e5-3eb66760cf6e.png">



## 퍼포먼스를 위한 check sql 

```sql
set profiling=1;
select SQL_NO_CACHE STRAIGHT_JOIN * from t1 a INNER JOIN t2 b ON a.c1=b.c1 and a.c1 between 1 and 50;
select SQL_NO_CACHE STRAIGHT_JOIN * from t1 a INNER JOIN t2 b ON a.c1=b.c1 and a.c1 between 1 and 500;
select SQL_NO_CACHE STRAIGHT_JOIN * from t1 a INNER JOIN t2 b ON a.c1=b.c1 and a.c1 between 1 and 5000;
show profiles;
```


<img width="1134" alt="스크린샷 2023-02-04 오후 12 14 16" src="https://user-images.githubusercontent.com/53357210/216744724-5c4aecb6-fd07-4c01-8b05-9863987d58ce.png">


