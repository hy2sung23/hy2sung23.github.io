---
title: "C++ 8. BackTracking"
author:
    박혜성
sidebar:
  nav: Algorithm
categories:
 - Data Structure & Algorithm
---


### 백준 9663

```c++
#include <bits/stdc++.h>

using namespace std;
int N;
int tot = 0;

bool arr[20][20]; // 퀸이 위치한 자리
bool isused1[20], isused2[40], isused3[40];
// 1: 세로, 2: 좌측 하단에서 우측 상단 대각선
// 3: 우측 하단에서 좌측 상단 대각선

void func(int cur) {
	if (cur == N ) { // k 가 N-1 행까지 통과했을 때.
		cout << "pass" << '\n';
		tot++;
		return;
	}
	for (int i = 0; i < N; i++) {
		if (isused1[i] || isused2[i + cur] || isused3[cur - i + N - 1]) // column이나 대각선 중에 문제가 있다면
			continue;
		cout << cur << ',' << i << '\n';
		// [k][i] 좌표의 세로,대각선의 isused = 1;
		isused1[i] = 1;
		isused2[cur + i] = 1;
		isused3[cur - i + N - 1] = 1;
		func(cur + 1);
		// impos = 0 으로 원상복귀
		isused1[i] = 0;
		isused2[cur + i] = 0;
		isused3[cur - i + N - 1] = 0;

	}
}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cin >> N;
	func(0);
	cout << tot;
}
```

### 백준 1182

아 잘 모르겠네 ;;
```c++
#include<bits/stdc++.h>

using namespace std;

int cnt;
int arr[20];
bool isused[20];
int sum;

int N, S;
void func(int k) {

	if (k == N) return;
	if (sum + arr[k] == S) cnt++;
	func(k + 1);
	sum += arr[k];
	func(k + 1);
	sum -= arr[k];
}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cin >> N >> S;
	for (int i = 0; i < N; i++)
		cin >> arr[i];
	func(0);
	cout << cnt;
}
```

### leetcode 46. permutation

1. STL next_permutation 사용
```c++
class Solution {
    vector<vector<int>> v1;
public:
    vector<vector<int>> permute(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        do {
            vector<int> v2;
		    for (int i = 0; i < nums.size(); i++){
		        v2.push_back(nums[i]);
            }
		    v1.push_back(v2);
        } while (next_permutation(nums.begin(), nums.end()));
        return v1;
    }
};
```
2. BackTracking 사용

최선이었다...

```c++
#include <bits/stdc++.h>

using namespace std;

vector<vector<int>> permute(vector<int>& nums) {
	vector<vector<int>> perms;
	vector<int> newlist;
	vector<bool> used(nums.size(), false);
	func(nums, newlist, perms, used);
	return perms;
}

void func(vector<int> nums, vector<int> newlist, vector<vector<int>>& perms, vector<bool> used) {
	if (newlist.size() == nums.size()) {
		perms.push_back(newlist);
	}
	else {
		for (int i = 0; i < used.size(); i++){
			if (used[i]) continue;
			newlist.push_back(nums[i]);
			used[i] = true;
			func(nums, newlist, perms, used);
			used[i] = false;
			newlist.pop_back();
		}
	}
}

int main() {

}
```
