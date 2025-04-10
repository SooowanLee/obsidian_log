---
상태: 완료
시작일: 2024-06-03
마감일: 2024-06-03
---
## 기본 키 매핑 어노테이션
- @Id
- @GeneratedValue
![](https://i.imgur.com/z93VOv1.png)


### IDENTITY 전략 - 특징
- 기본 키 생성을 **데이터베이스에 위임**
- 주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용
- **JPA는 보통 트랜잭션 커밋 시점에 INSERT SQL 실행** 
- **AUTO_INCREMENT는 데이터베이스에 INSERT SQL을 실행한 이후 ID값을 알 수 있음**
- **IDENTITY 전략은 em.persist() 시점에 즉시 INSERT SQL 실행하고 DB에서 식별자를 조회**

### SEQUENCE 전략 - 특징
- 데이터베이스 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트(예: 오라클 시퀀스)
- 오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용

####  SEQUENCE - @SequenceGenerator
- 주의 : allocationSize 기본값 = 50

| 속성              | 설명                                                                                           | 기본값                |
| --------------- | -------------------------------------------------------------------------------------------- | ------------------ |
| name            | 식별자 생성기 이름                                                                                   | 필수                 |
| sequenceName    | 데이터베이스에 등록되어 있는 시퀀스 이름                                                                       | hibernate_sequence |
| initialValue    | DDL 생성 시에만 사용됨, 시퀀스 DDL을 생성할 때 처음 1 시작하는 수를 지정한다.                                            | 1                  |
| allocationSize  | 시퀀스 한 번 호출에 증가하는 수(성능 최적화에 사용됨)<br>**데이터베이스 시퀀스 값이 하나씩 증가하도록 설정되어 있으면 이 값을 반드시 1로 설정해야 한다.** | **50**             |
| catalog, schema | 데이터베이스 catalog, schema 이름                                                                    |                    |

### TABLE 전략
- 키 생성 전용 테이블을 만들어서 데이터베이스 시퀀스를 흉내낸다.
- 장점: 모든 데이터베이스에 적용 가능
- 단점: 성능

### 권장하는 전략
- 기본 키 제약 조건: not null, 유일, **변하면 안된다**.
- 대체키를 사용하자.
- **권장: Long형 + 대체키 + 키 생성전략 사용**