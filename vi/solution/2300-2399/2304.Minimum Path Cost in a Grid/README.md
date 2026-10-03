---
comments: true
difficulty: Medium
rating: 1658
source: Weekly Contest 297 Q2
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [2304. Minimum Path Cost in a Grid](https://leetcode.com/problems/minimum-path-cost-in-a-grid)

[中文文档](/solution/2300-2399/2304.Minimum%20Path%20Cost%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận số nguyên <code>m x n</code> <code>grid</code> được đánh chỉ số từ <strong>0</strong>, gồm các số nguyên <strong>khác nhau</strong> từ <code>0</code> đến <code>m * n - 1</code>. Trong ma trận này, bạn có thể di chuyển từ một ô đến bất kỳ ô nào ở hàng <strong>tiếp theo</strong>. Cụ thể, nếu đang ở ô <code>(x, y)</code> với <code>x &lt; m - 1</code>, bạn có thể di chuyển đến bất kỳ ô nào trong số <code>(x + 1, 0)</code>, <code>(x + 1, 1)</code>, ..., <code>(x + 1, n - 1)</code>. <strong>Lưu ý</strong> rằng không thể di chuyển từ các ô ở hàng cuối cùng.</p>

<p>Mỗi bước di chuyển có một chi phí được cho bởi mảng 2 chiều <code>moveCost</code> được đánh chỉ số từ <strong>0</strong>, có kích thước <code>(m * n) x n</code>, trong đó <code>moveCost[i][j]</code> là chi phí di chuyển từ một ô có giá trị <code>i</code> đến ô ở cột <code>j</code> của hàng tiếp theo. Có thể bỏ qua chi phí di chuyển từ các ô ở hàng cuối cùng của <code>grid</code>.</p>

<p>Chi phí của một đường đi trong <code>grid</code> là <strong>tổng</strong> tất cả giá trị của các ô đã đi qua cộng với <strong>tổng</strong> chi phí của mọi bước di chuyển. Trả về <em><strong>chi phí nhỏ nhất</strong> của một đường đi bắt đầu từ bất kỳ ô nào ở hàng <strong>đầu tiên</strong> và kết thúc tại bất kỳ ô nào ở hàng <strong>cuối cùng</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2304.Minimum%20Path%20Cost%20in%20a%20Grid/images/griddrawio-2.png" style="width: 301px; height: 281px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[5,3],[4,0],[2,1]], moveCost = [[9,8],[1,5],[10,12],[18,6],[2,4],[14,3]]
<strong>Đầu ra:</strong> 17
<strong>Giải thích: </strong>Đường đi có chi phí nhỏ nhất có thể là 5 -&gt; 0 -&gt; 1.
- Tổng các giá trị của những ô đã đi qua là 5 + 0 + 1 = 6.
- Chi phí di chuyển từ 5 đến 0 là 3.
- Chi phí di chuyển từ 0 đến 1 là 8.
Vậy tổng chi phí của đường đi là 6 + 3 + 8 = 17.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[5,1,2],[4,0,3]], moveCost = [[12,10,15],[20,23,8],[21,7,1],[8,1,13],[9,10,25],[5,3,2]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Đường đi có chi phí nhỏ nhất có thể là 2 -&gt; 3.
- Tổng các giá trị của những ô đã đi qua là 2 + 3 = 5.
- Chi phí di chuyển từ 2 đến 3 là 1.
Vậy tổng chi phí của đường đi là 5 + 1 = 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 50</code></li>
	<li><code>grid</code> gồm các số nguyên khác nhau từ <code>0</code> đến <code>m * n - 1</code>.</li>
	<li><code>moveCost.length == m * n</code></li>
	<li><code>moveCost[i].length == n</code></li>
	<li><code>1 &lt;= moveCost[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước đi đều phải chuyển sang hàng tiếp theo. Nếu thử tất cả các đường đi thì có khoảng $n^{m-1}$ khả năng, không thể thực hiện được với $m, n \le 50$.
>
> Chi phí nhỏ nhất để đến ô $(i,j)$ chỉ phụ thuộc vào các giá trị tốt nhất ở hàng trước và chi phí di chuyển đến ô này. Ta tính lần lượt theo từng hàng: cập nhật mỗi cột từ $n$ trạng thái của hàng trước, đồng thời chỉ giữ lại hàng trước đó bằng rolling array.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là chi phí nhỏ nhất của đường đi từ hàng đầu tiên đến hàng thứ $i$ và cột $j$. Vì chỉ có thể di chuyển từ một cột ở hàng trước đến một cột ở hàng hiện tại, giá trị của $f[i][j]$ có thể được chuyển từ $f[i - 1][k]$, trong đó $k$ nằm trong khoảng $[0, n - 1]$. Do đó, công thức chuyển trạng thái là:

$$
f[i][j] = \min_{0 \leq k < n} \{f[i - 1][k] + \textit{moveCost}[grid[i - 1][k]][j] + grid[i][j]\}
$$

trong đó, $\textit{moveCost}[grid[i - 1][k]][j]$ biểu diễn chi phí di chuyển từ cột thứ $k$ của hàng thứ $i - 1$ đến cột thứ $j$ của hàng thứ $i$.

Đáp án cuối cùng là $\min_{0 \leq j < n} \{f[m - 1][j]\}$.

Vì mỗi lần chuyển trạng thái chỉ cần trạng thái của hàng trước đó, ta có thể dùng rolling array để tối ưu độ phức tạp không gian xuống còn $O(n)$.

Độ phức tạp thời gian là $O(m \times n^2)$ và độ phức tạp không gian là $O(n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của ma trận grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minPathCost(self, grid: List[List[int]], moveCost: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        f = grid[0]
        for i in range(1, m):
            g = [inf] * n
            for j in range(n):
                for k in range(n):
                    g[j] = min(g[j], f[k] + moveCost[grid[i - 1][k]][j] + grid[i][j])
            f = g
        return min(f)
```

#### Java

```java
class Solution {
    public int minPathCost(int[][] grid, int[][] moveCost) {
        int m = grid.length, n = grid[0].length;
        int[] f = grid[0];
        final int inf = 1 << 30;
        for (int i = 1; i < m; ++i) {
            int[] g = new int[n];
            Arrays.fill(g, inf);
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < n; ++k) {
                    g[j] = Math.min(g[j], f[k] + moveCost[grid[i - 1][k]][j] + grid[i][j]);
                }
            }
            f = g;
        }

        // return Arrays.stream(f).min().getAsInt();
        int ans = inf;
        for (int v : f) {
            ans = Math.min(ans, v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minPathCost(vector<vector<int>>& grid, vector<vector<int>>& moveCost) {
        int m = grid.size(), n = grid[0].size();
        const int inf = 1 << 30;
        vector<int> f = grid[0];
        for (int i = 1; i < m; ++i) {
            vector<int> g(n, inf);
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < n; ++k) {
                    g[j] = min(g[j], f[k] + moveCost[grid[i - 1][k]][j] + grid[i][j]);
                }
            }
            f = move(g);
        }
        return *min_element(f.begin(), f.end());
    }
};
```

#### Go

```go
func minPathCost(grid [][]int, moveCost [][]int) int {
	m, n := len(grid), len(grid[0])
	f := grid[0]
	for i := 1; i < m; i++ {
		g := make([]int, n)
		for j := 0; j < n; j++ {
			g[j] = 1 << 30
			for k := 0; k < n; k++ {
				g[j] = min(g[j], f[k]+moveCost[grid[i-1][k]][j]+grid[i][j])
			}
		}
		f = g
	}
	return slices.Min(f)
}
```

#### TypeScript

```ts
function minPathCost(grid: number[][], moveCost: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const f = grid[0];
    for (let i = 1; i < m; ++i) {
        const g: number[] = Array(n).fill(Infinity);
        for (let j = 0; j < n; ++j) {
            for (let k = 0; k < n; ++k) {
                g[j] = Math.min(g[j], f[k] + moveCost[grid[i - 1][k]][j] + grid[i][j]);
            }
        }
        f.splice(0, n, ...g);
    }
    return Math.min(...f);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_path_cost(grid: Vec<Vec<i32>>, move_cost: Vec<Vec<i32>>) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let mut f = grid[0].clone();

        for i in 1..m {
            let mut g: Vec<i32> = vec![i32::MAX; n];
            for j in 0..n {
                for k in 0..n {
                    g[j] = g[j].min(f[k] + move_cost[grid[i - 1][k] as usize][j] + grid[i][j]);
                }
            }
            f.copy_from_slice(&g);
        }

        f.iter().cloned().min().unwrap_or(0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
