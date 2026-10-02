---
comments: true
difficulty: Medium
rating: 1807
source: Weekly Contest 207 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [1594. Maximum Non Negative Product in a Matrix](https://leetcode.com/problems/maximum-non-negative-product-in-a-matrix)

[中文文档](/solution/1500-1599/1594.Maximum%20Non%20Negative%20Product%20in%20a%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> <code>grid</code>. Ban đầu, bạn ở góc trên bên trái <code>(0, 0)</code>, và mỗi bước chỉ có thể <strong>di chuyển sang phải hoặc xuống dưới</strong> trong ma trận.</p>

<p>Trong mọi đường đi từ góc trên bên trái <code>(0, 0)</code> đến góc dưới bên phải <code>(m - 1, n - 1)</code>, hãy tìm đường đi có <strong>tích không âm lớn nhất</strong>. Tích của một đường đi là tích của tất cả số nguyên trong các ô đã đi qua.</p>

<p>Trả về <em>tích không âm lớn nhất <strong>theo modulo</strong> </em><code>10<sup>9</sup> + 7</code>. <em>Nếu tích lớn nhất là <strong>âm</strong>, trả về </em><code>-1</code>.</p>

<p>Lưu ý rằng phép modulo được thực hiện sau khi tìm tích lớn nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1594.Maximum%20Non%20Negative%20Product%20in%20a%20Matrix/images/product1.jpg" style="width: 244px; height: 245px;" />
<pre>
<strong>Input:</strong> grid = [[-1,-2,-3],[-2,-3,-3],[-3,-3,-2]]
<strong>Output:</strong> -1
<strong>Explanation:</strong> It is not possible to get non-negative product in the path from (0, 0) to (2, 2), so return -1.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1594.Maximum%20Non%20Negative%20Product%20in%20a%20Matrix/images/product2.jpg" style="width: 244px; height: 245px;" />
<pre>
<strong>Input:</strong> grid = [[1,-2,1],[1,-2,1],[3,-4,1]]
<strong>Output:</strong> 8
<strong>Explanation:</strong> Maximum non-negative product is shown (1 * 1 * -2 * -4 * 1 = 8).
</pre>

<p><strong class="example">Example 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1594.Maximum%20Non%20Negative%20Product%20in%20a%20Matrix/images/product3.jpg" style="width: 164px; height: 165px;" />
<pre>
<strong>Input:</strong> grid = [[1,3],[0,-4]]
<strong>Output:</strong> 0
<strong>Explanation:</strong> Maximum non-negative product is shown (1 * 0 * -4 = 0).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 15</code></li>
	<li><code>-4 &lt;= grid[i][j] &lt;= 4</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi chỉ có thể sang phải hoặc xuống dưới; ta cần tích không âm lớn nhất. Vì có số âm, tích tốt nhất có thể đến từ tích âm nhỏ nhất nhân với một số âm khác. Chỉ lưu giá trị lớn nhất sẽ bỏ sót đường đi đó.
>
> Lưu cả tích nhỏ nhất và lớn nhất khi đến mỗi ô, cập nhật từ hai giá trị cực trị ở phía trên và bên trái. Nếu giá trị lớn nhất cuối cùng âm thì trả về $-1$; nếu không, lấy modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa mảng 3 chiều $f$, trong đó $f[i][j][0]$ và $f[i][j][1]$ lần lượt là tích nhỏ nhất và lớn nhất của mọi đường đi từ góc trên bên trái $(0, 0)$ đến vị trí $(i, j)$. Với mỗi vị trí $(i, j)$, ta có thể chuyển từ phía trên $(i - 1, j)$ hoặc bên trái $(i, j - 1)$, nên cần xét kết quả nhân các tích nhỏ nhất và lớn nhất của hai vị trí này với giá trị ô hiện tại.

Cuối cùng, ta trả về $f[m - 1][n - 1][1]$ theo modulo $10^9 + 7$. Nếu $f[m - 1][n - 1][1]$ nhỏ hơn $0$, trả về $-1$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProductPath(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        f = [[[0, 0] for _ in range(n)] for _ in range(m)]

        for i in range(m):
            for j in range(n):
                x = grid[i][j]
                if i == 0 and j == 0:
                    f[i][j][0] = x
                    f[i][j][1] = x
                    continue

                mn, mx = inf, -inf

                if i > 0:
                    a, b = f[i - 1][j]
                    mn = min(mn, a * x, b * x)
                    mx = max(mx, a * x, b * x)

                if j > 0:
                    a, b = f[i][j - 1]
                    mn = min(mn, a * x, b * x)
                    mx = max(mx, a * x, b * x)

                f[i][j][0], f[i][j][1] = mn, mx

        ans = f[m - 1][n - 1][1]
        mod = 10**9 + 7
        return -1 if ans < 0 else ans % mod
```

#### Java

```java
class Solution {
    public int maxProductPath(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        long[][][] f = new long[m][n][2];

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                long x = grid[i][j];

                if (i == 0 && j == 0) {
                    f[i][j][0] = x;
                    f[i][j][1] = x;
                    continue;
                }

                long mn = Long.MAX_VALUE, mx = Long.MIN_VALUE;

                if (i > 0) {
                    long a = f[i - 1][j][0], b = f[i - 1][j][1];
                    mn = Math.min(mn, Math.min(a * x, b * x));
                    mx = Math.max(mx, Math.max(a * x, b * x));
                }

                if (j > 0) {
                    long a = f[i][j - 1][0], b = f[i][j - 1][1];
                    mn = Math.min(mn, Math.min(a * x, b * x));
                    mx = Math.max(mx, Math.max(a * x, b * x));
                }

                f[i][j][0] = mn;
                f[i][j][1] = mx;
            }
        }

        long ans = f[m - 1][n - 1][1];
        int mod = (int) 1e9 + 7;
        return ans < 0 ? -1 : (int) (ans % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProductPath(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<array<long long, 2>>> f(m, vector<array<long long, 2>>(n));

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                long long x = grid[i][j];
                if (i == 0 && j == 0) {
                    f[i][j] = {x, x};
                    continue;
                }

                long long mn = LLONG_MAX, mx = LLONG_MIN;

                if (i > 0) {
                    auto [a, b] = f[i - 1][j];
                    mn = min(mn, min(a * x, b * x));
                    mx = max(mx, max(a * x, b * x));
                }

                if (j > 0) {
                    auto [a, b] = f[i][j - 1];
                    mn = min(mn, min(a * x, b * x));
                    mx = max(mx, max(a * x, b * x));
                }

                f[i][j] = {mn, mx};
            }
        }

        long long ans = f[m - 1][n - 1][1];
        const int mod = 1e9 + 7;
        return ans < 0 ? -1 : ans % mod;
    }
};
```

#### Go

```go
func maxProductPath(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	f := make([][][2]int64, m)
	for i := range f {
		f[i] = make([][2]int64, n)
	}

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			x := int64(grid[i][j])
			if i == 0 && j == 0 {
				f[i][j] = [2]int64{x, x}
				continue
			}

			mn, mx := int64(1<<63-1), int64(-1<<63)

			if i > 0 {
				a, b := f[i-1][j][0], f[i-1][j][1]
				mn = min(mn, min(a*x, b*x))
				mx = max(mx, max(a*x, b*x))
			}

			if j > 0 {
				a, b := f[i][j-1][0], f[i][j-1][1]
				mn = min(mn, min(a*x, b*x))
				mx = max(mx, max(a*x, b*x))
			}

			f[i][j] = [2]int64{mn, mx}
		}
	}

	ans := f[m-1][n-1][1]
	mod := int64(1e9 + 7)
	if ans < 0 {
		return -1
	}
	return int(ans % mod)
}
```

#### TypeScript

```ts
function maxProductPath(grid: number[][]): number {
    const m = grid.length,
        n = grid[0].length;
    const f = Array.from({ length: m }, () => Array.from({ length: n }, () => [0, 0]));

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            const x = grid[i][j];

            if (i === 0 && j === 0) {
                f[i][j] = [x, x];
                continue;
            }

            let mn = Infinity,
                mx = -Infinity;

            if (i > 0) {
                const [a, b] = f[i - 1][j];
                mn = Math.min(mn, a * x, b * x);
                mx = Math.max(mx, a * x, b * x);
            }

            if (j > 0) {
                const [a, b] = f[i][j - 1];
                mn = Math.min(mn, a * x, b * x);
                mx = Math.max(mx, a * x, b * x);
            }

            f[i][j] = [mn, mx];
        }
    }

    const ans = f[m - 1][n - 1][1];
    const mod = 1e9 + 7;
    return ans < 0 ? -1 : ans % mod;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_product_path(grid: Vec<Vec<i32>>) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let mut f = vec![vec![[0i64; 2]; n]; m];

        for i in 0..m {
            for j in 0..n {
                let x = grid[i][j] as i64;

                if i == 0 && j == 0 {
                    f[i][j] = [x, x];
                    continue;
                }

                let mut mn = i64::MAX;
                let mut mx = i64::MIN;

                if i > 0 {
                    let [a, b] = f[i - 1][j];
                    mn = mn.min(a * x).min(b * x);
                    mx = mx.max(a * x).max(b * x);
                }

                if j > 0 {
                    let [a, b] = f[i][j - 1];
                    mn = mn.min(a * x).min(b * x);
                    mx = mx.max(a * x).max(b * x);
                }

                f[i][j] = [mn, mx];
            }
        }

        let ans = f[m - 1][n - 1][1];
        let mod_val = 1_000_000_007i64;
        if ans < 0 {
            -1
        } else {
            (ans % mod_val) as i32
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
