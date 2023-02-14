# Mysql Index  & Full Scan Hint

## 개요

SQL문을 인덱스를 강제하거나 제외를 할 떄 사용을 한다. </br>
보편적으로 운영을 하다 보면, 많은 index 가 존재를 하고 해당하는 쿼리에서 index를 타지 않는 경우도 발생을 한다. </br>



## 샘플

* USE INDEX : `Index1` 이라는 인덱스를 사용을 한다는 것이다.

```sql
SELECT a
,b
,c
FROM Table
  USE INDEX (Index_Name)
WHERE a=1 AND b=2 AND c=3;
```

* IGNORE INDEX : `Index1` 이라는 인덱스를 제외 한다.

```sql
SELECT a
, b
FROM Table
  IGNORE INDEX Index1
WHERE a=1 AND b=2 AND c=3;
```

* USE INDEX FOR JOIN : JOIN 키워드는 테이블간 조인뿐 아니라 레코드 검색하는 용도까지 포함
* USE INDEX FOR ORDER BY : 명시된 인덱스를 ORDER BY 용도로만 사용하도록 제한
* USE INDEX FOR GROUP BY : 명시된 인덱스를 GROUP BY 용도로만 사용하도록 제한

```sql
SELECT * 
FROM TABLE
  USE INDEX (Index1)
  IGNORE INDEX (Index2) FOR ORDER BY
  IGNORE INDEX (Index3) FOR GROUP BY
WHERE a=1 AND b=2 AND c=3;
```
