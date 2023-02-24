# Test Case 작성시 DB별 function 사용시 h2 db 에 적용하기

테스트 케이스 작성시 H2 데이터 베이스에서 Mysql Function 사용하기 

테스트를 하다보면 , 각각의 DB 에 Function 을 사용할 떄가 존재를 한다. 

## 방법 

* test case 의 클래스 내부에서 선언을 해준다.

```java
class ValueTest {

    public static Long MethodName(String value) {
        return null;
    }
}
```

* 선언된 MethodName 과 Package Name 그리고 클래스명을 sql 스크립트 내에 지정을 해준다. 

```sql
CREATE ALIAS IF NOT EXISTS {MethodName} FOR "{packageName.ClassName.MethodName}";
```
