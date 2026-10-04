---
comments: true
difficulty: Medium
rating: 1430
source: Weekly Contest 370 Q2
tags:
    - Graph
---

<!-- problem:start -->

# [2924. Find Champion II](https://leetcode.com/problems/find-champion-ii)

[中文文档](/solution/2900-2999/2924.Find%20Champion%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> đội được đánh số từ <code>0</code> đến <code>n - 1</code> trong một giải đấu; mỗi đội cũng là một node trong một <strong>DAG</strong>.</p>

<p>Bạn được cho số nguyên <code>n</code> và mảng số nguyên 2 chiều <code>edges</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code><font face="monospace">m</font></code>, biểu diễn <strong>DAG</strong>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh có hướng từ đội <code>u<sub>i</sub></code> đến đội <code>v<sub>i</sub></code> trong đồ thị.</p>

<p>Một cạnh có hướng từ <code>a</code> đến <code>b</code> trong đồ thị có nghĩa là đội <code>a</code> <strong>mạnh hơn</strong> đội <code>b</code>, và đội <code>b</code> <strong>yếu hơn</strong> đội <code>a</code>.</p>

<p>Đội <code>a</code> sẽ là <strong>nhà vô địch</strong> của giải đấu nếu không có đội <code>b</code> nào <strong>mạnh hơn</strong> đội <code>a</code>.</p>

<p>Trả về <em>đội sẽ là <strong>nhà vô địch</strong> của giải đấu nếu có một <strong>nhà vô địch duy nhất</strong>, nếu không thì trả về </em><code>-1</code><em>.</em></p>

<p><strong>Ghi chú</strong></p>

<ul>
	<li><strong>Chu trình</strong> là một dãy node <code>a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub>, a<sub>n+1</sub></code> sao cho node <code>a<sub>1</sub></code> cũng là node <code>a<sub>n+1</sub></code>, các node <code>a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub></code> đôi một khác nhau, và có một cạnh có hướng từ node <code>a<sub>i</sub></code> đến node <code>a<sub>i+1</sub></code> với mọi <code>i</code> trong khoảng <code>[1, n]</code>.</li>
	<li><strong>DAG</strong> là một đồ thị có hướng không có <strong>chu trình</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img height="300" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2924.Find%20Champion%20II/images/graph-3.png" width="300" /></p>

<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[0,1],[1,2]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Đội 1 yếu hơn đội 0. Đội 2 yếu hơn đội 1. Vì vậy, nhà vô địch là đội 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img height="300" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2924.Find%20Champion%20II/images/graph-4.png" width="300" /></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[0,2],[1,3],[1,2]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Đội 2 yếu hơn đội 0 và đội 1. Đội 3 yếu hơn đội 1. Tuy nhiên, cả đội 1 và đội 0 đều không yếu hơn đội nào khác. Vì vậy, đáp án là -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>m == edges.length</code></li>
	<li><code>0 &lt;= m &lt;= n * (n - 1) / 2</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= edge[i][j] &lt;= n - 1</code></li>
	<li><code>edges[i][0] != edges[i][1]</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho nếu đội <code>a</code> mạnh hơn đội <code>b</code>, thì đội <code>b</code> không mạnh hơn đội <code>a</code>.</li>
	<li>Dữ liệu đầu vào được tạo sao cho nếu đội <code>a</code> mạnh hơn đội <code>b</code> và đội <code>b</code> mạnh hơn đội <code>c</code>, thì đội <code>a</code> mạnh hơn đội <code>c</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm bậc vào

<!-- thinking:start -->

> **Tư duy**
>
> Các cạnh hiện tạo thành một DAG từ đội mạnh hơn đến đội yếu hơn. Nhà vô địch là đỉnh duy nhất có bậc vào bằng $0$; nếu không thì không tồn tại nhà vô địch duy nhất. Vì $n \le 100$, ta chỉ cần duyệt qua các cạnh một lần để xây dựng mảng bậc vào.
>
> Trả về $-1$ nếu không có đúng một đỉnh có bậc vào bằng 0, nếu không thì trả về chỉ số của đỉnh đó.

<!-- thinking:end -->

Theo mô tả bài toán, ta chỉ cần đếm bậc vào của mỗi node và lưu chúng trong một mảng $indeg$. Nếu chỉ có một node có bậc vào bằng $0$, thì node này là nhà vô địch; nếu không, không có nhà vô địch duy nhất.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findChampion(self, n: int, edges: List[List[int]]) -> int:
        indeg = [0] * n
        for _, v in edges:
            indeg[v] += 1
        return -1 if indeg.count(0) != 1 else indeg.index(0)
```

#### Java

```java
class Solution {
    public int findChampion(int n, int[][] edges) {
        int[] indeg = new int[n];
        for (var e : edges) {
            ++indeg[e[1]];
        }
        int ans = -1, cnt = 0;
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                ++cnt;
                ans = i;
            }
        }
        return cnt == 1 ? ans : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findChampion(int n, vector<vector<int>>& edges) {
        int indeg[n];
        memset(indeg, 0, sizeof(indeg));
        for (auto& e : edges) {
            ++indeg[e[1]];
        }
        int ans = -1, cnt = 0;
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                ++cnt;
                ans = i;
            }
        }
        return cnt == 1 ? ans : -1;
    }
};
```

#### Go

```go
func findChampion(n int, edges [][]int) int {
	indeg := make([]int, n)
	for _, e := range edges {
		indeg[e[1]]++
	}
	ans, cnt := -1, 0
	for i, x := range indeg {
		if x == 0 {
			cnt++
			ans = i
		}
	}
	if cnt == 1 {
		return ans
	}
	return -1
}
```

#### TypeScript

```ts
function findChampion(n: number, edges: number[][]): number {
    const indeg: number[] = Array(n).fill(0);
    for (const [_, v] of edges) {
        ++indeg[v];
    }
    let [ans, cnt] = [-1, 0];
    for (let i = 0; i < n; ++i) {
        if (indeg[i] === 0) {
            ++cnt;
            ans = i;
        }
    }
    return cnt === 1 ? ans : -1;
}
```

#### JavaScript

```js
function findChampion(n, edges) {
    const indeg = Array(n).fill(0);
    for (const [_, v] of edges) {
        ++indeg[v];
    }
    let [ans, cnt] = [-1, 0];
    for (let i = 0; i < n; ++i) {
        if (indeg[i] === 0) {
            ++cnt;
            ans = i;
        }
    }
    return cnt === 1 ? ans : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 lưu bậc vào của mọi node. Tương đương, nhà vô địch là đỉnh duy nhất không xuất hiện ở vị trí đích của bất kỳ cạnh nào. Đưa $0 \ldots n-1$ vào một set, xóa mỗi đỉnh đích, và nếu còn lại một đỉnh thì trả về đỉnh đó. Ý nghĩa vẫn tương ứng với bậc vào; set chỉ ghi nhận các đội chưa thua.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function findChampion(n: number, edges: number[][]): number {
    const vertexes = new Set<number>(Array.from({ length: n }, (_, i) => i));

    for (const [_, v] of edges) {
        vertexes.delete(v);
    }

    return vertexes.size === 1 ? vertexes[Symbol.iterator]().next().value! : -1;
}
```

#### JavaScript

```js
function findChampion(n, edges) {
    const vertexes = new Set(Array.from({ length: n }, (_, i) => i));
    for (const [_, v] of edges) {
        vertexes.delete(v);
    }
    return vertexes.size === 1 ? vertexes[Symbol.iterator]().next().value : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
