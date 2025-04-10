---
상태: 완료
tags:
  - HTTP
시작일: 2024-12-20
마감일: 2024-12-20
---
## 인터넷 프로토콜 스택의 4계층
>애플리케이션 - HTTP, FTP
전송 계층 - TCP, UDP
인터넷 계층 - IP
네트워크 인터페이스 계층

## 프로토콜 계층
![](https://i.imgur.com/zzsSWgB.png)

## IP 패킷 정보
출발지 IP와 목적지 IP 등을 가진다.

## TCP/IP 패킷 정보
**TCP 세그먼트를 IP 패킷으로 감싼다.**
TCP 세그먼트 정보 : 출발지 PORT, 목적지 PORT, 전송 제어, 순서, 검증 정보 등

## TCP(Transmission Control Protocol) 전송 제어 프로토콜 특징
**연결지향** : TCP 3 way handshake (가상 연결) 메세지를 수신하는 쪽과 연결을 한 다음 메세지를 전송한다.
**데이터 전달 보증** : 메세지 수신 여부를 알려준다. (전송제어)
**순서 보장**(흐름제어)
**신뢰할 수 있는 프로토콜**(오류제어)
**현재는 대부분 TCP 사용**

## TCP 3 way handshacke
![](https://i.imgur.com/9wGpWYC.png)
SYN: 접속 요청
ACK: 요청 수락
참고: 3. ACK와 함께 데이터 전송 가능

## TCP 통신 단계
### MSS(Maximum Segment Size) 단위
- MSS - TCP로 전송할 수 있는 최대 페이로드 크기
- TCP 헤더 크기는 제외
![](https://i.imgur.com/zDTnDrs.png)

### TCP 세그먼트 구조
![](https://i.imgur.com/Vu9KZOs.png)
- 송신지 포트, 수신지 포트
	- 송수신하는 포트 번호

포트번호
> 실행중인 애플리케이션을 식별하는 번호 16비트로 표현하기 때문에 포트번호의 범위는 2의 16승 -1

- 순서 번호(sequence number)
	- 순서 번호가 명시되는 필드
	- 순서 번호
		- 송수신되는 세그먼트 데이터의 첫 바이트에 부여되는 번호
		- 세그먼트의 올바른 송수신 순서를 보장하기 위한 번호

- 확인 응답 번호(acknowledgment number)
	- 상대 호스트가 보낸 세그먼트에 대한 응답
	- 다음으로 수신하기를 기대하는 순서 번호가 명시



### 응용 계층으로부터 1900바이트 크기의 데이터를 전달받았다고 가정
![](https://i.imgur.com/H8djyxF.png)

### MSS 단위로 전송
- 편의상 MSS가 500바이트라고 가정(네 개의 세그먼트로 분할 500바이트씩)
![](https://i.imgur.com/J5Qwiii.png)

### 순서 번호 = 초기 순서 번호 + 송신한 바이트 수
- 초기 순서 번호(ISN, Initial Sequence Number)는 무작위 값
- 세그먼트 B의 순서 번호는 초기 순서 번호인 100에서 500바이트 떨어진 셈으로 600
- 세그먼트 C의 순서 번호는 초기 순서 번호에서 1000바이트 떨어진 1100
- 세그먼트 D의 순서 번호는 초기 순서 번호에서 1500바이트 떨어진 1600
![](https://i.imgur.com/d0fmidG.png)

### 확인 응답 번호
- 수신자가 다음으로 받기를 기대하는 순서 번호(일반적으로 '수신한 순서 번호 + 1')
- 확인 응답 번호 값을 보내기 위해서는 제어 비트에서 승인을 나타내는 비트인 ACK 플래그를 1로 설정
![](https://i.imgur.com/S0lJhEx.png)

### 제어비트(control bits) 또는 플래그 비트(flag bits)
- 현재 세그먼트에 대한 부가 정보
- 기본적으로 8비트로 구성
	- ACK: 세그먼트의 승인을 나타내기 위한 비트
	- SYN: 연결을 수립하기 위한 비트
	- FIN: 연결을 종료하기 위한 비트

### 윈도우(window)
- 수신 윈도우(한 번에 수신하고자 하는 데이터의 양)의 크기가 명시

### 연결 수립: 3way handshake
- 세 개의 단계로 이루어진 TCP의 연결 수립 과정
![](https://i.imgur.com/Wm1OMo5.png)
 
 - 호스트 A와 B가 쓰리 웨이 핸드셰이크를 한다고 가정
![](https://i.imgur.com/OPu1KOd.png)


### 연결 수립
- 액티브 오픈(active open): 연결 시작 호스트의 연결 수립 과정
- 패시브 오픈(passive open): 연결 수락 호스트의 연결 수립 과정
![](https://i.imgur.com/85LqBIH.png)

### 연결 종료
- 송수신 호스트가 각자 한 번식 FIN과 ACK를 주고 받으며 TCP가 연결 종료
	- 액티브 클로즈(active close): 종료 시작 호스트의 종료 과정
	- 패시브 클로즈(passive close): 종료 수락 호스트의 종료 과정
![](https://i.imgur.com/u7bQFvl.png)
**네 단계로 연결을 종료한다는 점에서 포 웨이 핸드셰이크(four-way handshake)라고 부리기도 한다.**

![](https://i.imgur.com/7eMw0vW.png)

### TCP 상태
상태(state): 현재 어떤 통신 과정에 있는지 나타내는 정보
상태를 유지하고 활용하는 TCP
- 스테이트풀(stateful) 프로토콜

### 연결이 수립되지 않은 상태
CLOSED - 아무런 연결이 없는 상태
LISTEN - 연결 대기 상태(SYN 세그먼트를 기다리는 상태)
- 서버로서 동작하는 패시브 오픈 호스트는 일반적으로 LISTEN 상태 유지
- LISTEN 호스트에게 STN 세그먼트를 보내면 3way handshake 시작

### 연결 수립 상태
**SYN-SENT**
- 연결 요청을 보낸 뒤 대기하는 상태
- 액티브 오픈 호스트가 SYN 세그먼트를 보낸 뒤 그에 대한 응답인 SYN + ACK 세그먼트를 기다리는 상태
**SYN-RECEIVED**
- 패시브 오픈 호스트가 SYN + ACK 세그먼트를 보낸 뒤 그에 대한 ACK 세그먼트를 기다리는 상태
**ESTABLISHED**
- 연결이 확립되었음을 나타내는 상태

### 연결 종료 상태
**FIN-WAIT-1**
- 일반적인 TCP 연결 종료 과정에 있어 FIN-WAIT-1은 연결 종료의 첫 단계
**CLOSE-WAIT**
- 종료 요청인 FIN 세그먼트를 받은 패시브 클로즈 호스트가  그에 대한 응답으로 ACK 세그먼트를 보낸 후 대기하는 상태
**FIN-WAIT-2**
-  FIN-WAIT-1 상태에서 ACK 세그먼트를 받고 상대 호스트의 FIN 세그먼트를 기다리는 상태
**LAST-ACK**
- CLOSE-WAIT 상태에서 FIN 세그먼트를 전송한 뒤 이에 대한 ACK 세그먼트를 기다리는 상태
**TIME-WAIT**
- 액티브 클로즈 호스트가 FIN 세그먼트를 수신한 뒤, 이에 대한 ACK 세그먼트를 전송한 뒤 접어드는 상태
- 패시브 클로즈 호스트는 마지막 ACK 세그먼트를 수신하면 CLOSED 상태로 전이 
- **TIME-WAIT 상태의** **액티브 클로즈 호스트는 일정 시간을 기다린 뒤** CLOSED 상태로 전이 