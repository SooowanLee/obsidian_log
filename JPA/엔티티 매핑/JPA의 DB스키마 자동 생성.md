---
상태: 완료
시작일: 2024-06-01
마감일: 2024-06-01
---
JPA가 매핑 정보를 보고 애플리케이션 로딩 시점에 자동으로 DB스키마를 생성하고 테이블에 데이터를 알아서 넣는다.

## 데이터베이스 스키마 자동 생성
| 속성            | 설명                                   |
| ------------- | ------------------------------------ |
| create        | 기존의 테이블이 존재하면 삭제하고 생성                |
| create-drop   | create와 같지만 마지막에 table을 삭제           |
| update        | 엔티티와 매핑 테이블이 없다면 생성, 테이블이 있다면 컬럼을 변경 |
| validate      | 엔티티 클래스와 테이블이 정상 매핑되는지만 검사           |
| none(default) | 아무 설정도 안함                            |

### 사용법
**persistence.xml**
```java
<persistence-unit name="hello">
	<properties>  
		<!-- 필수 속성 -->  
			...
		<!-- 옵션 -->  
			...
		<property name="hibernate.hbm2ddl.auto" value="create"/>  
	</properties>  
</persistence-unit>
```

**application.properties**
```application.properties
spring.jpa.hibernate.ddl-auto=옵션
```

**application.yml**
yml 설정에서는 계층 표현이 중요합니다. 띄어쓰기를 잘못하면 설정이 제대로 적용되지 않을 수 있습니다. 한 단계 내려갈때 띄어쓰기 2칸씩 합니다.
```yaml
spring:
  jpa: 
    hibernate: 
      ddl-auto: 옵션
```