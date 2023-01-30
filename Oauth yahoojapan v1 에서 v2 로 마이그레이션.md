

### 현상 발견

* 갑작스러운 야후재팬 oauth 버전 업데이트가 발생을함 
* v1 의 http status 가 404 가 발생을 하였다. 
* 발생 시점에서 docs 를 보았을 때 해당 스팩이 정의 되지 않아서 end point 가 사라진 것으로 판단을 하고 curl 을 통해서 테스트 후 v2 로 마이그레이션 진행



### 체크 방법 및 해결법

* curl 로 먼저 API 테스트를 진행을 하였다. 
* 테스트를 진행을 하면서 docs 의 스팩과 동일한지 체크 후 v2 도 동일하게 체크 후 
* v1 을 v2 로 마이그레이션 진행

```
* 성공시 http status 200 call
* 만약 http status 401 call 이면 token 에러 
* 만약 http status 5xx call 이면 서버 에러 
```

* v1 curl 

```bash 
curl https://userinfo.yahooapis.jp/yconnect/v1/attribute \
-H 'Authorization: Bearer {token}' -v
```

* v2 curl 

```bash 
curl https://userinfo.yahooapis.jp/yconnect/v2/attribute \
-H 'Authorization: Bearer {token}' -v
```


### About.

- [공식 github](https://github.com/yahoojapan/yconnect-servlet-sdk)
- [야후 제팬1](https://developer.yahoo.co.jp/yconnect/v2/userinfo.html)
- [야후 제팬2](https://developer.yahoo.co.jp/yconnect/v1/userinfo.html)
