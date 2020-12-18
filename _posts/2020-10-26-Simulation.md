---
title: "C++ 8. BackTracking"
author:
    박혜성
sidebar:
  nav: Algorithm
categories:
 - Data Structure & Algorithm
---

### 백준 15683 감시 (실패)
```c++
#include <bits/stdc++.h>
using namespace std;
#define row first;
#define col second;
int N, M; // N : 세로, M : 가로

string arr[10][10];

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cin >> N >> M;
	vector<pair<int, int>> c1, c2, c3, c4, c5;
	for (int i = 0; i < N; i++)
		for (int j = 0; j < M; j++) {
			cin >> arr[i][j];
			if (arr[i][j] == "1")
				c1.push_back({ i,j });
			if (arr[i][j] == "2")
				c2.push_back({ i,j });
			if (arr[i][j] == "3")
				c3.push_back({ i,j });
			if (arr[i][j] == "4")
				c4.push_back({ i,j });
			if (arr[i][j] == "5")
				c5.push_back({ i,j });
		}
	do

	/* 1. c1~c5 의 값이 모두 없어질 때까지 while 로 묶기
	 2. recursion ? -> basecondition은 어떻게 정하지? 재귀 안됨
	 3. 백트래킹,, 이것도 재귀잖아 그치만 가능한 모든 경우의 수 탐색이 가능한데?
	 근데 우린 가능한 모든 경우의 수가 아니라 그냥 모든 경우의 수를 탐색해야 해.
	 해당 cctv가 존재하면
	*/
}
```

### 백준 18808 스티커 붙이기
