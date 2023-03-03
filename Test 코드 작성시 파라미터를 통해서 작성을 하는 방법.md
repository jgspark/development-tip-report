# Test 코드 작성시 파라미터를 통해서 작성을 하는 방법.md 

* junit 을 사용한 테스트 코드 작성을 하는 방법 중 파라미터를 통해서 작성을 하는 방법 

## 작성 방법 

* 테스트 케이스 작성시, 파라미터를 넘겨주기 위해서 static method 로 작성 후 @MethodSource 어노테이션을 사용해서 선언을 해준다.

```java
public static Stream<Arguments> getArgs() {
return Stream.of(
 Arguments.of(1),
 Arguments.of(2)
);
}

@DisplayName("파라미터 테스트 시나리오 입니다.")
@ParameterizedTest(name = "{0} 을 집어 넣습니다.")
@MethodSource("getArgs")
public void param(int n) {
  assertTrue(n != 0);
}
```

* 테스트 케이스 작성시, 파라미터를 넘겨주기 위해서 ArgumentsProvider 상속 받아서 메소드에 파라미터를 넘겨준다.

```java
public class SampleArgs implements ArgumentsProvider {

    @Override
    public Stream<? extends Arguments> provideArguments(ExtensionContext context) throws Exception {
        return Stream.of(
            Arguments.of(1),
            Arguments.of(2)
        );
    }
}


@DisplayName("파라미터 테스트 시나리오 입니다.")
@ParameterizedTest(name = "{0} 을 집어 넣습니다.")
@ArgumentsSource(SampleArgs.class)
public void param(int n) {
  assertTrue(n != 0);
}
```
<img width="906" alt="스크린샷 2023-03-03 오후 11 06 40" src="https://user-images.githubusercontent.com/53357210/222740671-cd1fe8cd-906b-4def-8a33-34d608928702.png">


