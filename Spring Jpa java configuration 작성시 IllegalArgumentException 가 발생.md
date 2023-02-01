# Spring Jpa java configuration 작성시 IllegalArgumentException 가 발생

## 개요 

* Spring Batch 프로젝트를 진행을 하던 도주 기존에 클론 코딩으로 진행하는 과정에서는 발생이 되지 않았던 에럭 발생이 됨 

* 해다 에러의 경우 `LocalContainerEntityManagerFactoryBean` 에서 `IllegalArgumentException` 가 발생이 되느 현상이로 확인을 하였다. 

* JPA 에서 `EntityManagerFactoryBean` 해당 Bean을 등록을 하다가 발생이 된다.


**에러 로그** 

``` 
No PersistenceProvider specified in EntityManagerFactory configuration, and chosen PersistenceUnitInfo does not specify a provider class name either
```

<img width="1401" alt="스크린샷 2023-02-01 오후 10 24 17" src="https://user-images.githubusercontent.com/53357210/216055250-565a1dbd-8529-469d-a104-92c4187a1611.png">


### 에러의 추적 


* 디버그를 통햇 볼 때 , `LocalContainerEntityManagerFactoryBean.class` 에서 `this.persistenceUnitInfo.getPersistenceProviderClassName()` 의 함수를 호출 후 에러가 발생이 된다.

<img width="1123" alt="스크린샷 2023-02-01 오후 10 26 31" src="https://user-images.githubusercontent.com/53357210/216055821-362d527f-c49d-4154-af0f-64b2c017903b.png">


* 해당 호출 과정을 디버그를 통해서 보면, `AbstractEntityManagerFactoryBean` 클레스 에서 `afterPropertiesSet` 에서 호출을 해준다. 
* 호출을 해주는데 해당 함수의 경우 JPA 설정을 셋업 시켜주고 만약 없으면 `buildNativeEntityManagerFactory` 를 호출을 한다. 
* 해당 메소드의 경우 EntityMangerFactory 를 만드느 메소드로 보여진다. 
* `createNativeEntityManagerFactory` 함수를 호출을 해주는데 추상화를 시켜주기 때문에 내가 상속 받아서 호출를 시켜주는 `LocalContainerEntityManagerFactoryBean` 에서 에러가 발생을 하느 것으로 보여진다.  


<img width="1003" alt="스크린샷 2023-02-01 오후 10 30 32" src="https://user-images.githubusercontent.com/53357210/216056832-8715eb11-7892-4662-9632-e238721badba.png">


## 해결 방법 

* 해결 방법은 간단하다. entitymangerfactorybean을 설정해주는 곳에서 `jpaVendorAdapter` 를 셋업 해주면 된다.  


<img width="907" alt="스크린샷 2023-02-01 오후 10 34 41" src="https://user-images.githubusercontent.com/53357210/216057736-309a00cc-f603-4492-a689-60e81aa694ff.png">


* 셋팅 후 디버그를 통해서 보면, `PersistenceProvider provider = getPersistenceProvider();` 가 null 이 아닌것이 보여진다.

<img width="748" alt="스크린샷 2023-02-01 오후 10 22 01" src="https://user-images.githubusercontent.com/53357210/216057919-5d1b69f8-64b4-45f4-97ce-2dece6a13f71.png">


