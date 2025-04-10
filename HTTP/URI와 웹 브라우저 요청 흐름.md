---
상태: 완료
tags:
  - HTTP
시작일: 2025-01-05
마감일: 2025-01-05
---
## URI(Uniform Resource Identifier)

## URI
>로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다. URI, URL, URN 중 가장 큰 개념

URL은 리소스의 위치로 자원을 식별 / 현재는 대부분 URL로 표현

**U**niform: 리소스 식별하는 통일된 방식
**R**esource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
**I**dentifier: 다른 항목과 구분하는데 필요한 정보

### URL(Resource Locator)
foo://example.com:8042/over/there?name=rerret#nose
scheme/authority/path/query/fragment

리소스의 위치로 리소스를 찾음
위치는 변할 수 있지만, 이름은 변하지 않는다.

scheme://[userinfo@]host[:port][/path][?query][#fragment]
https://www.google.com/443/search?q=hello&hi=ko


스키마는 주로 프로토콜 사용

프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
	http, https, ftp 등등
http/80, https/443 포트 생략 가능(대부분 URL에서 포트는 생략되어 있다.)
http에 보안이 추가되면 https(HTTP Secure)

URL은 보통 계층적 구조로 되어있다.

### 쿼리(query)
key=value 형태
?로 시작, &로 추가 가능 ?key1=value1&key2=value2
query parameter, query string으로 불림(데이터가 문자형태
로 넘어간다), 웹서버에 제공하는 파라미터 문자 형태