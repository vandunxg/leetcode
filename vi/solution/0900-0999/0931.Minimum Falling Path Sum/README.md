---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [931. Minimum Falling Path Sum](https://leetcode.com/problems/minimum-falling-path-sum)

[中文文档](/solution/0900-0999/0931.Minimum%20Falling%20Path%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận số nguyên <code>matrix</code> kích thước <code>n x n</code>, hãy trả về <em><strong>tổng nhỏ nhất</strong> của một <strong>đường đi rơi</strong> bất kỳ qua </em><code>matrix</code>.</p>

<p><strong>Đường đi rơi</strong> bắt đầu từ một phần tử bất kỳ ở hàng đầu tiên, sau đó mỗi bước chọn phần tử ở hàng kế tiếp nằm ngay bên dưới hoặc chéo sang trái/phải. Cụ thể, từ vị trí <code>(row, col)</code>, phần tử tiếp theo có thể ở <code>(row + 1, col - 1)</code>, <code>(row + 1, col)</code> hoặc <code>(row + 1, col + 1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0931.Minimum%20Falling%20Path%20Sum/images/failing1-grid.jpg" style="width: 499px; height: 500px;" />
<pre>
<strong>Input:</strong> matrix = [[2,1,3],[6,5,4],[7,8,9]]
<strong>Output:</strong> 13
<strong>Giải thích:</strong> Có hai đường đi rơi đạt tổng nhỏ nhất như hình minh họa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0931.Minimum%20Falling%20Path%20Sum/images/failing2-grid.jpg" style="width: 164px; height: 365px;" />
<pre>
<strong>Input:</strong> matrix = [[-19,57],[-40,-5]]
<strong>Output:</strong> -59
<strong>Giải thích:</strong> Đường đi rơi có tổng nhỏ nhất được minh họa trong hình.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == matrix.length == matrix[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>-100 &lt;= matrix[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (Rolling Array)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước của đường đi rơi chỉ có thể đến một trong ba ô lân cận ở hàng tiếp theo, và $n\le 100$, nên không thể liệt kê mọi đường đi. Đường đi tốt nhất đến $(i,j)$ chỉ phụ thuộc vào các ô $j-1,j,j+1$ ở hàng trước. Ta tính lần lượt theo từng hàng và luân phiên dùng mảng một chiều, với $O(n)$ bộ nhớ phụ.

<!-- thinking:end -->

Gọi $f[i][j]$ là tổng đường đi rơi nhỏ nhất kết thúc tại hàng $i$, cột $j$:

$$
f[i][j] = \textit{matrix}[i][j] + \min \left\{ \begin{aligned} & f[i - 1][j - 1], & j > 0 \\ & f[i - 1][j], & 0 \leq j < n \\ & f[i - 1][j + 1], & j + 1 < n \end{aligned} \right.
$$

Đáp án là $\min_{0 \leq j < n} f[n - 1][j]$.

$f[i][j]$ chỉ phụ thuộc vào hàng trước đó, vì vậy ta chỉ cần giữ hai mảng $f$ và $g$ có độ dài $n$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là kích thước cạnh của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minFallingPathSum(self, matrix: List[List[int]]) -> int:
        n = len(matrix)
        f = [0] * n
        for row in matrix:
            g = [0] * n
            for j, x in enumerate(row):
                l, r = max(0, j - 1), min(n, j + 2)
                g[j] = min(f[l:r]) + x
            f = g
        return min(f)
```

#### Java

```java
class Solution {
    public int minFallingPathSum(int[][] matrix) {
        int n = matrix.length;
        var f = new int[n];
        for (var row : matrix) {
            var g = f.clone();
            for (int j = 0; j < n; ++j) {
                if (j > 0) {
                    g[j] = Math.min(g[j], f[j - 1]);
                }
                if (j + 1 < n) {
                    g[j] = Math.min(g[j], f[j + 1]);
                }
                g[j] += row[j];
            }
            f = g;
        }
        // return Arrays.stream(f).min().getAsInt();
        int ans = 1 << 30;
        for (int x : f) {
            ans = Math.min(ans, x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int n = matrix.size();
        vector<int> f(n);
        for (auto& row : matrix) {
            auto g = f;
            for (int j = 0; j < n; ++j) {
                if (j) {
                    g[j] = min(g[j], f[j - 1]);
                }
                if (j + 1 < n) {
                    g[j] = min(g[j], f[j + 1]);
                }
                g[j] += row[j];
            }
            f = move(g);
        }
        return *min_element(f.begin(), f.end());
    }
};
```

#### Go

```go
func minFallingPathSum(matrix [][]int) int {
	n := len(matrix)
	f := make([]int, n)
	for _, row := range matrix {
		g := make([]int, n)
		copy(g, f)
		for j, x := range row {
			if j > 0 {
				g[j] = min(g[j], f[j-1])
			}
			if j+1 < n {
				g[j] = min(g[j], f[j+1])
			}
			g[j] += x
		}
		f = g
	}
	return slices.Min(f)
}
```

#### TypeScript

```ts
function minFallingPathSum(matrix: number[][]): number {
    const n = matrix.length;
    const f: number[] = new Array(n).fill(0);
    for (const row of matrix) {
        const g = f.slice();
        for (let j = 0; j < n; ++j) {
            if (j > 0) {
                g[j] = Math.min(g[j], f[j - 1]);
            }
            if (j + 1 < n) {
                g[j] = Math.min(g[j], f[j + 1]);
            }
            g[j] += row[j];
        }
        f.splice(0, n, ...g);
    }
    return Math.min(...f);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
