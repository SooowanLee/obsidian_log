---
상태: 진행중
tags:
  - 자바
시작일: 2025-02-28
마감일:
---
## 객체생성과정
### 객체 생성
상태정보, 행위정보 등을 가지고 있다.

상태정보 = 변수
행위정보 = 메소드

Java에서는 class를 사용해서 객체를 구조화 한다. 객체는 생성하는 방법 new 연산자를 사용한다.
`User user = new User();` <- 이렇게 생체를 생성하면 this라는 자기 자신을 가리키는 객체도 함께 만들어진다.

`User user = new User()`에서 user라는 변수에는 `User`객체의 메모리 주소값이 들어가 있다.

### 객체 접근
아래와 같은 `User` 클래스가 있다고 할 때
```java
public class User {
	private String name;
	private int age;

	public String getName(){
		return this.name;
	}

	public void setName(String name){
		 this.name = name;
	}
}
```

그럼 생성된 `User` 객체에 접근하기 위해서는 어떻게 해야 할까? (**10초 정도 생각해보길 바랍니다.**)
정답은 `.(dot)` 연산자를 사용하는 것이다. 
 
`user.getName()` `.`연산자를 사용해서 객체의 메소드 및 변수에 접근이 가능하다. 물론 `user.name`와 같이 변수에 직접 접근도 가능하지만 **정보은닉,** **캡슐화라는 이유로 자바에서는 `getXxx()`, `setXxx()`과 같이  프로퍼티를 통해 접근하는 것을 권장**합니다.

### 🔥**참고 자료**
**SOLID에 대해 더 알고 싶다면?**  -> https://developer-soo.tistory.com/28

### 접근 제어자
> **클래스와 멤버(변수, 메서드)에 사용되며 외부에서 해당 클래스나 메서드에 대해 접근 권한을 설정할 수 있는 제어자를 의미**

종류 : public, private, protected, default
