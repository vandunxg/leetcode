---
comments: true
difficulty: Medium
rating: 1498
source: Weekly Contest 387 Q2
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [3070. Count Submatrices with Top-Left Element and Sum Less Than k](https://leetcode.com/problems/count-submatrices-with-top-left-element-and-sum-less-than-k)

[中文文档](/solution/3000-3099/3070.Count%20Submatrices%20with%20Top-Left%20Element%20and%20Sum%20Less%20Than%20k/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>grid</code> được đánh chỉ số từ <strong>0</strong> và một số nguyên <code>k</code>.</p>

<p>Trả về <em><strong>số lượng</strong> <span data-keyword="submatrix">ma trận con</span> chứa phần tử trên cùng bên trái của</em> <code>grid</code>, <em>và có tổng nhỏ hơn hoặc bằng </em><code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3070.Count%20Submatrices%20with%20Top-Left%20Element%20and%20Sum%20Less%20Than%20k/images/example1.png" style="padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> grid = [[7,6,3],[6,6,1]], k = 18
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Chỉ có 4 ma trận con, được minh họa trong hình trên, chứa phần tử trên cùng bên trái của grid và có tổng nhỏ hơn hoặc bằng 18.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3070.Count%20Submatrices%20with%20Top-Left%20Element%20and%20Sum%20Less%20Than%20k/images/example21.png" style="padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> grid = [[7,2,9],[1,5,0],[2,6,6]], k = 20
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Chỉ có 6 ma trận con, được minh họa trong hình trên, chứa phần tử trên cùng bên trái của grid và có tổng nhỏ hơn hoặc bằng 20.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length </code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= n, m &lt;= 1000 </code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố hai chiều

<!-- thinking:start -->

> **Tư duy**
>
> Mọi ma trận con được đếm đều chứa ô trên cùng bên trái, nên chúng là các ma trận con tiền tố. Với $n,m \le 1000$, không thể tính tổng lại từ đầu cho từng ma trận.
>
> Bảng tổng tiền tố 2 chiều cho tổng của ma trận con kết thúc tại $(i,j)$ trong $O(1)$, sau đó ta so sánh tổng này với $k$.
>
> Ta điền $s_{i,j}=s_{i-1,j}+s_{i,j-1}-s_{i-1,j-1}+x$.

<!-- thinking:end -->

Thực chất, bài toán yêu cầu đếm số ma trận con tiền tố trong một ma trận hai chiều có tổng nhỏ hơn hoặc bằng $k$.

Công thức tính tổng tiền tố hai chiều là:

$$
s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + x
$$

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubmatrices(self, grid: List[List[int]], k: int) -> int:
        s = [[0] * (len(grid[0]) + 1) for _ in range(len(grid) + 1)]
        ans = 0
        for i, row in enumerate(grid, 1):
            for j, x in enumerate(row, 1):
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + x
                ans += s[i][j] <= k
        return ans
```

#### Java

```java
class Solution {
    public int countSubmatrices(int[][] grid, int k) {
        int m = grid.length, n = grid[0].length;
        int[][] s = new int[m + 1][n + 1];
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + grid[i - 1][j - 1];
                if (s[i][j] <= k) {
                    ++ans;
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
    int countSubmatrices(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        int s[m + 1][n + 1];
        memset(s, 0, sizeof(s));
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + grid[i - 1][j - 1];
                if (s[i][j] <= k) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSubmatrices(grid [][]int, k int) (ans int) {
	s := make([][]int, len(grid)+1)
	for i := range s {
		s[i] = make([]int, len(grid[0])+1)
	}
	for i, row := range grid {
		for j, x := range row {
			s[i+1][j+1] = s[i+1][j] + s[i][j+1] - s[i][j] + x
			if s[i+1][j+1] <= k {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countSubmatrices(grid: number[][], k: number): number {
    const m = grid.length;
    const n = grid[0].length;
    const s: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    let ans: number = 0;
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + grid[i - 1][j - 1];
            if (s[i][j] <= k) {
                ++ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_submatrices(grid: Vec<Vec<i32>>, k: i32) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let mut s = vec![vec![0; n + 1]; m + 1];
        let mut ans = 0;
        for i in 1..=m {
            for j in 1..=n {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + grid[i - 1][j - 1];
                if s[i][j] <= k { ans += 1; }
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int CountSubmatrices(int[][] grid, int k) {
        int m = grid.Length, n = grid[0].Length;
        int[,] s = new int[m + 1, n + 1];
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                s[i, j] = s[i - 1, j] + s[i, j - 1] - s[i - 1, j - 1] + grid[i - 1][j - 1];
                if (s[i, j] <= k) ++ans;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
