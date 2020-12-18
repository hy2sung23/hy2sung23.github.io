---
title: "C++ 8. Recursion"
author:
    박혜성
sidebar:
  nav: Algorithm
categories:
 - Data Structure & Algorithm
---

## Recursion 재귀

하나의 함수에서 자기 자신을 다시 호출하여 작업을
수행하는 알고리즘.

| 재귀를 이용하여 문제를 푼다 => 귀납적 방식으로 문제를 해결한다.


ex) 1번 도미노가 쓰러진다. -> k번 도미노가 쓰러지면 k+1번 도미노가 쓰러진다. ->
모든 도미노가 쓰러진다.

### 재귀함수의 조건

특정 입력에 대해서는 재귀하지 않고 종료해야 함.
이 경우를 base condition이라 하고, 모든 입력은 base condition으로 수렴해야 함

```c++
void func1(n){
    if (!n) return; //<---- base condition
    cout<<n<<' ';
    func1(n-1)
}
```

-함수의 인자로 어떤 것을 받고, 어디까지 계산한 뒤 자기 자신한테 넘길지 명확히 해야 함.
- 모든 재귀함수는 반복문만으로 동일한 동작을 하는 코드를 만들 수 있음.
- 재귀는 반복문에 비해 코드가 간결하지만, 메모리, 시간 비용이 많이 듦

## 연습문제 1629. 곱셈 (mod)

```c++

using ll = long long;



ll POW(ll a, ll b, ll c) {
	if (b == 1)
		return a % c; //base condition

	ll val = POW(a, b / 2, c);
	val = val * val % c;
	if (b % 2 == 0) return val;
	return val * a% c;
```

어렵다.. 지금 이해 안 되면 외우기라도...

## 백준 11729번. 하노이 탑

n번 원판이 3번째 장대로 가려면 1~n01 원판이 2번 장대에
있어야 함. n-1번이 2번째로 가려면 1~n-2번이 3번째에 있어야 함

=>원판이 1개일 때 옮길 수 있고, 원판이 n-1개일 때 옮길 수 있다면, n개일 때도 옮길 수 있다.

=> 재귀

### 1. 함수의 정의 void func(int a, int b, int n)

만약 그냥 void func(int n)이라고 한다면?
함수 내부에서 func(int n-1)을 호출할텐데 그럼 몇번 장대로 가는 지 알 수 없음

따라서 void func(int a, int b, int n) 원판 n개를 a에서 b로 이동

### 2. base condition

n == 1일 때 ```cout<<a<<' ' << b<<'\n';```

### 3. 재귀 식

1. n-1개의 원판을 기둥 a에서 6-a-b로 옮긴다 func(a,6-a-b,n-1)
2. n번 원판을 기둥 a에서 b로 옮긴다. ```cout << a<<' ' << b<<'\n';```
3. n-1개의 원판을 6-a-b에서 b로 옮긴다 func(6-a-b,b,n-1)

```c++
#include<bits/stdc++.h>
using namespace std;

void func(int a, int b, int n) { //원판 n개를 a번에서 b번으로 이동
	if (n == 1) {
		cout << a << ' ' << b << '\n';
		return;
	}
	func(a, 6 - a - b, n - 1);
	cout << a << ' ' << b << '\n';
	func(6 - a - b, b, n - 1);

}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	int n;
	cin >> n;
	cout << (1 << n) - 1 << '\n';
	func(1, 3, n);

}
```
허미 어려운거;;;;

## 백준 1074번. Z

### 1. 함수의 정의

``` int func(int n, int r, int c)```
2^n * 2^n 배열에서 (r,c)를 방문하는 순서를 반환하는 함수.

### 2. base condition

n==0일때 return 0;
### 3. 재귀 식

(r,c)가 1번/2번/3번/4번 사각형일때 리턴값 달라지면서

```c++
#include <bits/stdc++.h>

using namespace std;

int func(int n, int r, int c) {
	if (n == 0) return 0;

	// 1번,2번,3번,4번 사각형
	int half = 1<<(n-1); // 2^(n-1)
	if (r < half && c < half) return func(n - 1, r, c); //1번
	else if (r < half && c >= half) return half * half + func(n - 1, r, c - n); //2번
	else if (r >= half && c < half) return 2 * half * half + func(n - 1, r - n, c); //3번
	else return 3 * half * half +func(n - 1, r - n, c - n);

}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	int n, r, c;
	cin >> n >> r >> c;
	cout << func(n, r, c);
}
```
