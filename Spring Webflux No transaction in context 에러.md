### 환경 

* spring boot 2.x.x 
* kotlin 
* webflux 
* r2dbc

### 설명 

* 현재 트랜잭션의 싱크 쪽을 가지고 올 때 에러가 발생이 되는 것으로 보여진다. 

<img width="1806" alt="스크린샷 2023-01-27 오후 11 06 25" src="https://user-images.githubusercontent.com/53357210/215106381-550a7ca2-166f-4505-8bf8-8256781fc330.png">


### 원인 분석 

* `ConnectionFactoryUtils` 클래스에서 `getConnection` 을 호출을 해주고 있는데 이떄 `doGetConnection` 을 호출을 해준다. 

<img width="701" alt="스크린샷 2023-01-28 오전 11 45 06" src="https://user-images.githubusercontent.com/53357210/215238131-aacd5914-23ba-4444-946c-e04ab674ef6d.png">

* `doGetConnection` 에서 `TransactionSynchronizationManager.forCurrentTransaction()` 을 통해서 현재 호출을 해주는데 이때 `onErrorResume` 를 통해서 해당 에러가 발생이 되는 것으로 보여진다. 

<img width="776" alt="스크린샷 2023-01-28 오전 11 43 55" src="https://user-images.githubusercontent.com/53357210/215238092-41825bad-5e82-4a30-8664-bccf0b4f322d.png">

### 처리 과정 

* 처리 과정의 경우 다음 과 같다. 

* MileageCustomRepository 레파지토리 클래스에서 Transactional 어노테이션을 통해서 트랜잭션 처리를 해준다. 
* 처리를 해준 뒤 상속 받은 클래스를 open 클래스로 변경을 해준다. 


### 코멘트



### 관련 이슈 

https://github.com/jgspark/kotlin-webflux/issues/18#issue-1559827527
