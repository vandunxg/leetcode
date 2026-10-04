---
comments: true
difficulty: Hard
rating: 3027
source: Biweekly Contest 135 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [3225. Maximum Score From Grid Operations](https://leetcode.com/problems/maximum-score-from-grid-operations)

[中文文档](/solution/3200-3299/3225.Maximum%20Score%20From%20Grid%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận 2D <code>grid</code> có kích thước <code>n x n</code>. Ban đầu, tất cả các ô của ma trận đều có màu trắng. Trong một thao tác, bạn có thể chọn một ô có chỉ số <code>(i, j)</code>, rồi tô đen tất cả các ô trong cột thứ <code>j<sup>th</sup></code> từ hàng đầu tiên đến hàng thứ <code>i<sup>th</sup></code>.</p>

<p>Điểm của ma trận là tổng của tất cả <code>grid[i][j]</code> sao cho ô <code>(i, j)</code> có màu trắng và có một ô màu đen kề bên theo chiều ngang.</p>

<p>Hãy trả về <strong>điểm lớn nhất</strong> có thể đạt được sau một số thao tác bất kỳ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,0,0,0,0],[0,0,3,0,0],[0,1,0,0,0],[5,0,0,3,0],[0,0,0,0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3225.Maximum%20Score%20From%20Grid%20Operations/images/one.png" style="width: 300px; height: 200px;" />
<p>Trong thao tác đầu tiên, ta tô đen tất cả các ô trong cột 1 đến hàng 3, còn trong thao tác thứ hai, ta tô đen tất cả các ô trong cột 4 đến hàng cuối cùng. Điểm của ma trận sau các thao tác là <code>grid[3][0] + grid[1][2] + grid[3][3]</code>, bằng 11.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[10,9,0,0,15],[7,1,0,8,0],[5,20,0,11,0],[0,0,0,1,2],[8,12,1,10,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">94</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3225.Maximum%20Score%20From%20Grid%20Operations/images/two-1.png" style="width: 300px; height: 200px;" />
<p>Ta thực hiện các thao tác trên các cột 1, 2 và 3, lần lượt đến các hàng 1, 4 và 0. Điểm của ma trận sau các thao tác là <code>grid[0][0] + grid[1][0] + grid[2][1] + grid[4][1] + grid[1][3] + grid[2][3] + grid[3][3] + grid[4][3] + grid[0][4]</code>, bằng 94.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;=&nbsp;n == grid.length &lt;= 100</code></li>
    <li><code>n == grid[i].length</code></li>
    <li><code>0 &lt;= grid[i][j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cột được tô đen từ trên xuống với một chiều cao; một ô trắng chỉ được tính điểm nếu có một cột kề bên bị tô đen tại hàng đó. Vì $n\le 100$, việc liệt kê các bộ chiều cao sẽ có $(n+1)^n$ khả năng, nên không phù hợp. Điểm của cột $j$ chỉ phụ thuộc vào chiều cao của cột đó và hai cột kề bên, từ đó gợi ý sử dụng quy hoạch động theo cột.
>
> $f[h_1][h_2]$ là điểm lớn nhất khi cột hiện tại có chiều cao $h_1$ và cột trước đó có chiều cao $h_2$. Khi liệt kê chiều cao tiếp theo, giá trị cộng thêm có dạng từng đoạn theo $\max(h_2,h_p)$, vì vậy ta có thể dùng prefix/suffix maximum trên $h_2$ để giảm chi phí xử lý mỗi cột từ $O(n^3)$ xuống $O(n^2)$. Prefix sum theo cột được dùng để tính trước tổng các đoạn ô trắng.

<!-- thinking:end -->

Với mỗi cột $j$, đặt $k[j] \in \{0, 1, \ldots, n\}$ là số ô được tô đen tính từ trên xuống. Một ô trắng $(i, j)$ được tính điểm khi và chỉ khi có ít nhất một ô kề bên theo chiều ngang bị tô đen, và mỗi ô chỉ được tính một lần. Do đó, phần đóng góp của cột $j$ là:

$$
\max\bigl(0,\ s[j][\max(k[j-1], k[j+1])] - s[j][k[j]]\bigr)
$$

trong đó $s[j][h]$ là prefix sum của $h$ ô đầu tiên trong cột $j$ (chiều cao của các cột biên được xem là $0$).

Gọi $f[h_1][h_2]$ là điểm lớn nhất sau khi xử lý cột $j$, với $k[j] = h_1$ và $k[j-1] = h_2$. Khi chọn chiều cao tiếp theo $hp = k[j+1]$:

$$
g[hp][h_1] = \max_{h_2}\bigl(f[h_1][h_2] + \max(0,\ s[j][\max(h_2, hp)] - s[j][h_1])\bigr)
$$

Chia phép chuyển trạng thái thành hai trường hợp $h_2 \le hp$ và $h_2 > hp$, rồi duy trì prefix / suffix maximum để chi phí xử lý mỗi cột là $O(n^2)$ thay vì $O(n^3)$.

Độ phức tạp thời gian là $O(n^3)$, còn độ phức tạp không gian là $O(n^2)$, trong đó $n$ là kích thước của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, grid: List[List[int]]) -> int:
        n = len(grid)
        s = [[0] * (n + 1) for _ in range(n)]
        for j in range(n):
            for i, x in enumerate(grid):
                s[j][i + 1] = s[j][i] + x[j]
        f = [[-inf] * (n + 1) for _ in range(n + 1)]
        for h in range(n + 1):
            f[h][0] = 0
        for j in range(n - 1):
            g = [[-inf] * (n + 1) for _ in range(n + 1)]
            for h1 in range(n + 1):
                pre = [-inf] * (n + 2)
                pre[0] = f[h1][0]
                for h2 in range(1, n + 1):
                    pre[h2] = max(pre[h2 - 1], f[h1][h2])
                suf = [-inf] * (n + 2)
                for h2 in range(n, -1, -1):
                    v = -inf
                    if f[h1][h2] != -inf:
                        v = f[h1][h2] + max(0, s[j][h2] - s[j][h1])
                    suf[h2] = max(suf[h2 + 1], v)
                for hp in range(n + 1):
                    add = max(0, s[j][hp] - s[j][h1])
                    v1 = -inf if pre[hp] == -inf else pre[hp] + add
                    g[hp][h1] = max(v1, suf[hp + 1])
            f = g
        ans = 0
        for h1 in range(n + 1):
            for h2 in range(n + 1):
                if f[h1][h2] != -inf:
                    ans = max(ans, f[h1][h2] + max(0, s[-1][h2] - s[-1][h1]))
        return ans
```

#### Java

```java
class Solution {
    public long maximumScore(int[][] grid) {
        int n = grid.length;
        final long inf = Long.MIN_VALUE / 2;
        long[][] s = new long[n][n + 1];
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < n; ++i) {
                s[j][i + 1] = s[j][i] + grid[i][j];
            }
        }
        long[][] f = new long[n + 1][n + 1];
        for (long[] row : f) {
            Arrays.fill(row, inf);
        }
        for (int h = 0; h <= n; ++h) {
            f[h][0] = 0;
        }
        for (int j = 0; j < n - 1; ++j) {
            long[][] g = new long[n + 1][n + 1];
            for (long[] row : g) {
                Arrays.fill(row, inf);
            }
            for (int h1 = 0; h1 <= n; ++h1) {
                long[] pre = new long[n + 2];
                pre[0] = f[h1][0];
                for (int h2 = 1; h2 <= n; ++h2) {
                    pre[h2] = Math.max(pre[h2 - 1], f[h1][h2]);
                }
                long[] suf = new long[n + 2];
                Arrays.fill(suf, inf);
                for (int h2 = n; h2 >= 0; --h2) {
                    long v = f[h1][h2] == inf ? inf : f[h1][h2] + Math.max(0, s[j][h2] - s[j][h1]);
                    suf[h2] = Math.max(suf[h2 + 1], v);
                }
                for (int hp = 0; hp <= n; ++hp) {
                    long add = Math.max(0, s[j][hp] - s[j][h1]);
                    long v1 = pre[hp] == inf ? inf : pre[hp] + add;
                    g[hp][h1] = Math.max(v1, suf[hp + 1]);
                }
            }
            f = g;
        }
        long ans = 0;
        for (int h1 = 0; h1 <= n; ++h1) {
            for (int h2 = 0; h2 <= n; ++h2) {
                if (f[h1][h2] != inf) {
                    ans = Math.max(ans, f[h1][h2] + Math.max(0, s[n - 1][h2] - s[n - 1][h1]));
                }
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
    long long maximumScore(vector<vector<int>>& grid) {
        int n = grid.size();
        const long long inf = LLONG_MIN / 2;
        vector<vector<long long>> s(n, vector<long long>(n + 1));
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < n; ++i) {
                s[j][i + 1] = s[j][i] + grid[i][j];
            }
        }
        vector<vector<long long>> f(n + 1, vector<long long>(n + 1, inf));
        for (int h = 0; h <= n; ++h) {
            f[h][0] = 0;
        }
        for (int j = 0; j < n - 1; ++j) {
            vector<vector<long long>> g(n + 1, vector<long long>(n + 1, inf));
            for (int h1 = 0; h1 <= n; ++h1) {
                vector<long long> pre(n + 2, inf), suf(n + 2, inf);
                pre[0] = f[h1][0];
                for (int h2 = 1; h2 <= n; ++h2) {
                    pre[h2] = max(pre[h2 - 1], f[h1][h2]);
                }
                for (int h2 = n; h2 >= 0; --h2) {
                    long long v = f[h1][h2] == inf ? inf : f[h1][h2] + max(0LL, s[j][h2] - s[j][h1]);
                    suf[h2] = max(suf[h2 + 1], v);
                }
                for (int hp = 0; hp <= n; ++hp) {
                    long long add = max(0LL, s[j][hp] - s[j][h1]);
                    long long v1 = pre[hp] == inf ? inf : pre[hp] + add;
                    g[hp][h1] = max(v1, suf[hp + 1]);
                }
            }
            f.swap(g);
        }
        long long ans = 0;
        for (int h1 = 0; h1 <= n; ++h1) {
            for (int h2 = 0; h2 <= n; ++h2) {
                if (f[h1][h2] != inf) {
                    ans = max(ans, f[h1][h2] + max(0LL, s[n - 1][h2] - s[n - 1][h1]));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
import "math"

func maximumScore(grid [][]int) int64 {
    n := len(grid)
    const inf = math.MinInt64 / 2
    s := make([][]int64, n)
    for j := 0; j < n; j++ {
        s[j] = make([]int64, n+1)
        for i := 0; i < n; i++ {
            s[j][i+1] = s[j][i] + int64(grid[i][j])
        }
    }
    f := make([][]int64, n+1)
    for i := range f {
        f[i] = make([]int64, n+1)
        for k := range f[i] {
            f[i][k] = inf
        }
    }
    for h := 0; h <= n; h++ {
        f[h][0] = 0
    }
    for j := 0; j < n-1; j++ {
        g := make([][]int64, n+1)
        for i := range g {
            g[i] = make([]int64, n+1)
            for k := range g[i] {
                g[i][k] = inf
            }
        }
        for h1 := 0; h1 <= n; h1++ {
            pre := make([]int64, n+2)
            pre[0] = f[h1][0]
            for h2 := 1; h2 <= n; h2++ {
                pre[h2] = max(pre[h2-1], f[h1][h2])
            }
            suf := make([]int64, n+2)
            for i := range suf {
                suf[i] = inf
            }
            for h2 := n; h2 >= 0; h2-- {
                v := int64(inf)
                if f[h1][h2] != inf {
                    v = f[h1][h2] + max(int64(0), s[j][h2]-s[j][h1])
                }
                suf[h2] = max(suf[h2+1], v)
            }
            for hp := 0; hp <= n; hp++ {
                add := max(int64(0), s[j][hp]-s[j][h1])
                v1 := int64(inf)
                if pre[hp] != inf {
                    v1 = pre[hp] + add
                }
                g[hp][h1] = max(v1, suf[hp+1])
            }
        }
        f = g
    }
    var ans int64
    for h1 := 0; h1 <= n; h1++ {
        for h2 := 0; h2 <= n; h2++ {
            if f[h1][h2] != inf {
                ans = max(ans, f[h1][h2]+max(int64(0), s[n-1][h2]-s[n-1][h1]))
            }
        }
    }
    return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
