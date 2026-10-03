---
comments: true
difficulty: Medium
rating: 2105
source: Weekly Contest 250 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [1937. Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost)

[中文文档](/solution/1900-1999/1937.Maximum%20Number%20of%20Points%20with%20Cost/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>m x n</code> <code>points</code> (<strong>đánh chỉ số từ 0</strong>). Ban đầu có <code>0</code> điểm, bạn muốn <strong>tối đa hóa</strong> số điểm có thể nhận được từ ma trận.</p>

<p>Để nhận điểm, bạn phải chọn một ô trong <strong>mỗi hàng</strong>. Chọn ô có tọa độ <code>(r, c)</code> sẽ <strong>cộng</strong> <code>points[r][c]</code> vào điểm số.</p>

<p>Tuy nhiên, bạn sẽ bị trừ điểm nếu chọn một ô quá xa ô đã chọn ở hàng trước đó. Với mỗi hai hàng kề nhau <code>r</code> và <code>r + 1</code> (trong đó <code>0 &lt;= r &lt; m - 1</code>), nếu chọn các ô có tọa độ <code>(r, c<sub>1</sub>)</code> và <code>(r + 1, c<sub>2</sub>)</code> thì điểm số sẽ bị <strong>trừ</strong> <code>abs(c<sub>1</sub> - c<sub>2</sub>)</code>.</p>

<p>Hãy trả về <em><strong>số điểm tối đa</strong> có thể đạt được</em>.</p>

<p><code>abs(x)</code> được định nghĩa như sau:</p>

<ul>
	<li><code>x</code> nếu <code>x &gt;= 0</code>.</li>
	<li><code>-x</code> nếu <code>x &lt; 0</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong><strong> </strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1937.Maximum%20Number%20of%20Points%20with%20Cost/images/screenshot-2021-07-12-at-13-40-26-diagram-drawio-diagrams-net.png" style="width: 300px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,2,3],[1,5,1],[3,1,1]]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Các ô màu xanh là những ô tối ưu được chọn, có tọa độ (0, 2), (1, 1) và (2, 0).
Bạn cộng 3 + 5 + 3 = 11 vào điểm số.
Tuy nhiên, bạn phải trừ abs(2 - 1) + abs(1 - 0) = 2 khỏi điểm số.
Điểm số cuối cùng là 11 - 2 = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1937.Maximum%20Number%20of%20Points%20with%20Cost/images/screenshot-2021-07-12-at-13-42-14-diagram-drawio-diagrams-net.png" style="width: 200px; height: 299px;" />
<pre>
<strong>Đầu vào:</strong> points = [[1,5],[2,3],[4,2]]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong>
Các ô màu xanh là những ô tối ưu được chọn, có tọa độ (0, 1), (1, 1) và (2, 0).
Bạn cộng 5 + 3 + 4 = 12 vào điểm số.
Tuy nhiên, bạn phải trừ abs(1 - 1) + abs(1 - 0) = 1 khỏi điểm số.
Điểm số cuối cùng là 12 - 1 = 11.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == points.length</code></li>
	<li><code>n == points[r].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= points[r][c] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng chọn một ô và chịu một khoản phạt theo khoảng cách tuyệt đối giữa các cột. Nếu thử mọi cột của hàng trước, mỗi hàng sẽ tốn $O(n^2)$ và không phù hợp khi $mn\le 10^5$.
>
> Với phần $k\le j$, ta chỉ cần $\max(f[k]+k)$; còn phần $k\ge j$ chỉ cần $\max(f[k]-k)$. Vì vậy, ta có thể dùng prefix max từ trái sang phải và suffix max từ phải sang trái để tính mỗi ô trong $O(1)$.
>
> Dùng DP cuốn chiếu theo từng hàng giúp giảm bộ nhớ phụ xuống $O(n)$, trong khi thời gian tuyến tính theo số ô.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPoints(self, points: List[List[int]]) -> int:
        n = len(points[0])
        f = points[0][:]
        for p in points[1:]:
            g = [0] * n
            lmx = -inf
            for j in range(n):
                lmx = max(lmx, f[j] + j)
                g[j] = max(g[j], p[j] + lmx - j)
            rmx = -inf
            for j in range(n - 1, -1, -1):
                rmx = max(rmx, f[j] - j)
                g[j] = max(g[j], p[j] + rmx + j)
            f = g
        return max(f)
```

#### Java

```java
class Solution {
    public long maxPoints(int[][] points) {
        int n = points[0].length;
        long[] f = new long[n];
        final long inf = 1L << 60;
        for (int[] p : points) {
            long[] g = new long[n];
            long lmx = -inf, rmx = -inf;
            for (int j = 0; j < n; ++j) {
                lmx = Math.max(lmx, f[j] + j);
                g[j] = Math.max(g[j], p[j] + lmx - j);
            }
            for (int j = n - 1; j >= 0; --j) {
                rmx = Math.max(rmx, f[j] - j);
                g[j] = Math.max(g[j], p[j] + rmx + j);
            }
            f = g;
        }
        long ans = 0;
        for (long x : f) {
            ans = Math.max(ans, x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxPoints(vector<vector<int>>& points) {
        using ll = long long;
        int n = points[0].size();
        vector<ll> f(n);
        const ll inf = 1e18;
        for (auto& p : points) {
            vector<ll> g(n);
            ll lmx = -inf, rmx = -inf;
            for (int j = 0; j < n; ++j) {
                lmx = max(lmx, f[j] + j);
                g[j] = max(g[j], p[j] + lmx - j);
            }
            for (int j = n - 1; ~j; --j) {
                rmx = max(rmx, f[j] - j);
                g[j] = max(g[j], p[j] + rmx + j);
            }
            f = move(g);
        }
        return *max_element(f.begin(), f.end());
    }
};
```

#### Go

```go
func maxPoints(points [][]int) int64 {
	n := len(points[0])
	const inf int64 = 1e18
	f := make([]int64, n)
	for _, p := range points {
		g := make([]int64, n)
		lmx, rmx := -inf, -inf
		for j := range p {
			lmx = max(lmx, f[j]+int64(j))
			g[j] = max(g[j], int64(p[j])+lmx-int64(j))
		}
		for j := n - 1; j >= 0; j-- {
			rmx = max(rmx, f[j]-int64(j))
			g[j] = max(g[j], int64(p[j])+rmx+int64(j))
		}
		f = g
	}
	return slices.Max(f)
}
```

#### TypeScript

```ts
function maxPoints(points: number[][]): number {
    const n = points[0].length;
    const f: number[] = new Array(n).fill(0);
    for (const p of points) {
        const g: number[] = new Array(n).fill(0);
        let lmx = -Infinity;
        let rmx = -Infinity;
        for (let j = 0; j < n; ++j) {
            lmx = Math.max(lmx, f[j] + j);
            g[j] = Math.max(g[j], p[j] + lmx - j);
        }
        for (let j = n - 1; ~j; --j) {
            rmx = Math.max(rmx, f[j] - j);
            g[j] = Math.max(g[j], p[j] + rmx + j);
        }
        f.splice(0, n, ...g);
    }
    return Math.max(...f);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
