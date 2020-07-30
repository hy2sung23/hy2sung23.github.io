---
title: "C++ 2-2. Practice"
categories:
 - Data Structure & Algorithm
last_modified_at: 2020-07-26
toc: true
toc_sticky: true
---

### 1. 5397번 키로거

```c++
#include <bits/stdc++.h>

using namespace std;

int main() {
	ios::sync_with_stdio(0);


	int q;
	cin >> q;
	while (q--) {
		list<char> L;
		auto cursor = L.begin();
		string pw;
		cin >> pw;
		for (auto c : pw) {
			switch (c) {
			case '<':
				if (cursor != L.begin())
					cursor--;
				break;
			case '>':
				if (cursor != L.end())
					cursor++;
				break;
			case '-':
				if (cursor != L.begin()) {
					cursor--;
					cursor = L.erase(cursor);
				}
					break;
			default:
				L.insert(cursor, c);
				break;
			}
		}
		for (auto c : L) 
			cout << c;
		cout << '\n';
	}
}
```