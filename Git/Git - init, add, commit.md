---
상태: 완료
tags:
  - Git
---

### Git이 왜 필요한가? 
- 내 소스 코드를 저장(버전 관리)
- 소스코드 공유
- 협업하는 공간

### Git 사용법
git 명령어 -option 으로 이루어졌다.
ex) git add, git commit, git push 등

**git init**을 하면 **.git**이라는 폴더와 함께 깃이 관리하는 폴더가 되는것을 알 수 있다.
여기서 **.** 은 숨겨진 폴더라는 의미이다. 생성된 **.git** 폴더를 삭제하고 싶다면 **rm -rf .git** 명령어를 사용하면 된다.

### Git Workflow
총 세가지의 작업 환경으로 나누어져 있다.
- **working directory**: 프로젝트의 파일들을 수정하는 곳
- **staging area**: 버전 히스토리에 저장할 준비된 파일들이 있는 곳
- **repository**: 버전의 히스토리를 가지고 있는 저장소

#### working directory
**working directory**에는 **untracked** | **tracked** 이 있다. 그 중에 **tracked**안에는 **unmodified** | **modified** 이 있다. **새로 생성된 파일**은 **working directory**의 **untracked**상태이다. 

만약 **test.txt** 파일 있다고 할때 **git add test.txt**를 하게되면 **tracked 상태**가 된다. 그리고 **test.txt** 파일은 **staging area**에 있게 된다. **tracked 상태**에서 **test.txt** 파일에 **변경이 생긴**다면 **modified 상태**가 된다. 그리고 **test.txt** 파일은 **working directory**에 있게 된다. 여기서 **git add test.txt**를 하게 되면 다시 **staging area**에 있게되고 **commit될 준비**를 하고 있다.

그리고 git add 를 해서 **tracked 상태**가 된 파일을 **untracked 상태로 만들고 싶다**면 **git restore --staged 파일.확장자|(test.txt)** 를 하면 된다.

#### staging area
commit을 하기전에 commit될 준비가 된 파일들이 모여 있는 장소

#### git commit
staging area에 모여있는 파일들을 git commit -m "커밋합니다." 을 해주면 repository로 이동하게 된다.
### repository에 어느정도 주기로 Commit 하는 것이 좋을까?
1. 프로젝트를 초기화 하는 커밋
2. 작은 단위로 의미있는 작업을 했을 때 커밋을 한다.(LoginService module, LoginRepository module, homPage, welecome)
