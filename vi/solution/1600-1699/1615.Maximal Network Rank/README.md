---
comments: true
difficulty: Medium
rating: 1521
source: Weekly Contest 210 Q2
tags:
    - Graph
---

<!-- problem:start -->

# [1615. Maximal Network Rank](https://leetcode.com/problems/maximal-network-rank)

[中文文档](/solution/1600-1699/1615.Maximal%20Network%20Rank/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cơ sở hạ tầng gồm <code>n</code> thành phố và một số <code>roads</code> kết nối chúng. Mỗi <code>roads[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị một con đường hai chiều giữa thành phố <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>.</p>

<p><strong>Hạng mạng</strong><em> </em>của <strong>hai thành phố khác nhau</strong> là tổng số con đường kết nối <strong>trực tiếp</strong> với <strong>một trong hai</strong> thành phố. Nếu một con đường kết nối trực tiếp với cả hai thành phố thì chỉ được tính <strong>một lần</strong>.</p>

<p><strong>Hạng mạng lớn nhất</strong> của cơ sở hạ tầng là <strong>hạng mạng lớn nhất</strong> trong mọi cặp thành phố khác nhau.</p>

<p>Cho số nguyên <code>n</code> và mảng <code>roads</code>, hãy trả về <em><strong>hạng mạng lớn nhất</strong> của toàn bộ cơ sở hạ tầng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1615.Maximal%20Network%20Rank/images/ex1.png" style="width: 292px; height: 172px;" /></strong></p>

<pre>
<strong>Input:</strong> n = 4, roads = [[0,1],[0,3],[1,2],[1,3]]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Hạng mạng của thành phố 0 và 1 là 4 vì có 4 con đường kết nối với 0 hoặc 1. Con đường giữa 0 và 1 chỉ được tính một lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1615.Maximal%20Network%20Rank/images/ex2.png" style="width: 292px; height: 172px;" /></strong></p>

<pre>
<strong>Input:</strong> n = 5, roads = [[0,1],[0,3],[1,2],[1,3],[2,3],[2,4]]
<strong>Output:</strong> 5
<strong>Giải thích:</strong> Có 5 con đường kết nối với thành phố 1 hoặc 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 8, roads = [[0,1],[1,2],[2,3],[2,4],[5,6],[5,7]]
<strong>Output:</strong> 5
<strong>Giải thích:</strong> Hạng mạng của 2 và 5 là 5. Lưu ý rằng không cần tất cả thành phố phải liên thông.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= roads.length &lt;= n * (n - 1) / 2</code></li>
	<li><code>roads[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub>&nbsp;&lt;= n-1</code></li>
	<li><code>a<sub>i</sub>&nbsp;!=&nbsp;b<sub>i</sub></code></li>
	<li>Each&nbsp;pair of cities has <strong>at most one</strong> road connecting them.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Hạng mạng là tổng bậc của hai thành phố, trừ một nếu chúng nối với nhau bằng một con đường. Số thành phố đủ nhỏ để thử mọi cặp không có thứ tự.
>
> Ta cần kiểm tra kề trong $O(1)$ và có sẵn bậc của mỗi thành phố.
>
> Tập kề $g$ cung cấp cả $\lvert g[a] \rvert$ và phép kiểm tra $a \in g[b]$. Hai vòng lặp sẽ ghi nhận giá trị lớn nhất.

<!-- thinking:end -->

Ta dùng mảng một chiều $\textit{cnt}$ để ghi bậc của mỗi thành phố và mảng hai chiều $\textit{g}$ để ghi việc mỗi cặp thành phố có nối đường hay không. Nếu có đường giữa $a$ và $b$ thì $\textit{g}[a][b] = \textit{g}[b][a] = 1$; ngược lại $\textit{g}[a][b] = \textit{g}[b][a] = 0$.

Tiếp theo, ta liệt kê từng cặp thành phố $(a, b)$ với $a \lt b$ và tính hạng mạng là $\textit{cnt}[a] + \textit{cnt}[b] - \textit{g}[a][b]$. Giá trị lớn nhất là đáp án.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số thành phố.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximalNetworkRank(self, n: int, roads: List[List[int]]) -> int:
        g = [[0] * n for _ in range(n)]
        cnt = [0] * n
        for a, b in roads:
            g[a][b] = g[b][a] = 1
            cnt[a] += 1
            cnt[b] += 1
        return max(cnt[a] + cnt[b] - g[a][b] for a in range(n) for b in range(a + 1, n))
```

#### Java

```java
class Solution {
    public int maximalNetworkRank(int n, int[][] roads) {
        int[][] g = new int[n][n];
        int[] cnt = new int[n];
        for (var r : roads) {
            int a = r[0], b = r[1];
            g[a][b] = 1;
            g[b][a] = 1;
            ++cnt[a];
            ++cnt[b];
        }
        int ans = 0;
        for (int a = 0; a < n; ++a) {
            for (int b = a + 1; b < n; ++b) {
                ans = Math.max(ans, cnt[a] + cnt[b] - g[a][b]);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximalNetworkRank(int n, vector<vector<int>>& roads) {
        int cnt[n];
        int g[n][n];
        memset(cnt, 0, sizeof(cnt));
        memset(g, 0, sizeof(g));
        for (auto& r : roads) {
            int a = r[0], b = r[1];
            g[a][b] = g[b][a] = 1;
            ++cnt[a];
            ++cnt[b];
        }
        int ans = 0;
        for (int a = 0; a < n; ++a) {
            for (int b = a + 1; b < n; ++b) {
                ans = max(ans, cnt[a] + cnt[b] - g[a][b]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximalNetworkRank(n int, roads [][]int) (ans int) {
	g := make([][]int, n)
	cnt := make([]int, n)
	for i := range g {
		g[i] = make([]int, n)
	}
	for _, r := range roads {
		a, b := r[0], r[1]
		g[a][b], g[b][a] = 1, 1
		cnt[a]++
		cnt[b]++
	}
	for a := 0; a < n; a++ {
		for b := a + 1; b < n; b++ {
			ans = max(ans, cnt[a]+cnt[b]-g[a][b])
		}
	}
	return
}
```

#### TypeScript

```ts
function maximalNetworkRank(n: number, roads: number[][]): number {
    const g: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    const cnt: number[] = Array(n).fill(0);
    for (const [a, b] of roads) {
        g[a][b] = 1;
        g[b][a] = 1;
        ++cnt[a];
        ++cnt[b];
    }
    let ans = 0;
    for (let a = 0; a < n; ++a) {
        for (let b = a + 1; b < n; ++b) {
            ans = Math.max(ans, cnt[a] + cnt[b] - g[a][b]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
