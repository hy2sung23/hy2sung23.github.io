---
title: "C++ 6-. BFS Practice"
categories:
 - Data Structure & Algorithm
last_modified_at: 2020-08-01
toc: true
toc_sticky: true
---

##### 백준 1012번. 유기농 배추

```c++
#include <bits/stdc++.h>

using namespace std;
#define x first;
#define y second;

int n, m, num;

int dx[4] = { 0,1,0,-1 };
int dy[4] = { 1,0,-1,0 };

int main() {
	int test;
	cin >> test;
	while (test--) {
		int board[53][53] = { 0, };
		int vis[53][53] = { 0, };
		int count = 0;
		int a, b;
		cin >> n >> m >> num;
		queue<pair<int, int>> Q;
		for (int i = 0; i < num; i++) {
			cin >> a >> b;
			board[a][b] = 1;
		}
		for (int i = 0; i < n; i++)
			for (int j = 0; j < m; j++) 
				if (board[i][j] && !vis[i][j]) {
					
					count++;
					vis[i][j]=1;
					Q.push({ i,j });
					while (!Q.empty()) {
						auto cur = Q.front();
						Q.pop();
						for (int dir = 0; dir < 4; dir++) {
							int nx = dx[dir] + cur.x;
							int ny = dy[dir] + cur.y;
							if (nx < 0 || nx >= n || ny < 0 || ny >= m) continue;
							if (vis[nx][ny] || !board[nx][ny]) continue;
							vis[nx][ny] = 1;
							Q.push({ nx,ny });
						}
					}
					
				}

		cout << count << '\n';
	}
}
```

##### 백준 7568번. 토마토

잘 풀었는데, i,j,z가 계속 헷갈려서 틀렸었음.

```c++
#include <bits/stdc++.h>

using namespace std;


int dx[6] = { 1,-1,0,0,0,0 };
int dy[6] = { 0,0,1,-1,0,0 };
int dz[6] = { 0,0,0,0,1,-1 };

int board[103][103][103];
int M, N, H;


int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cin >> M >> N >> H;

	// 1: 익음 || 0: 안 익음 || -1 : 없음
	int count = 0, day = 0;
	queue <tuple<int, int, int>> Q;
	for (int z = 0; z < H; z++)
		for (int i = 0; i < N; i++)
			for (int j = 0; j < M; j++) {
				cin >>board[i][j][z];
				if (board[i][j][z] == 1)
					Q.push({ i,j,z });
			}

	while (!Q.empty()) {
		int x, y, z;
		tie(x,y,z)= Q.front();
		Q.pop();
		for (int dir = 0; dir < 6; dir++) {
			int nx = dx[dir] + x;
			int ny = dy[dir] + y;
			int nz = dz[dir] + z;
			if (nx < 0 || ny < 0 || nz < 0 || nx >= N || ny >= M || nz >= H) continue;
			if (board[nx][ny][nz]) continue;
			board[nx][ny][nz] = board[x][y][z] + 1;
			Q.push({ nx,ny,nz });
		}
	}
	for (int z = 0; z < H; z++)
		for (int i = 0; i < N; i++)
			for (int j = 0; j < M; j++) {
				if (board[i][j][z] == 0) {
					cout << -1;
					return 0;
				}
				day = max(day, board[i][j][z]);
			}

	cout << day-1;

}
```

##### 백준 9466번. 텀 프로젝트 (fail)

모르겠다;;

##### 백준 2583번. 영역 구하기 (ing)

```c++
#include<bits/stdc++.h>
using namespace std;

#define x first;
#define y second;

int board[100][100];
int vis[100][100];

int dx[4] = { 1,0,-1,0 };
int dy[4] = { 0,1,0,-1 };

int main() {
	queue<pair<int, int>> Q;
	vector<int> v;
	int M, N, K;
	cin >> M >> N >> K; //M:높이 N:밑변
	int St_x, St_y, Fin_x, Fin_y;
	while (K--) {
		// St_x,St_y, Fin_x,Fin_y
		// St_x - Fin_x, St_y - Fin_y for문으로 순회하면서 Q.push()
		cin >> St_x >> St_y >> Fin_x >> Fin_y;
		for (int i = 0; i < Fin_x - St_x; i++)
			for (int j = 0; j < Fin_y - St_y; j++) {
				vis[St_x + i][St_y + j] = 1;
			}
	}
		for (int i = 0; i < N; i++)
			for (int j = 0; j < M; j++) 
				if (!vis[i][j]) {
					int size = 0;
					Q.push({ i,j });
					while (!Q.empty()) {
						auto cur = Q.front();
						for (int dir = 0; dir < 4; dir++) {
							int nx = dx[dir] + cur.x;
							int ny = dy[dir] + cur.y;
							if (nx < 0 || ny < 0 || nx >= N || ny >= M) continue;
							if (vis[nx][ny] || board[nx][ny]) continue;
							vis[nx][ny] = 1;
							Q.push({ nx,ny });
							size++;

						}
					}
					v.push_back(size);
				}
		for (auto c : v) cout << c << '\n';
	

}
```