---
상태: 완료
tags:
  - Git
---

### 브랜치 생성 git branch 브랜치명
브랜치를 생성하기 위해서 branch라는 명령어를 사용한다. **실제 사용 : git branch login(브랜치이름은 작업하는 의미있는 이름으로 하면 된다.)**
### 브랜치 이동 git switch 브랜치명
git switch 이동할 브랜치명
- `git switch test`
### 브랜치 합치기는 기준 브랜치 이동 후 git merge 브랜치명
만약 `test` 브랜치에서 작업을 하다 `main` 브랜치와 합치려면 `git switch main` 으로 돌아와서
`git merge main`을 하면 된다. 
### 충돌 해결은 코드 고치고 git add & git commit
`merge`를 하다가 충돌이 나는 경우 
![](https://i.imgur.com/JOTSBe0.png)

충돌되었다는 메세지가 보인다.
![](https://i.imgur.com/hyJsjr4.png)

남기고 싶은 메세지를 제외하고 지운다.
![](https://i.imgur.com/Y04tkce.png)

다시 `git add, commit` 를 해준다.
![](https://i.imgur.com/5jyvN6v.png)

정상적으로 `merge`가 되었다.
![](https://i.imgur.com/mxDSdD2.png)

### git merge
1. 중심브랜치로 이동
2. git merge 새로운브랜치명(ex: login) 
	- 중심 브랜치가 main 이라면 main으로 이동해서 git merge login을 한다.
**보통 merge**는 **3-way-merge**가 된다. 

### git merge를 하고 나면 사용한 branch는 삭제한다.
git branch -d 브랜치명
![](https://i.imgur.com/8QbA8vI.png)

### rebase를 사용하는 이유
> 간단하고 짧은 브랜치들은 rebase를 하면 git history, log를 출력할 때 깔끔하게 보인다.
### git rebase & merge
**rebase**는 **브랜치 이동이 가능**하다. **A라는 브랜치**를  **main 브랜치**의 **제일 최근 commit에 이동** 시키고 **merge를 하면 fase-forward merge**를 할 수 있다.
1. 새로운 브랜치로 이동
2. git rebase 중심브랜치명
3. 중심브랜치로 이동
4. git merge 새로운브랜치명
5. fase-forward merge가 실행된다.


❗**단점** : 기존 커밋을 억지로 다른 곳에 이어 붙이는 것이기 때문에 conflict가 많이 발생할 수 있다. merge에서 해결하는 것처럼 필요한 메세지만 남기고 지워준다 그리고 다시 git add, commit을 한다.

### squash & merge
> rebase와 마찬가지로 history를 깔끔하게 유지할 수 있다.
- git merge --squash 브랜치명