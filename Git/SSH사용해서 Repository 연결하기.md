---
상태: 진행중
tags:
  - Git
---
## 문제 발생
git push origin main 명령어를 실행했을 때 git@github.com: Permission denied (publickey). fatal: Could not read from remote repository.라는 오류가 발생했다면, 이는 SSH 인증 문제로 인해 GitHub 원격 저장소에 접근할 수 없다는 뜻입니다.

### 가능한 원인과 해결 방법
**SSH 키가 설정되지 않았거나 SSH 에이전트에 추가되지 않음**
#### 해결 방법 :
- SSH 키가 있는지 확인 `ls -al ~/.ssh`
	-  **id_rsa**나 **id_ed25519** 같은 파일(및 .pub 파일)이 있는지 확인합니다.
- 키가 없다면 새로 생성
	- `ssh-keygen -t ed25519 -C "your_email@example.com"`
	- 기본 위치를 수락하려면 Enter를 누른다.
- SSH 에이전트에 키를 추가한다.
	- `ssh-add ~/.ssh/id_ed25519`
- Enter passphrase (empty for no passphrase)
	- 비밀번호를 설정합니다.
- 출력된 키를 복사한 후, GitHub의 설정 > SSH 및 GPG 키 > 새 SSH 키 추가에서 붙여넣습니다.
	- `'ssh-rsa'`, `'ecdsa-sha2-nistp256'`, `'ecdsa-sha2-nistp384'`, `'ecdsa-sha2-nistp521'`, `'ssh-ed25519'` 등으로 시작하는 키를 넣고 Add SSH key를 누른다.

### 연결에 성공한 경우
`git remote -v`로 연결된 repository를 확인합니다.
이제부터 기존에 작업하던 대로 작업을 이어가면 된다.