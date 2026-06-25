# Floyd算法-全源最短路

前置：邻接矩阵、动态规划、最短路基础

Floyd-Warshall 是 All-Pairs Shortest Path（全源最短路）算法，用来一次性求出任意两点之间的最短距离。

## 核心思想

- `dist[i][j]` 表示当前已知的 `i -> j` 最短距离。
- 最外层枚举中转点 `k`，尝试让路径从 `i -> j` 改成 `i -> k -> j`。
- 转移方程：

```cpp
dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
```

`k` 必须放在最外层。处理完第 `k` 轮后，`dist[i][j]` 表示只允许使用 `0..k` 这些点作为中转点时，`i -> j` 的最短距离。

## 适用场景

- 点数较少，并且需要查询很多组点对最短路。
- 通常 `n <= 500` 比较稳；`n = 1000` 已经是约 `10^9` 次转移，一般只有时限宽、常数极小或特殊优化时才考虑。
- 可以处理负边，但不能存在负环。
- 如果 Floyd 后存在 `dist[i][i] < 0`，说明图中有负环。

## 复杂度

- 时间复杂度：`O(n^3)`
- 空间复杂度：`O(n^2)`

为什么选 Floyd：如果只求单源最短路，Dijkstra / Bellman-Ford / SPFA 往往更合适；但如果要一次性拿到所有点对最短路，Floyd 写法短、常数小、实现稳定。

## 初始化

- `dist[i][i] = 0`
- 不连通的点对设为 `INF`
- 有重边时保留最小边权：`dist[u][v] = min(dist[u][v], w)`
- 无向图双向赋值，有向图只赋值单向

## 模板

```cpp
using ll = long long;
const ll INF = (1LL << 60);

void floyd(vector<vector<ll>>& dist) {
    int n = dist.size();
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            if (dist[i][k] == INF) continue;
            for (int j = 0; j < n; j++) {
                if (dist[k][j] == INF) continue;
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}
```

如果题目保证所有点互相可达，并且边权较小，也可以去掉 `INF` 判断；普通模板保留判断更安全，能避免 `INF + INF` 溢出。

## 例题：P2910 Clear And Present Danger S

[P2910 [USACO08OPEN] Clear And Present Danger S - 洛谷](https://www.luogu.com.cn/problem/P2910)

题意：给出农场之间的距离矩阵，以及一条计划访问序列。每次从当前农场到下一个农场可以走最短路，求整条访问序列的最短总路程。

思路：

1. 读入访问序列。
2. 读入邻接矩阵作为初始 `dist`。
3. Floyd 求出任意两点之间的最短路。
4. 按访问序列累加相邻两点的最短距离。

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll INF = (1LL << 60);

void floyd(vector<vector<ll>>& dist) {
    int n = dist.size();
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            if (dist[i][k] == INF) continue;
            for (int j = 0; j < n; j++) {
                if (dist[k][j] == INF) continue;
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
}

void solve() {
    int n, m;
    cin >> n >> m;

    vector<int> path(m);
    for (int i = 0; i < m; i++) {
        cin >> path[i];
        path[i]--;
    }

    vector<vector<ll>> dist(n, vector<ll>(n, INF));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> dist[i][j];
        }
    }

    floyd(dist);

    ll ans = 0;
    for (int i = 1; i < m; i++) {
        ans += dist[path[i - 1]][path[i]];
    }
    cout << ans << '\n';
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    solve();
    return 0;
}
```

## 易错点

- 三重循环顺序必须是 `k -> i -> j`。
- 使用 `int INF = 1e9` 时，必须判断连通性再相加，防止溢出或假更新。
- 题目输入点编号通常从 `1` 开始，数组下标如果从 `0` 开始要统一减一。
- Floyd 求的是所有点对最短路，不要把它和单源最短路混用。

## 练习

- [洛谷 P2910 Clear And Present Danger S](https://www.luogu.com.cn/problem/P2910)：Floyd 入门模板题。
- [洛谷 P1119 灾后重建](https://www.luogu.com.cn/problem/P1119)：按时间增量加入中转点，是 Floyd 思想的经典变形。
- [CSES Shortest Routes II](https://cses.fi/problemset/task/1672/)：多次点对最短路查询，适合练习标准 Floyd。

参考模板与详解：[OI-Wiki - 最短路](https://oi-wiki.org/graph/shortest-path/)
