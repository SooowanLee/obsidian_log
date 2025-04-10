## SOP(Same-Origin-Policy)
> **같은 출처**의 자원만을 허용

#### 여기에서 "출처"라는 것은 다음 세 가지 요소로 구성된다.
아래 세가지 요소가 모두 같아야 동일한 출처로 본다.
- `프로토콜`:  http, https
- `호스트`: www.example.com
- `포트`: 80, 443
#### 예를 들어
- http://www.ecample.com와 http://www.example.com/page2는 **동일한 출처**이다.
	- 프로토콜:호스트:포트가 **모두 같고 Path만 추가**되었기 때문이다.
- 하지만, https://www.example.com과 http://www.example.com은 **다른 출처**이다.
	- 호스트:포트가 같더라도 **프로토콜이 다르기 때문**에 다른 출처이다.

기본적으로 웹 브라우저는 SOP를 사용한다. 그래서 다른 출처에서 API요청 등을 하면 막아버린다. 
하지만, 웹 애플리케이션의 경우 상호작용을 필요로 하는 경우가 많은데 다른 출저의 API를 모두 막아버리고 같은 출처의 자료만 사용할 수 있다면 너무 불편할 것이다.

---
**다른 출처의 자원을 사용하고 싶다면 CORS를 사용하면된다.**
## CORS(Cross-Origin-Resource-Sharing)
> **다른 출처**의 자원을 공유
> 추가 HTTP 헤더를 사용하여, 한 **출처**에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 **브라우저**에 알려주는 체제

**프로토콜:호스트:포트번호**중 하나라도 다르면 다른 출처라고 한다.
### CORS 접근제어 시나리오
- 단순 요청(Simple Request)
- 프리플라이트 요청(Preflight Request)
- 인증정보 포함 요청(Credentialed Request)

### Preflight(프리플라이트)
- OPTIONS 메서드를 통해 다른 도메인의 리소스에 요청이 가능한 지 확인 작업
- 요청이 가능하다면 실제 요청( Actual Request)을 보낸다.
![](https://i.imgur.com/vpj0Olj.png)

**PREFLIGHT REQUEST**
- Origin : 요청출처
- Access-Control-Request-Method : 실제 요청의 메서드
- Access-Control-Request-Headers : 실제 요청의 추가 헤더![](https://i.imgur.com/IXA7WY6.png)

**PREFLIGHT RESPONSE**
- Access-Control-Allow-Origin : 서버 측 허가 출처
- Access-Control-Allow-Methods : 서버 측 허가 메서드
- Access-Control-Allow-Headers : 서버 측 허가 헤더
- Access-Control-Max-Age : 응답 캐시 시간
![](https://i.imgur.com/BqTCxir.png)

**Preflight Response 특징**
- 응답 코드는 200대여야 한다.
- 응답 바디는 비어있는 것이 좋다.
---
### Simple Request
> Preflight 요청 없이 바로 본 요청을 보낸다.

#### 다음 조건을 모두 만족해야 한다.
- GET, POST, HEAD 메서드 중 하나여야한다.
- Content-Type / 아래 중 하나의 타입이어야한다.
	- application/x-www-form-urlencoded
	- multipart/form-data
	- text/plain
- 헤더는 Accept, Accept-Language, Content-Language, Content-Type만 허용된다.

### 왜 굳이 두번 요청을 하는 Preflight를 사용해야할까?
- 보안강화 : 민감한 데이터가 포함된 요청이 서버의 상태를 변경할 수 있는 요청이 있을 경우, 브라우저는 사전에 서버의 허가를 받습니다. 
	- 예를 들어서 **PUT, DELETE** 같은 **서버의 정보**를 **수정**하거나 **삭제**하는 요청을 **CORS가 없는 서버**가 받았다면 **이미 SERVER에서 요청을 처리**하고 **브라우저에 응답**을 하는데 
	- 그제서야 **브라우저**는 **CORS 에러를 발생**해서 **CLIENT에 보낸다**면 SERVER는 **이미 데이터**는 **수정, 삭제**한 후이기 때문에 **크리티컬한 문제가 발생**한다.
---
### Credentialed Request
> 인증 관련 헤더를 호함할 때 사용하는 요청

클라이언트
credentials: include

서버
Access-Control-Allow-Credentalis : true
특정 출처만 허용 가능 : Access-Control-Allow-Origin : *(와일드카드)를 사용할 수 없다.