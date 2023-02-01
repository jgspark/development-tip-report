# Spring Webflux No transaction in context 에러 발생

## 개요

### 설명 

* 발생된 상황의 경우 다음 과 같다. 
* R2dbc 에서 `R2dbcEntityOperations` 에서 `databaseClient` 를 가지고 Custom Sql 를 작성을 하기 위해서 Custom Repository 를 작성을 해주던 중 발생이 되었다. 
* 해당 내용을 분석을 해보 았을때 현재 상태의 트랜잭션을 가지고 올 수 없어서 발생이 된것으로 보여 진다. 


### 어플리케이션 스팩

```text 
* spring boot 2.x.x 
* kotlin 
* webflux 
* r2dbc
```

<img width="1806" alt="스크린샷 2023-01-27 오후 11 06 25" src="https://user-images.githubusercontent.com/53357210/215106381-550a7ca2-166f-4505-8bf8-8256781fc330.png">


## 에러

* `ConnectionFactoryUtils` 클래스에서 `getConnection` 을 호출을 해주고 있는데 이떄 `doGetConnection` 을 호출을 해준다. 

<img width="701" alt="스크린샷 2023-01-28 오전 11 45 06" src="https://user-images.githubusercontent.com/53357210/215238131-aacd5914-23ba-4444-946c-e04ab674ef6d.png">

* `doGetConnection` 에서 `TransactionSynchronizationManager.forCurrentTransaction()` 을 통해서 현재 호출을 해주는데 이때 `onErrorResume` 를 통해서 해당 에러가 발생이 되는 것으로 보여진다. 

<img width="776" alt="스크린샷 2023-01-28 오전 11 43 55" src="https://user-images.githubusercontent.com/53357210/215238092-41825bad-5e82-4a30-8664-bccf0b4f322d.png">


## 해결 방법

* 처리 과정의 경우 다음 과 같다. 

* MileageCustomRepository 레파지토리 클래스에서 Transactional 어노테이션을 통해서 트랜잭션 처리를 해준다. 
* 처리를 해준 뒤 상속 받은 클래스를 open 클래스로 변경을 해준다. 


## 참고

https://github.com/jgspark/kotlin-webflux/issues/18#issue-1559827527
