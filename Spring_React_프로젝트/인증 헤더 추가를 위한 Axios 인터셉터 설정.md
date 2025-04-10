---
상태: 완료
시작일: 2024-05-17
마감일: 2024-05-17
---
애플리케이션에는 많은 API들이 있다.
매번 전송 때마다 많은 API에 별도로 Header를 추가하기에는 많은 노력이 필요하다.
Token을 공통적으로 적용할 방법을 알아보겠다.

현재 두 개의 API가 있기 때문에 두 API에서 통합으로 사용하는 API를 만들겠다.
![](https://i.imgur.com/6yx7GYN.png)

### ApiClient를 생성하고 여기에서 Axios 관련 요청을 처리
- 일단은 간단하게 8080에 요청하는 코드만 삽입
![](https://i.imgur.com/krdjbWO.png)

### Axios 인터셉터 기능을 사용해서 요청을 가로채서 요청에 필요한 구성을 해준다.
![](https://i.imgur.com/zA6Wgko.png)

코드를 바꾼 후에 그전에는 실행되지 않던 Todos 가 실행된다.
![](https://i.imgur.com/79SAPbs.png)

AuthContext에서 Aixos.interceptor를 사용하기 때문에 HelloWolrdApiService에 token을 넘겨주지 않아도 정상적인 요청을 할 수 있다.
![](https://i.imgur.com/cvmFNJ0.png)
![](https://i.imgur.com/RZe2OLU.png)

물론 Todos에서 추가, 삭제, 업데이트도 정상적으로 작동한다.