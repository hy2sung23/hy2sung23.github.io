---
title: "2. Linked List With C++"
categories:
 - Data Structure & Algorithm
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
#include <bits/stdc++.h>

using namespace std;

const int MX = 1000030;
string dat[MX];
int pre[MX], nxt[MX];
int unused = 1;

void insert(int addr, char str) {
}

void erase(int addr) {
}

void traverse() {
}

int main() {
	ios::sync_with_stdio(0);
	fill(pre, pre + MX, -1);
	fill(nxt, nxt + MX, -1);

	string init;
	cin >> init;
	int curser = 0;
	for (auto c : init) {
		insert(curser, c);
		curser++;
	}
	int num;
	cin >> num;
	while (num--) {
		char cmd, cha;
		cin >> cmd;
		switch (cmd){
		case 'L':
			if (curser != 0)
				curser=pre[curser];
			break;
		case 'D':
			if (nxt[curser] != -1) {
				curser = nxt[curser];
			}
			break;
		case 'B':
			if (curser != 0) {
				erase(curser);
				curser = pre[curser];
			}
				break;
		case 'P':
			cin >> cha;
			insert(curser, cha);
			curser = nxt[curser];
			break;
		}
	}
	traverse();
}
```

- STL List 이용 풀이

```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
	ios::sync_with_stdio(0);
	list<char> L;
	list<char>::iterator cursor = L.begin();
	string init;
	cin >> init;
	for (auto c : init) {
		L.push_back(c);
	}
	cursor = L.end();
	int q;
	cin >> q;
	while (q--) {
		char cmd;
		cin >> cmd;
		switch (cmd) {
		case 'L':
			if(cursor != L.begin())
				cursor--;
			break;
		case 'D':
			if(cursor != L.end())
				cursor++;
			break;
		case 'B':
			if (cursor != L.begin()) {
				cursor--;
				cursor = L.erase(cursor);
			}
				break;
		case 'P':
			char cha;
			cin >> cha;
			L.insert(cursor,cha);
			break;
		}
	}

	for (auto c : L) cout << c;
}
```

### 연습문제 (머리로 생각)

```script
Q1.
원형 연결 리스트 내의 임의의 노드 하나가 주어졌을 때, 
해당 List의 길이를 효율적으로 구하는 방법은? 

A.
자신 노드를 참조할 때 까지 다음 노드로 이동하면 됨. 이때 시간복잡도O(N)

Q2.
중간에 만나는 두 연결 리스트의 시작점들이 주어졌을 때
만나는 지점을 구하는 방법은?

A.
두 리스트를 끝까지 진행시켜 각각의 길이를 구함.
그후 시작점으로 돌아와 더 긴 쪽을 둘의 차이만큼 앞으로 이동시킨 후,
만나는 지점까지 노드를 한 칸씩 이동. 이 때 시간복잡도 O(A+B)

Q3.
주어진 연결 리스트 안에 사이클이 있는지 판단하는 방법은?

A. Floyd's cycle-finding algorithm
```
오.. 저번에 했던 기억이 난다
<https://hy2sung23.github.io/data%20structure%20&%20algorithm/CT-11/>
