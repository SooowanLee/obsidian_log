---
상태: 완료
tags:
  - 자료구조
시작일: 2025-01-09
마감일: 2025-01-09
---
## **Set**
- 중복 허용 X
- 순서 보장 X(순서를 보장하는 경우도 있음)

### Set은 ADT(Abstract Data Type)이다 
Set은 Interface이고 Set을 구현한 구현체인 HashSet, SortedSet등이 있다.

## **HashSet**
HashSet의 구현체는 HashMap이다.

HashMap은 key:value로 테이블 형태로 데이터를 저장
key는 중복X, value는 중복 O

HashSet에서 사용하는 HashMap은 Key만 사용하고 Value부분에는 그냥 0,1 같은 dummy 값을 넣는다.

## **HashMap**
key:value로 데이터를 저장

### 특징
**삽입, 삭제, 갱신, 탐색이 O(1)(상수)시간에 처리된다.** 
상수시간으로 데이터가 처리된다는 것은 아무리 데이터가 커지더라도 일정 시간안에 데이터를 처리 가능하다.
반대로, O(n)만큼의 시간이 걸리는 자료구조는 데이터의 사이즈가 커질수록 비례해서 시간이 늘어난다.

### 예) 인기투표: 한국의 영화배우
투표 참여자 : 전국민 5,500만명 / 한 명씩 자신이 좋아하는 영화배우에게 투표한다.
![](https://i.imgur.com/dI3HOEh.png)

List를 사용해서 미리 배우 이름을 넣어두기에는 배우가 너무 많다.
HashMap을 사용해서 배우 이름이 나올 때 마다 key에 저장을 하고 똑같은 배우가 나온다면 value++하면 효율적으로 투표가 가능하다.

### HashMap
- 배열의 특징인 상수시간으로 처리하는 기능을 사용해서 구현
- 하나의 key는 하나의 value에 맵핑
- value는 배열의 index에 저장
- HashFunction 함수를 사용해서 key를 배열의 index로 변환
- key는 중복X


#### 내부 구현
- 배열(Array)을 사용하여 구현
- 배열에 저장하기 위해 key의 hash를 구해야함
- hash function으로 key의 hash를 구함
- 구해진 hash를 배열 사이즈로 modular(%) 한 값을 index로 사용
#### HashFunction이 중요하다!
충돌 가능성에 영향을 준다.

#### 해시 충돌(Hash Collision)
서로 다른 key들이 같은 hash를 가질 때 발생
**key1 != key2 BUT!!! hf(key1) == hf(key2)**

HashMap에서 또 다른 의미의 해시 충돌
**hf(key1) != hf(key2) BUT!!! hf(key1) % M == hf(key2) % M**

**왜 발생하는가?**
- perfect hash function 구현의 어려움
- key의 사이즈에 비해 hash map의 사이즈가 작기 때문

**내 서비스에 가입한 사람의 핸드폰 번호를 key, 그 사람 정보를 value로 hash map에 저장한다면?**
전국민이 한개의 번호만 가지고 있다고 치고 배열의 크기를 5천만개로 잡았다.
실제 서비스이용자는 1만명이라하면 나머지 4999만개는 낭비된다.
그래서 hashmap의 사이즈를 잡을 때 서비스를 사용할 사람보다는 훨씬 적게 잡는다.
**그럼 이용할 사람이 5천만인데 메모리가 2000이라고 치면 사용자%메모리 크기를 하기 때문에 해시 충돌이 일어날 수밖에 없다.**

### HashMap의 해시 충돌 해결 방법
**최대한 데이터를 퍼트려서 저장할 수 있게 하는 것이 중요하다.**
- Separate chaining: **추가적인 공간**을 활용하여 해결하는 방식
	- Linked List 사용
- Open addressing: 충돌 발생 시 **인접한 비어있는 공간**(bucket)에 저장
	- Linear probing: **고정폭**으로 이동하여 빈 공간을 찾음
	- Quadratic probing: **제곱수**로 이동하여 빈 공간을 찾음
		- 데이터가 군집된 곳에 빠졌을 때 빠르게 탈출실 수 있다.
	- Double Hashing: **또 다른 hash function**을 사용하여 빈 공간을 찾음

### Open addressing 주의점
중간 연결고리 역할을 하는 key 의 삭제 시, 이미 있는 값도 없다고 판단할 수 있음

 해결챌
- 삭제 시 DELETE 같은 상징적인 형태로 표시
	- DELETE 표시를 보고 다음 위치도 확인해봐야함을 알 수 있음
	- 단점: DELETE를 만나면 무조건 다음 위치를 확인해야함
- 삭제 위치 다음의  open addressing된 key들은 한 단계씩 앞으로 옮겨준다.
	- 단점: 이동시키는 추가적인 비용 발생

## Java에서는 해시 충돌을 어떻게 해결했을까?
Jdk7까지는 LinkedList를 사용한 separate chaining을 활용
Jdk8에서 LinkedList와 Red Black Tree를 혼용한 separate chaining을 활용