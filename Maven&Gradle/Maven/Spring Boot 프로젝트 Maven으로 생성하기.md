---
상태: 완료
시작일: 2024-05-18
마감일: 2024-05-18
---
**본 게시글은 인텔리제이 IDE를 기준으로 작성되었습니다.**
**이클립스 등 다른 IDE에서는 다르게 보일 수 있습니다.**
### https://start.spring.io/ 접속
#### 주의!
> Spring Boot 3.x.x부터는 자바 17이상을 사용해야 한다.

### 3.x.x 이상부터 자바17을 사용해야하는 이유
- 새로운 기능
- 성능향상
- 보안 개선
- 장기 지원(LTS)


Maven프로젝트를 만들 것이기 때문에 당연히 Project에서 Maven을 체크한다. 
### 프로젝트 구성
- Project : Maven
- Language : Java
- Spring Boot : 3.x.x / 스냅샷 버전은 현재 개발 중인 버전이라 절대 사용하면 안된다.
- Group: study.maven
- Artifact: learn-maven
- 나머지는 손대지 않는다.
![](https://i.imgur.com/FLJx6gC.png)
`Group` : 클래스의 패키지 이름과 비슷하다.
`Artifact`: 다른 프로젝트에서 해당 프로젝트를 이용하고 싶을 때 참조

  이제 자신이 사용하는 IDE를 사용해서 프로젝트를 불러온다.
  처음에는 프로젝트를 구성하느라 시간이 조금 걸릴 수 있다.
![](https://i.imgur.com/TzRGTok.png)
Maven에서는 소스 파일을 src/main/java에 정의한다.
JUnit 테스트나 기타 모든 단위 테스트는 src/test/java에 작성된다.

다운받은 모든 Maven 의존성은 External Libraries에서 볼 수 있다.
![](https://i.imgur.com/5jEE8NF.png)
