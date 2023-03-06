# git 이미 push 한 commit 합치기


* 종종 commit 이 된 이후 squash 로 깔끔한 commit line 을 유지하 필요가 있다. 
* 해당 하는 사항의 경우 commit & push 이후에 commit 을 squash 를 통해서 합치는 방법을 설명한다. 


### 사용법

* 처음 (Head) 에서 2개 까지를 의미를 한다. 
* 만약 처음 부터 4개 까지 하고 싶다면, HEAD~4 로 하면 된다.

```
git rebase -i HEAD~2
```

<img width="568" alt="스크린샷 2023-03-06 오후 10 01 02" src="https://user-images.githubusercontent.com/53357210/223117723-ef11d2be-ad11-454a-ab5d-97500614de87.png">

* 맨 아래의 `pick` 을 `squash` 바꾸어 준다.
* 바꾼뒤, `x:` 을 입력 한다.


<img width="766" alt="스크린샷 2023-03-06 오후 10 01 39" src="https://user-images.githubusercontent.com/53357210/223117718-3ecfc1cb-195a-425a-86ec-3d1ad9c72496.png">

* 그리고 # 이 붙어 있지 않는 라인이 각각의 commit message 를 의미 하며, 해당 하는 부분이 합쳐진 commit message 로 올라가게 된다. 
* 필요에 따라 수정한 뒤 `wq:` 을 입력 후 
* push 를 해준다.

```
git push --force origin {브랜치명}
```
