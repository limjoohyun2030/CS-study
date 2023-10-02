# 시간 복잡도(Time Complexity)란?

알고리즘의 효율성 분석을 위해 알고리즘의 수행 시간(CPU Time)을 따지는 것

함수의 실행 시간을 표현하는 것

주로 점근적 분석을 통해 실행 시간을 단순하게 표현하며 이 때 점근적 표기법으로 표현함

# 시간 복잡도 분석

### 구현한 알고리즘을 컴퓨터에서 실행시켜 실제 수행 시간 측정

- **사용X** - 일반성 결여 : 컴퓨터 속도, 프로그래밍 언어, 프로그램 작성 방법, 컴파일러의 효율성 등에 종속적

### 알고리즘 단위 연산의 수행 횟수의 합

시간 복잡도에 영향을 미치는 요인

- 입력 크기 N
    - 입력으로 제공되는 데이터의 크기, 문제가 해결하려는 대상이 되는 개체의 개수
    - 예: 행렬의 크기, 리스트 원소의 수, 그래프의 정점의 수 등
- 입력 데이터의 상태

### 입력 크기 n이 증가하면 수행 시간도 증가

- 단순히 수행되는 단위 연산의 개수의 합으로 표현하는 것은 부적절
    - 입력 크기 n에 대한 함수 f(n)으로 표현
- 입력 데이터의 상태에 종속적
    - upper , lower bound가 같을 때(tight bound) : θ(n)
    - 최선 수행 시간(lower bound) : Ω(n)
    - 최악 수행 시간(upper bound) : O(n)

# 시간 복잡도 표기법

주로 빅 오 표기법(Big O Notation)이 사용됨

최악의 경우에 얼마나 많은 연산(기본 연산 단위)이 필요한지 나타내는 표기법

# 대표적인 시간 복잡도 속도 비교

- **O(1) < O(log n) < O(n) < O(n log n) < O(n^2) < O(2^n) < O(N!)**

![Untitled](https://github.com/limjoohyun2030/CS-study/blob/main/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/%E1%84%89%E1%85%B5%E1%84%80%E1%85%A1%E1%86%AB%20%E1%84%87%E1%85%A9%E1%86%A8%E1%84%8C%E1%85%A1%E1%86%B8%E1%84%83%E1%85%A9(Time%20Complexity)/Untitled.png?raw=true)

# 시간 복잡도 구하기

```java
int[] multiply(int[] inputs, int multiplier) {
	int[] nums= new int[inputs.length]; // c1 = 1
	for (int i = 0; i < inputs.length; i++) { // c2 = N + 1
		nums[i] = intpus[i] * multiplier; // c3 = N
	}
	return nums; // c4 = 1
}

N = size of inputs
T(N) = c1 + c2*(N + 1) + c3*N + 1
		 = (c2 + c3)*N + c1 + c2 + 1
		 = a*N +b // 식을 간단히 하기 위해서 치환해줌

여기서 더 정확한 계산은 어렵다
N이 작을땐 실행 시간이 의미 없다
N -> ∞ 일 때 실행 시간이 궁금하다!

N이 커질수록 덜 중요한 것은 제거
최고차항만 의미있다 -> a*N
최고차항의 계수는 의미없다 -> N

최종적으로 θ(N)으로 표현할 수 있다. <- 점근적 표기법
(input 사이즈에 비례하기 때문에 
아무리 오래 걸려도 O(N)이고
아무리 빨라도 O(N) 이라 
N에 비례하는 형태로 시간이 걸린다고 표현 할 수 있기 때문에
tight bound로 표현할 수 있다 θ(N))

점근적 분석(Asymptotic analysis)
임의의 함수가 N -> ∞ 일 때
어떤 함수 형태에 근접해지는지 분석
```

```java
boolean exists(int[] inputs, int target) {
	for (int i = 0; i < inputs.length; i++) {
		if (inputs[i] == target) {
			return true;
		}
	}
	return false;
}

N = size of inputs
위 함수는 파라미터 데이터에 따라 실행 시간이 조금씩 다르다

{52, 7, 17, 34, ... , 86}
예를 들어 위 배열에서 찾으려는 숫자가 앞쪽에 존재한다면 함수의 실행이 빠르지만
반대로 뒤쪽에 존재한다면 오래 걸릴 것
```

| case | when | times |
| --- | --- | --- |
| best | 0 번째에 존재 | 1 |
| worst | 마지막에 존재 or 존재하지 않음 | N |

|  | Best | Worst |  | Avg(정의하기 애매함) |
| --- | --- | --- | --- | --- |
| Ω | Ω(1) | Ω(N) |  |  |
| O | O(1) | O(N) |  | 리스트 중간에서 찾았다고 가정하면 N / 2 이고 계수를 제거하면 N이니 그냥 O(N)으로 Avg를 표기한다. |
| θ | θ(1) | θ(N) | lower bound와 upper bound가 같을 때 θ notation은 tight bound에 사용 된다. |  |
- lower bound : 함수 실행 시간은 아무리 빨라도 상수 시간 이상이다.
    - 최선 수행 시간 Ω(1)
- upper bound: 함수 실행 시간은 아무리 오래 걸려도 N에 비례하는 정도 이하이다.
    - 최악 수행 시간 O(n)
- θ(N) 일 경우 lower bound와 upper bound 모두 N에 비례한 형태를 띈다고 해석하면 된다.

---

- 참조
    - 강태원, 정광식. 자료구조. 2개정판 1쇄. 서울 : 방송통신대학교 출판문화원.
    - 이관용, 김진욱. 알고리즘. 초판 6쇄. 서울 : 방송통신대학교 출판문화원.
    - 쉬운코드. 시간복잡도([https://youtu.be/tTFoClBZutw?si=qY7T7WxP-8-wU3Wy](https://youtu.be/tTFoClBZutw?si=qY7T7WxP-8-wU3Wy))
