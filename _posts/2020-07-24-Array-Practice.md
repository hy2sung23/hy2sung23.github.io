---
title: "C++ 1-1. Practice"
categories:
 - Data Structure & Algorithm
last_modified_at: 2020-07-24
toc: true
toc_sticky: true
---

### 1. 10807번 개수 새기

```c++
#include <iostream>
using namespace std;

int main(void)
{
	ios::sync_with_stdio(0);
	cin.tie(0); cout.tie(0);
	int N, num, target;
	int arr[201] = {};
	cin >> N;

	for (int i = 0; i < N; i++) {
		cin >> num;
		arr[num + 100]++;
	}
	cin >> target;
	cout << arr[target + 100];
}

```

### 2. 2577번 숫자의 개수
```c++
#include <iostream>
using namespace std;

int main(void)
{
	int arr[10] = {};
	ios::sync_with_stdio(0);
	cin.tie(0); cout.tie(0);
	int A, B, C;
	cin >> A >> B >> C;
	int sum = A * B * C;
	do
		arr[sum % 10] ++;
	while (sum /= 10);

	for (int i = 0; i < 10; i++)
		cout << arr[i] << '\n';

	return 0;
}
```

### 3. 11328번 Strfry

```c++
#include <bits/stdc++.h>
using namespace std;

int main(void) {
	ios_base::sync_with_stdio(0);
	cin.tie(0); cout.tie(0);
	int n;
	string origin, strfried;
	cin >> n;
	while (n--)
	{
		bool sign = true;
		int arr1[26] = {};
		int arr2[26] = {};
		cin >> origin >> strfried;
		for (int i = 0; i < origin.size(); i++)
		{
			arr1[origin[i]-'a']++;
			arr2[strfried[i]-'a']++;
		}
		for (int i = 0; i < 26; i++)
		{
			if ( arr1[i] != arr2[i]) {
				sign = false;
				break;
			}
		}
		if (sign)
			cout << "Possible" << "\n";
		else
			cout << "Impossible" << "\n";

	}


	return 0;
}

```

4. 13300번 방 배정
```c++
#include <bits/stdc++.h>
using namespace std;

int main(void) {
	int arr0[6] = {};
	int arr1[6] = {};
	int n, k, sex, year;
	int room = 0;
	cin >> n >> k;
	while (n--) {
		cin >> sex >> year;
		if (sex) {
			arr1[year - 1]++;
		}
		else {
			arr0[year - 1]++;
		}
		
	}
	for (int i = 0; i < 6; i++) {

		room += (arr0[i] % k != 0 ? (arr0[i]/ k)+1 : (arr0[i] / k));
		room += (arr1[i] % k != 0 ? (arr1[i] / k)+1 : (arr1[i] / k));


	}
	cout << room;
}
```

### 5. 1475번 방 번호
```c++
#include <bits/stdc++.h>

using namespace std;

int main(void) {
	ios::sync_with_stdio(0);
	int N;
	int ret = 0;
	int arr[10] = {};
	cin >> N;
	do {
		int num = N % 10; 

		if ((num == 6 || num == 9) && (arr[6] || arr[9])) {
			cout << "Case 1. \n";
			if (arr[6] >= arr[9])
				arr[6]--;
			else
				arr[9]--;
		}
		else if (arr[num]) {
			cout << "Case 2. \n";
			arr[num]--;
		}
		else {
			cout << "Csae 3. \n";
			ret++;
			for (int i = 0; i < 10; i++)
				arr[i] ++;
			arr[num]--;
		}
	} while (N /= 10);//N=50이면 N/10=5임
	cout << ret;
}
```