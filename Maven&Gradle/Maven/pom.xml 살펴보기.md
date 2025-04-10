---
상태: 완료
시작일: 2024-05-18
마감일: 2024-05-18
---
### pom.xml(Project Object Model)
> Maven 프로젝트의 핵심 구성 파일
> 프로젝트의 **메타데이터**, **의존성**, **빌드 구성**, **플러그인** 등을 정의

예를 들어 Spring, Spring MVC, Spring AOP, JPA, Hibernate 등 사용할 수 있는데 이런 라이브러리 및 프레임워크를 Maven 의존성이라한다.
의존성은 전부 pom.xml에 정의할 수 있다.

### 의존성 추가를 아무것도 하지 않아서 기본 Spring Boot만 있다.
![](https://i.imgur.com/8g2YcKE.png)

### pom.xml에 의존성을 정의하면 관련된 많은 의존성들이 같이 들어온다. 이런 의존성을 **전이 의존성**이라 한다.
- Spring-boot-starter-web이라는 직접 의존성에 관련된 톰캣등 다른 의존성(전이 의존성)도 같이 불러와졌다.
![](https://i.imgur.com/w39xaxA.png)
![](https://i.imgur.com/fqmoSJm.png)

### 의존성 관리
사용하고 싶은 직접 의존성을 추가하면 직접 의존성과 관련된 전이 의존성까지 같이 불러온다.
위에서는 3개의 직접 의존성이 있지만 External Libraries를 보면 수많은 전이 의존성이 같이 들어온 것을 볼 수 있다.

