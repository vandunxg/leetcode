---
comments: true
difficulty: Hard
rating: 1697
source: Biweekly Contest 15 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [1289. Minimum Falling Path Sum II](https://leetcode.com/problems/minimum-falling-path-sum-ii)

[中文文档](/solution/1200-1299/1289.Minimum%20Falling%20Path%20Sum%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận số nguyên <code>n x n</code> <code>grid</code>, hãy trả về <em>tổng nhỏ nhất của một <strong>đường đi giảm dần có dịch chuyển khác 0</strong></em>.</p>

<p><strong>Đường đi giảm dần có dịch chuyển khác 0</strong> chọn đúng một phần tử từ mỗi hàng của <code>grid</code>, sao cho hai phần tử được chọn ở hai hàng kề nhau không nằm cùng một cột.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1289.Minimum%20Falling%20Path%20Sum%20II/images/falling-grid.jpg" style="width: 244px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2,3],[4,5,6],[7,8,9]]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> 
Các đường đi giảm dần có thể là:
[1,5,9], [1,5,7], [1,6,7], [1,6,8],
[2,4,8], [2,4,9], [2,6,7], [2,6,8],
[3,4,8], [3,4,9], [3,5,7], [3,5,9]
Đường đi giảm dần có tổng nhỏ nhất là&nbsp;[1,5,7], nên đáp án là&nbsp;13.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[7]]
<strong>Đầu ra:</strong> 7
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>-99 &lt;= grid[i][j] &lt;= 99</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming (Mảng cuộn)

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi giảm dần không được chọn lại cùng một cột ở hàng kế tiếp. Vì $n \le 200$, độ phức tạp $O(n^3)$ là chấp nhận được. Tổng nhỏ nhất khi kết thúc hàng $i$ ở cột $j$ bằng giá trị nhỏ nhất của hàng trước, bỏ qua cột $j$, cộng với $grid[i][j]$.
>
> Ta chỉ cần giữ $n$ giá trị của hàng trước và với mỗi cột, cộng giá trị nhỏ nhất ở các cột còn lại. Dùng mảng cuộn để loại bỏ chiều hàng.

<!-- thinking:end -->

Gọi $f[i][j]$ là tổng đường đi nhỏ nhất khi xét $i$ hàng đầu tiên và kết thúc ở cột $j$:

$$
f[i][j] = \min_{k \neq j} f[i - 1][k] + \textit{grid}[i - 1][j]
$$

Đáp án là $\min_{0 \leq j < n} f[n][j]$. Sau khi cuộn trạng thái, chỉ còn hàng cuối, nên đáp án là giá trị nhỏ nhất trong mảng một chiều $f$.

$f[i][j]$ chỉ phụ thuộc vào hàng trước đó, nên ta chỉ cần hai mảng $f$ và $g$ có độ dài $n$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số hàng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minFallingPathSum(self, grid: List[List[int]]) -> int:
        n = len(grid)
        f = [0] * n
        for row in grid:
            g = row[:]
            for i in range(n):
                g[i] += min((f[j] for j in range(n) if j != i), default=0)
            f = g
        return min(f)
```

#### Java

```java
class Solution {
    public int minFallingPathSum(int[][] grid) {
        int n = grid.length;
        int[] f = new int[n];
        final int inf = 1 << 30;
        for (int[] row : grid) {
            int[] g = row.clone();
            for (int i = 0; i < n; ++i) {
                int t = inf;
                for (int j = 0; j < n; ++j) {
                    if (j != i) {
                        t = Math.min(t, f[j]);
                    }
                }
                g[i] += (t == inf ? 0 : t);
            }
            f = g;
        }
        return Arrays.stream(f).min().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<int> f(n);
        const int inf = 1e9;
        for (const auto& row : grid) {
            vector<int> g = row;
            for (int i = 0; i < n; ++i) {
                int t = inf;
                for (int j = 0; j < n; ++j) {
                    if (j != i) {
                        t = min(t, f[j]);
                    }
                }
                g[i] += (t == inf ? 0 : t);
            }
            f = move(g);
        }
        return ranges::min(f);
    }
};
```

#### Go

```go
func minFallingPathSum(grid [][]int) int {
	f := make([]int, len(grid))
	const inf = math.MaxInt32
	for _, row := range grid {
		g := slices.Clone(row)
		for i := range f {
			t := inf
			for j := range row {
				if j != i {
					t = min(t, f[j])
				}
			}
			if t != inf {
				g[i] += t
			}
		}
		f = g
	}
	return slices.Min(f)
}
```

#### TypeScript

```ts
function minFallingPathSum(grid: number[][]): number {
    const n = grid.length;
    const f: number[] = Array(n).fill(0);
    for (const row of grid) {
        const g = [...row];
        for (let i = 0; i < n; ++i) {
            let t = Infinity;
            for (let j = 0; j < n; ++j) {
                if (j !== i) {
                    t = Math.min(t, f[j]);
                }
            }
            g[i] += t === Infinity ? 0 : t;
        }
        f.splice(0, n, ...g);
    }
    return Math.min(...f);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_falling_path_sum(grid: Vec<Vec<i32>>) -> i32 {
        let n = grid.len();
        let mut f = vec![0; n];
        let inf = i32::MAX;

        for row in grid {
            let mut g = row.clone();
            for i in 0..n {
                let mut t = inf;
                for j in 0..n {
                    if j != i {
                        t = t.min(f[j]);
                    }
                }
                g[i] += if t == inf { 0 } else { t };
            }
            f = g;
        }

        *f.iter().min().unwrap()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
