---
시작일: 2024-05-17
마감일: 2024-05-17
상태: 완료
---
지금까지는 기본인증 문자를 하드코딩된 형태를 취하고 있었다. 
이것 바꿔보겠다.

### 기존코드
![](https://i.imgur.com/6qcgJc8.png)

### AuthContext를 통해서 token을 받아서 넘겨주는 형태
- Context API를 사용하기 위한 변수 추가
- Context API에 담겨있는 token을 추출해서 사용
![](https://i.imgur.com/n95hhyr.png)

### username, token을 받아서 백엔드로 요청
![](https://i.imgur.com/fZ4j7pD.png)