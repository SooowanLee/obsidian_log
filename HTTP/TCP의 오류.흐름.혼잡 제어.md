---
상태: 완료
tags:
  - HTTP
시작일: 2024-12-26
마감일: 2024-12-27
---
## TCP의 기능
재전송을 기반으로 다양한 **오류**를 **제어**
**흐름 제어**를 통해 처리할 수 있을 만큼의 데이터 송수신
**혼잡 제어**를 통해 네트워크가 혼잡한 정도에 따라 전송량 조절
![](https://i.imgur.com/0e5a4gK.png)

## 오류 검출과 재전송
**TCP 세그먼트의 체크섬 필트, 충분할까?**
- 송신 호스트가 송신한 세그먼트에 문제가 발생했음을 인지할 수 있어야한다.
- 오류를 감지하게 되면 해당 세그먼트를 재전송할 수 있어야한다.
**TCP는 언제 오류를 검출하고 재전송할까?**
- 중복된 ACK 세그먼트를 수신했을 때
- 타임아웃이 발생했을 때

## TCP의 재전송 기반 오류 제어
### 중복된 ACK 세그먼트를 수신했을 때
![](https://i.imgur.com/RVr0RC8.png)
### 타임아웃이 발생했을 때
호스트가 세그먼트를 전송할 때마다 재전송 타이머(retransmission timer) 시작
타임아웃이 발생할 때까지 ACK 세그먼트를 받지 못하면 재전송
![](https://i.imgur.com/XuJoIgP.png)

## 재전송 기법: ARQ(Automatic Repeat Request, 자동 재전송 요구)
**수신 호스트의 답변(ACK)과 타임아웃**을 **토대로** 문제를 진단하고 **문제가 생긴** **메시지**를 **재전송함**으로써 신뢰성을 확보하는 방식

## ARQ의 대표적인 세 가지 방식
**Stop-and-Wait ARQ**
**Go-Back-N ARQ**
**Selective Repeat ARQ**
![](https://i.imgur.com/NpiOlbE.png)
**TCP는 ARQ(Automatic Repeat Request)를 사용하는 대표적인 계층이자 프로토콜**

### Stop-and-wait ARQ
제대로 전달했음을 확인하기 전까지 새로운 메시지를 보내지 않는 방식
- 장점: 단순하다. 높은 신뢰성을 보장
- 단점: 네트워크의 이용 효율이 낮아지고 성능 저하

송신 호스트(A): 확인 응답을 받기 전에는 더 보내고 싶어도 못 보낸다.
수신 호스트(B): 더 많은 데이터를 처리할 수 있어도 하나씩만 확인 응답한다.
![](https://i.imgur.com/FtRlTtq.png)

### Stop-and-Wait ARQ 문제 해결 방법
각 세그먼트에 대한 ACK 세그먼트가 도착하기 전이더라도 여러 세그먼트를 보낼 수 있어야 한다.
**파이프라이닝(pipelining)** - 연속해서 메세지를 전송할 수 있는 기술
![](https://i.imgur.com/fbWaPJf.png)

### Go-Back-N ARQ
파이프라이닝 기반 ARQ 일종
여러 세그먼트 전송 중 **오류가 발생하면 해당 세그먼트부터 전부 재전송**
![](https://i.imgur.com/aElYpBx.png)

순서 번호 n번에 대한 ACK 세그먼트는 'n번만의' 확인 응답이 아닌 '**n번까지의 누적 확인 응답**'
누적 확인 응답(CACK, Cumulative Acknowledment)

## 빠른 재전송(fact retransmit)
재전송 타이머가 만료되기 전이라도 세 번의 동일한 ACK 세그먼트를 받았다면 곧바로 재전송![](https://i.imgur.com/UjLJgBJ.png)

### Selective Repeat ARQ
선택적으로 재전송: 각각의 패킷들에 대해 ACK 세그먼트를 보내는 방식
문제가 발생한 ACK 세그먼트만 재전송
Go-Back-N ARQ의 ACK 세그먼트가 누적 확인 응답이라면, Selective Repeat ARQ의 ACK 세그먼트는 개별 확인 응답(Selective Acknowledgment)

대부분의 호스트는 Selective Repeat ARQ 지원
Selective Repeat ARQ를 사용하지 않을 경우 Go-Back-N ARQ로 동작
![](https://i.imgur.com/0YPW2CR.png)

## 흐름 제어
### 파이프라이닝 기반 Go-Back-N ARQ/Selective Repeat ARQ
**수신호스트가 한 번에 얼마나 받아 처리할 수 있는지 반드시 고려해야한다.**
호스트가 한 번에 받아서 처리할 수 있는 세그먼트의 양에는 한계가 있다.

### TCP의 흐름 제어
송신 호스트가 수신 호스트의 처리 속도를 고려하며 송수신 속도를 균일하게 유지하는 기능
	-Stop-and-Wait ARQ를 사용하면 별도의 흐름 제어가 필요하지 않음

### 슬라이딩 윈도우(sliding window): TCP 흐름 제어 기법
윈도우(window) - 송신 호스트가 파이프라이닝할 수 있는 최대량
윈도우의 크기만큼 확인 응답을 받지 않고 한 번에 전송 가능하다.
![](https://i.imgur.com/efOeBSm.png)

### 송신 윈도우
수신 호스트는 **TCP 헤더(윈도우 필드)를 통해** 송신 호스트에게 자신이 받을 데이터의 양을 알려줌
송신 윈도우: 헤더로 전달받은 수신 윈도우 토대로 연산
![](https://i.imgur.com/swCTXtq.png)
![](https://i.imgur.com/Ybi8BqU.png)

## 혼잡 제어
### 혼잡(congestion)
많은 트래픽으로 인해 패킷의 처리 속도가 늦어지거나 유실될 우려가 있는 네트워크 상황
![](https://i.imgur.com/zEICIhz.png)

### TCP의 혼잡 제어(congestion control)
송신 호스트가 혼잡한 정도에 맞춰 유동적으로 전송량을 조절하는 기능
흐름 제어의 주체가 수신 호스트라면 혼잡 제어의 주체는 송신 호스트

혼잡 윈도우(congestion window) - 혼잡 없이 전송할 수 있을 법한 데이터양
	혼잡 윈도우가 크면? 한 번에 전송할 수 있는 세그먼트 수가 많음
	혼잡 윈도우가 작으면? (네트워크가 혼잡한 상황) 한 번에 전송할 수 있는 세그먼트 수가 적음

수신 윈도우는 수신 호스트가 헤더로 알려줌
혼잡 윈도우는 손신 호스트가 알아서 직접 계산해서 알아내야 함

송신 호스트는 어떻게 네트워크가 혼잡한지 알 수 있을까?
### TCP의 혼잡 제어 알고리즘
가장 기본적인 알고리즘인 AIMD(Additive Increase/Multiplicative Decrease)
	'합으로 증가, 곱으로 감소'
	혼잡이 감지되지 않는다면 - 혼잡 윈도우를 RTTT(Round Trip Time)마다 1씩 선형적으로 증가
	혼잡이 감지되면 - 혼잡 윈도우를 절반으로 떨어뜨리는 동작을 반복

![](https://i.imgur.com/PlL0F7p.png)
### RTT(Round Trip Time)
메세지를 전송한 뒤 그에 대한 답변까지 걸리는 시간
![](https://i.imgur.com/MdfgyJz.png)

### 송신호스트가 네트워크의 혼잡을 결정하는 척도
1. 중복된 ACK 세그먼트를 수신했을 때
2. 타임아웃이 발생했을 때


## AIMD(Additive Increase/Multiplicative Decrease) 의 고도화
### 느린 시작(slow start) 알고리즘
혼잡 윈도우를 1부터 시작해 문제 없이 수신된 ACK 세그먼트 하나당 1씩 증가시키는 방식
혼잡 윈도우는 RTT마다 2배씩 지수적으로 증가: 초기 전송 속도 빠른 확보

느린시작 임계치(slow start threshold)
혼잡 윈도우 값이 계속 증가하다가
	혼잡 윈도우 크기가 느린 시작 임계치 이상이 되거나
	타임아웃이 발생하거나
	세 번 중복된 ACK 세그먼트를 수신하면
	다음 세 가지 방법 중 하나를 선택
	![](https://i.imgur.com/JfaS9Ue.png)
### 혼잡 회피 알고리즘
RTT마다 혼잡 윈도우를 1MSS씩 증가시키는 알고리즘
혼잡 윈도우 크기 선형적으로 증가
"워워워": 느린 시작 임계치를 넘어서면 혼잡 여지가 있으니 조심해서 혼잡 윈도우 증가하기
![](https://i.imgur.com/oEx3S9J.png)

### 빠른 회복(fast recovery) 알고리즘
세 번의 중복된 ACK 세그먼트 수신: 빠른 재전송 + 빠른 회복 알고리즘
빠른 전송률 회복을 위해 느린 시작은 건너뛰고 혼잡 회피를 수행하는 알고리즘
단, 빠른 회복 도중이라도 타임아웃이 발생하면 다시 느린 시작을 수행

### 혼잡 제어 알고리즘 종합
![](https://i.imgur.com/zwHGloa.png)
처음에 느린시작으로 빠르게 전송률을 높이다가 느린시작 임계치에 도달하면 혼잡회피를 통해서 네트워크를 전송속도를 제어하다가 송신 세그먼트가 유실되거나 오류가 생기겨 세 번의 중복 ACK 세그먼트가 수신이 되면 느린시작부터 다시 혼잡제어를 하지 않고 빠른 회복을 통해서 혼잡을 제어한다. 그러다 타임아웃이 발생하면 느린시작부터 다시 시작한다.