---
title: "2. Linked List With C++"
categories:
 - Algorithm & Data Structure
last_modified_at: 2020-07-25
toc: true
toc_sticky: true
---

### Linked List

- Node :  data와 다음 Node의 주소를 갖고 있음
- 배열보다 메모리 2배 더 사용
- 원소 조회 O(N), 원소 추가/삭제 O(1)
- 임의의 위치에 원소를 추가하거나 제거하는 연산이 많을 때 사용


### Linked List Without Structure

```c++
#include <bits/stdc++.h>
using namespace std;

const int MX = 1000005;
int dat[MX], pre[MX], nxt[MX];
int unused = 1;

void insert(int addr, int num) {
	dat[unused] = num;
	nxt[unused] = nxt[addr];
	if (nxt[addr] != -1) pre[nxt[addr]] = unused;
	nxt[addr] = unused;
	pre[unused] = addr;
	unused++;
}

void erase(int addr) {
	if (nxt[addr] != -1) pre[nxt[addr]] = pre[addr];
	 nxt[pre[addr]] = nxt[addr];


}

void traverse() {
	int cur = nxt[0];
	while (cur != -1) {
		cout << dat[cur] << ' ';
		cur = nxt[cur];
	}
	cout << "\n\n";
}

void insert_test() {
	cout << "****** insert_test *****\n";
	insert(0, 10); // 10(address=1)
	traverse();
	insert(0, 30); // 30(address=2) 10
	traverse();
	insert(2, 40); // 30 40(address=3) 10
	traverse();
	insert(1, 20); // 30 40 10 20(address=4)
	traverse();
	insert(4, 70); // 30 40 10 20 70(address=5)
	traverse();
}

void erase_test() {
	cout << "****** erase_test *****\n";
	erase(1); // 30 40 20 70
	traverse();
	erase(2); // 40 20 70
	traverse();
	erase(4); // 40 70
	traverse();
	erase(5); // 40
	traverse();
}

int main(void) {
	fill(pre, pre + MX, -1);
	fill(nxt, nxt + MX, -1);
	insert_test();
	erase_test();
	cout << dat[nxt[unused]];
}
```

### STL list

```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
	list<int> L = { 1,2 };  // 1 2
	list<int>::iterator t = L.begin(); // t는 1을 가리키는 중
	L.push_front(10); // 10 1 2
	cout << *t << '\n'; // t가 가리키는 값 = 1 출력
	L.push_back(5); // 10 1 2 5
	L.insert(t, 6); // t가 가리키는 값 앞에 6 삽입 10 6 1 2 5

	t++; // t를 1칸 앞으로 전진, 현재 t가 가리키는 값은 2
	t = L.erase(t); // t가 가리키는 값 제거, 그 다음 원소인 5의 위치 반환

	cout << *t << '\n';
	for (auto i : L) cout << i << ' ';
	cout << '\n';

}
```


### 연습문제 1406번 에디터

- Linked List 이용 풀이

```c++

```

- STL List 이용 풀이

```c++

```