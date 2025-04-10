---
상태: 완료
시작일: 2024-06-03
마감일: 2024-06-03
---
자바와 DB의 데이터 타입이 다른 것들이 있기 때문에 이 데이터를 어노테이션을 사용해서 맞춰줘야합니다.

## 매핑 어노테이션
| 어노테이션       | 설명             |
| ----------- | -------------- |
| @Column     | 컬럼 매핑          |
| @Temporal   | 날짜 타입 매핑       |
| @Enumerated | enum 타입 매핑     |
| @Lob        | BLOB, CLOB 매핑  |
| @Transient  | 특정 필드를 컬럼에서 제외 |

### @Column
객체 필드와 테이블 컬럼 매핑에 사용

| 속성                       | 설명                                                                     | 기본값                  |
| ------------------------ | ---------------------------------------------------------------------- | -------------------- |
| name                     | 필드와 매핑할 테이블의 컬럼 이름                                                     | 객체 필드명               |
| insertable<br>updatable  | 등록, 변경 가능 여부를 설정                                                       | TRUE                 |
| nullable(DDL)            | null값의 허용 여부를 설정, false 설정시 not null                                   |                      |
| unique(DDL)              | @Table의 uniqueConstraints과 같지만 한 컬럼에 간단히 유니크 제약조건을 걸 때 사용한다.           |                      |
| columnDefinition(DDL)    | 데이터베이스 컬럼 정보를 직접 줄 수 있다.<br>예) varchar(100) default 'EMPTY'            | 필드의 자바 타입과 방언 정보를 사용 |
| length(DDL)              | 문자 길이 제약조건, String 타입에만 사용한다.                                          | 255                  |
| precision,<br>scale(DDL) | percision은 소수점을 포함한 전체 자릿수를, scale은 소수의 자릿수<br>정밀한 소수를 다루어야 할 때만 사용한다. | precision=19         |

### @Enumerated
자바 enum 타입을 매핑할 때 사용
무조건 EnumType.STRING으로 사용해야한다.

**주의사항**
>EnumType.ORDINAL 사용 X(순서를 저장) 
>기본이 ORDINAL로 되어있는데 이는 숫자로 나오기 때문에 나중에 데이터가 꼬이는 현상이 발생할 수 있다.
