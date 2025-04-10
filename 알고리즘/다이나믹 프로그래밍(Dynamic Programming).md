---
상태: 완료
tags:
  - 알고리즘
시작일: 2025-02-14
마감일: 2024-02-14
---
## 다이나믹 프로그래밍
>큰 문제를 작은 문제로 나누어 해결하고, 그 결과를 저장하여 동일한 작은 문제가 다시 등장했을 때 재계산하지 않고 저장된 값을 재사용하는 방식

### 아래의 조건을 충족해야 다이나믹 프로그래밍을 사용할 수 있다.
#### 🔥핵심 개념 : 부분 문제(Overlapping Subproblems) + 최적 부분 구조(Optimal Substructure)
- **부분 문제** : 큰 문제를 해결하려면 작은 문제들을 풀어야 하는데, **그 작은 문제들이 반복해서 등장하는 경우**
- **최적 부분 구조** : 작은 문제의 최적해를 이용해 큰 문제의 최적해를 구성할 수 있는 경우
	- 어떤 문제를 부분 문제로 나눌 수 있어야한다.

### DP 해결 방식
- **Top-Down** (메모이제이션, Memoization)
	- **재귀(Recursion) + 캐싱(변수에 저장)
	- 큰 문제를 작은 문제로 쪼개서 **재귀적으로** 해결하며, 이미 해결한 작은 문제의 결과를 저장하여 중복 계산을 방지
- **Bottom-up** (탭 메모이제이션, Tabulation)
	- **반복문 + 테이블(배열)**
	- 작은 문제부터 차례로 풀어나가며 **테이블(배열)에 결과를 저장**
	- 재귀 호출을 사용하지 않고 반복문을 활용하여 성능이 더욱 향상됨

### DP를 사용하는 이유 (vs. 단순 재귀)
#### ❌ 일반적인 재귀 방식의 문제점
단순 재귀는 같은 부분 문제가 여러 번 반복적으로 호출되어 비효율적입니다.
예제: 피보나치 수열 (일반 재귀)
```java
public class Fibonacci {
    public static int fib(int n) {
        if (n <= 1) return n;
        return fib(n - 1) + fib(n - 2);
    }

    public static void main(String[] args) {
        System.out.println(fib(10)); // 55
    }
}

```
✅**시간 복잡도: O(2^n)** → 너무 느림 (중복 계산 발생)

###  DP를 활용한 개선 방법
#### 1. Top-Down 방식 (메모이제이션)
```java
static class TopDown {  
	static Map<Integer, Integer> memo = new HashMap<>();  
  
	public static int fib(int n) {  
		if(n <= 1) return n;  
		if (memo.containsKey(n)) return memo.get(n);  
  
		int result = fib(n - 1) + fib(n - 2);  
		memo.put(n, result);  
  
		return result;  
	}  
}
```
✅**시간 복잡도: O(n)**
#### 2. Bottom-up 방식 (탭 메모이제이션)
```java
static class BottomUp {  
	public static int fib(int n) {  
		if (n <= 1) return n;  
  
		int[] dp = new int[n + 1];  
		dp[0] = 0;  
		dp[1] = 1;  
  
		for (int i = 2; i <= n; i++) {  
			dp[i] = dp[i - 1] + dp[i - 2];  
		}  
  
		return dp[n];  
	}  
}
```
✅**시간 복잡도: O(n)**

### DP 문제 해결 패턴
- DP인지 판단하는 법
	- 문제가 겹치는 부분 문제와 최적 부분 구조를 만족하는지 확인
- 작은 문제로 나누어 점화식 찾기
	- 예: 피보나치 수열 -> `F(n) = F(n-1) + F(n-2)`
- Top-Down(재귀 + 메모이제이션) 또는 Bottom-Up(반복문 + 테이블(배열)) 선택
	- 보통 Bottom-Up 방식이 성능적으로 더 우수함
	
#### 왜 Bottom-Up 방식이 더 성능적으로 우수한가?
1. 재귀 호출이 없어서 스택 오버플로우 위험 없음
2. 함수 호출 비용(컨텍스트 스위칭) 없음 -> 실행 속도 증가
3. 해시맵 조회 비용(O(1))(조회를 많이 하면 오버헤드 증가할 수 있다) 대신 배열 접근 (O(1))로 최적화