---
title: "Leetcode 690. Employee Importance, 1306. Jump Game 3 (BFS) "
categories:
 - Data Structure & Algorithm
last_modified_at: 2020-08-04
toc: true
toc_sticky: true
---

자신감 회복 휴..
##### 690
```c++
#include <bits/stdc++.h>
using namespace std;


// Definition for Employee.
class Employee {
public:
	int id;
	int importance;
	vector<int> subordinates; //subordinates.id
};


class Solution {
	int sum = 0;
	stack<Employee*> s;
public:
	int getImportance(vector<Employee*> employees, int id) {
		func1(employees, id);
		while (!s.empty()) {
			auto cur = s.top();
			s.pop();
			while (cur->subordinates.size()) {
				func1(employees, cur->subordinates.back());
				cur->subordinates.pop_back();
			}
			sum += cur->importance;
				
		}
		return sum;
	}

	void func1(vector<Employee*> employees, int id) {
		/*
		func1에서는 한바퀴 돌면서 인자로 들어온 id를 스택에 집어 넣어
		스택에 있는 걸 하나씩 꺼내고 cur에 할당 cur의 부하가 있으면
		해당 부하의 id를 스택에 넣어 부하가 없으면 sum += value를 하고, pop()조져
		*/
		for (auto c : employees)
			if (c->id == id)
				s.push(c);
	}
};
```


##### 1306


```c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
	queue<int> Q;
	bool vis[50000] = { 0, };
public:
	bool canReach(vector<int>& arr, int start) {
		vis[start] = true;
		Q.push(start);
		while (!Q.empty()) {
			int cur = Q.front();
			Q.pop();
			if (arr[cur] == 0) return true;
			for (int nxt : {cur + arr[cur], cur - arr[cur]}) {
				if (nxt <0 || nxt>=arr.size())continue;
				if (!vis[nxt]) { //만약 방문을 했다면 큐에 넣지 않을거야
					Q.push(nxt);
					vis[nxt] = true;
				}
			}
		}
		return false; 
//큐가 텅 빔 -> 0값의 인덱스를 방문하지 않고 다른 인덱스만 순환하는 경우
	}
};
```

vis를 5만개나 만들 필요 없이 set을 이용하면 메모리를 좀 덜 쓸 수 있었음
또는
못움직이게 ```arr[start] =0``` 으로 바꿨다면? 괜찮네 이것도..