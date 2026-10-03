---
comments: true
difficulty: Hard
rating: 2413
source: Biweekly Contest 56 Q4
tags:
    - Graph
    - Array
    - Dynamic Programming
    - Dijkstra
---

<!-- problem:start -->

# [1928. Minimum Cost to Reach Destination in Time](https://leetcode.com/problems/minimum-cost-to-reach-destination-in-time)

[中文文档](/solution/1900-1999/1928.Minimum%20Cost%20to%20Reach%20Destination%20in%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Có một quốc gia gồm <code>n</code> thành phố được đánh số từ <code>0</code> đến <code>n - 1</code>, trong đó <strong>tất cả các thành phố đều được kết nối</strong> bằng các con đường hai chiều. Các con đường được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code>, trong đó <code>edges[i] = [x<sub>i</sub>, y<sub>i</sub>, time<sub>i</sub>]</code> biểu thị một con đường giữa các thành phố <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code>, mất <code>time<sub>i</sub></code> phút để đi qua. Có thể có nhiều con đường với thời gian di chuyển khác nhau nối cùng một cặp thành phố, nhưng không có con đường nào nối một thành phố với chính nó.</p>

<p>Mỗi lần đi qua một thành phố, bạn phải trả một khoản phí. Khoản phí này được biểu diễn bằng một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>passingFees</code> có độ dài <code>n</code>, trong đó <code>passingFees[j]</code> là số đô la bạn phải trả khi đi qua thành phố <code>j</code>.</p>

<p>Ban đầu, bạn ở thành phố <code>0</code> và muốn đến thành phố <code>n - 1</code> trong <code>maxTime</code><strong> phút hoặc ít hơn</strong>. <strong>Chi phí</strong> của hành trình là <strong>tổng các khoản phí</strong> của mỗi thành phố mà bạn đã đi qua tại một thời điểm nào đó trong hành trình (<strong>bao gồm</strong> cả thành phố xuất phát và thành phố đích).</p>

<p>Với <code>maxTime</code>, <code>edges</code> và <code>passingFees</code>, hãy trả về <em><strong>chi phí nhỏ nhất</strong> để hoàn thành hành trình, hoặc </em><code>-1</code><em> nếu không thể hoàn thành trong </em><code>maxTime</code><em> phút</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1928.Minimum%20Cost%20to%20Reach%20Destination%20in%20Time/images/leetgraph1-1.png" style="width: 371px; height: 171px;" /></p>

<pre>
<strong>Đầu vào:</strong> maxTime = 30, edges = [[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Đường đi được chọn là 0 -&gt; 1 -&gt; 2 -&gt; 5, mất 30 phút và có tổng phí đi qua là $11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1928.Minimum%20Cost%20to%20Reach%20Destination%20in%20Time/images/copy-of-leetgraph1-1.png" style="width: 371px; height: 171px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> maxTime = 29, edges = [[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]
<strong>Đầu ra:</strong> 48
<strong>Giải thích:</strong> Đường đi được chọn là 0 -&gt; 3 -&gt; 4 -&gt; 5, mất 26 phút và có tổng phí đi qua là $48.
Bạn không thể đi theo đường 0 -&gt; 1 -&gt; 2 -&gt; 5 vì đường đi đó mất quá nhiều thời gian.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> maxTime = 25, edges = [[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có cách nào đi từ thành phố 0 đến thành phố 5 trong vòng 25 phút.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= maxTime &lt;= 1000</code></li>
	<li><code>n == passingFees.length</code></li>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>n - 1 &lt;= edges.length &lt;= 1000</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>1 &lt;= time<sub>i</sub> &lt;= 1000</code></li>
	<li><code>1 &lt;= passingFees[j] &lt;= 1000</code>&nbsp;</li>
	<li>Đồ thị có thể chứa nhiều cạnh giữa hai nút.</li>
	<li>Đồ thị không chứa khuyên.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các cạnh mang cả thời gian và phí đi qua. Nếu chỉ tối ưu một trong hai chiều, ta sẽ bỏ qua thời hạn hoặc chi phí nhỏ nhất. Vì $\textit{maxTime}\le 1000$ và $n\le 1000$, ta có thể dùng thời gian làm chỉ số DP.
>
> Gọi $f[i][j]$ là phí nhỏ nhất để đến thành phố $j$ sau đúng $i$ phút. Tăng dần $i$ và nới lỏng mọi cạnh tương đương với việc tìm đường đi ngắn nhất trên đồ thị thời gian–thành phố.
>
> Đáp án là giá trị nhỏ nhất của $f[\cdot][n-1]$ trên mọi thời gian khả thi, hoặc $-1$ nếu không có thời gian nào phù hợp.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là chi phí nhỏ nhất để đi từ thành phố $0$ đến thành phố $j$ sau $i$ phút. Ban đầu, $f[0][0] = \textit{passingFees}[0]$, còn các giá trị $f[0][j] = +\infty$ khác.

Tiếp theo, trong khoảng thời gian $[1, \textit{maxTime}]$, ta duyệt qua tất cả các cạnh. Với mỗi cạnh $(x, y, t)$, nếu $t \leq i$, ta thực hiện:

- Trước tiên có thể dùng $i - t$ phút để đi từ thành phố $0$ đến thành phố $y$, sau đó dùng $t$ phút để đi từ thành phố $y$ đến thành phố $x$ và cộng phí đi qua thành phố $x$, tức là $f[i][x] = \min(f[i][x], f[i - t][y] + \textit{passingFees}[x])$;
- Cũng có thể trước tiên dùng $i - t$ phút để đi từ thành phố $0$ đến thành phố $x$, sau đó dùng $t$ phút để đi từ thành phố $x$ đến thành phố $y$ và cộng phí đi qua thành phố $y$, tức là $f[i][y] = \min(f[i][y], f[i - t][x] + \textit{passingFees}[y])$.

Đáp án cuối cùng là $\min\{f[i][n - 1]\}$, trong đó $i \in [0, \textit{maxTime}]$. Nếu đáp án là $+\infty$, trả về $-1$.

Độ phức tạp thời gian là $O(\textit{maxTime} \times (m + n))$, trong đó $m$ và $n$ lần lượt là số cạnh và số thành phố. Độ phức tạp không gian là $O(\textit{maxTime} \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(
        self, maxTime: int, edges: List[List[int]], passingFees: List[int]
    ) -> int:
        m, n = maxTime, len(passingFees)
        f = [[inf] * n for _ in range(m + 1)]
        f[0][0] = passingFees[0]
        for i in range(1, m + 1):
            for x, y, t in edges:
                if t <= i:
                    f[i][x] = min(f[i][x], f[i - t][y] + passingFees[x])
                    f[i][y] = min(f[i][y], f[i - t][x] + passingFees[y])
        ans = min(f[i][n - 1] for i in range(m + 1))
        return ans if ans < inf else -1
```

#### Java

```java
class Solution {
    public int minCost(int maxTime, int[][] edges, int[] passingFees) {
        int m = maxTime, n = passingFees.length;
        int[][] f = new int[m + 1][n];
        final int inf = 1 << 30;
        for (var g : f) {
            Arrays.fill(g, inf);
        }
        f[0][0] = passingFees[0];
        for (int i = 1; i <= m; ++i) {
            for (var e : edges) {
                int x = e[0], y = e[1], t = e[2];
                if (t <= i) {
                    f[i][x] = Math.min(f[i][x], f[i - t][y] + passingFees[x]);
                    f[i][y] = Math.min(f[i][y], f[i - t][x] + passingFees[y]);
                }
            }
        }
        int ans = inf;
        for (int i = 0; i <= m; ++i) {
            ans = Math.min(ans, f[i][n - 1]);
        }
        return ans == inf ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(int maxTime, vector<vector<int>>& edges, vector<int>& passingFees) {
        int m = maxTime, n = passingFees.size();
        const int inf = 1 << 30;
        vector<vector<int>> f(m + 1, vector<int>(n, inf));
        f[0][0] = passingFees[0];
        for (int i = 1; i <= m; ++i) {
            for (const auto& e : edges) {
                int x = e[0], y = e[1], t = e[2];
                if (t <= i) {
                    f[i][x] = min(f[i][x], f[i - t][y] + passingFees[x]);
                    f[i][y] = min(f[i][y], f[i - t][x] + passingFees[y]);
                }
            }
        }
        int ans = inf;
        for (int i = 1; i <= m; ++i) {
            ans = min(ans, f[i][n - 1]);
        }
        return ans == inf ? -1 : ans;
    }
};
```

#### Go

```go
func minCost(maxTime int, edges [][]int, passingFees []int) int {
	m, n := maxTime, len(passingFees)
	f := make([][]int, m+1)
	const inf int = 1 << 30
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = inf
		}
	}
	f[0][0] = passingFees[0]
	for i := 1; i <= m; i++ {
		for _, e := range edges {
			x, y, t := e[0], e[1], e[2]
			if t <= i {
				f[i][x] = min(f[i][x], f[i-t][y]+passingFees[x])
				f[i][y] = min(f[i][y], f[i-t][x]+passingFees[y])
			}
		}
	}
	ans := inf
	for i := 1; i <= m; i++ {
		ans = min(ans, f[i][n-1])
	}
	if ans == inf {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minCost(maxTime: number, edges: number[][], passingFees: number[]): number {
    const [m, n] = [maxTime, passingFees.length];
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n).fill(Infinity));
    f[0][0] = passingFees[0];
    for (let i = 1; i <= m; ++i) {
        for (const [x, y, t] of edges) {
            if (t <= i) {
                f[i][x] = Math.min(f[i][x], f[i - t][y] + passingFees[x]);
                f[i][y] = Math.min(f[i][y], f[i - t][x] + passingFees[y]);
            }
        }
    }
    let ans = Infinity;
    for (let i = 1; i <= m; ++i) {
        ans = Math.min(ans, f[i][n - 1]);
    }
    return ans === Infinity ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
