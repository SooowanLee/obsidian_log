---
상태: 진행중
tags:
  - HTTP
시작일: 2025-01-06
마감일:
---
## HTTP(HyperText Transfer Protocol)
- HTML, TEXT
- IMAGE, 음성, 영상, 파일
- JSON, XML(API)
- 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용

## HTTP 특징
- 클라이언트 서버 구조
- 무상태 프로토콜(스테이스리스), 비연결성
- HTTP 메세지
- 단순함, 확장 가능

### 클라이언트 서버 구조
**Request Response 구조**
클라이언트는 서버에 요청을 보내고, 응답을 대기
서버가 요청에 대한 결과를 만들어서 응답
![](https://i.imgur.com/y2tKYno.png)

서버와 클라이언트가 분리되어 있기 때문에 양쪽이 따로 문제를 해결할 수 있다.

### 무상태 프로토콜(스테이스리스/Stateless)
서버가 클라이언트의 상태를 보존X
장점: 서버 확장성 높음(스케일 아웃)
단점: 클라이언트가 추가 데이터 전송

#### Stateful, Stateless 차이
상태유지 - Stateful
![](https://i.imgur.com/pwZKsBY.png)

상태유지 - Stateful, 점원이 중간에 바뀌면?
![](https://i.imgur.com/Zlae0Oo.png)


정리
![](https://i.imgur.com/loK6TNo.png)


![](https://i.imgur.com/iUid3vk.png)

**상태유지**: 중간에 다른 점원으로 바뀌면 그 전에 알고 있던 정보를 함께 알려줘야 한다.

**무상태**: 중간에 다른 점원으로 바뀌어도 된다.
- 갑자기 고객이 증가해도 점원을 대거 투입할 수 있다.
- 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다.
무상태는 응답 서버를 쉽게 바꿀 수 있다. -> 무한 서버 증설 가능

### Stateless
실무한계

상태 유지
- 로그인
로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지
일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태 유지
최소한의 상태만 유지

### 비 연결성
- HTTP는 기본이 연결을 유지하지 않는 모델
- 일반적으로 초 단위의 이하의 빠른 속도로 응답
- 1 시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
	- 웹 브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.
- 서버 자원을 매우 효율적으로 사용할 수 있음

#### 한계와 극복
- TPC/IP 연결을 새로 맺어야 한다 - 3 way handshake 시간 추가
- 지금은 HTTP 지속 연결(Persistent Connections)로 문제 해결

### 가장 중요한 **HTTP 메세지 기능**
![](https://i.imgur.com/AIRlqQH.png)

#### REQUEST 시작라인
- start-line = request-line / status-line
- request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)

**HTTP 메서드**
- 종류 - GET, POST, PUT, DELETE...
- 서버가 수행해야 할 동작 지정
	- GET - 리소스 조회
	- POST - 요청 내역 처리

### HTTP 헤더
- HTTP 전송에 필요한 모든 부가정보
- 임의의 헤더 추가 가능

### HTTP 메시지 바디
- 실제 전송할 데이터
- HTML 문서, 이미지, 영상, JSON 등등 byte로 표현할 수 있는 모든 데이터 전송 가능