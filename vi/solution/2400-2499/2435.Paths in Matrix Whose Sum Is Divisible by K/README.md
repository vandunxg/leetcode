---
comments: true
difficulty: Hard
rating: 1951
source: Weekly Contest 314 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [2435. Paths in Matrix Whose Sum Is Divisible by K](https://leetcode.com/problems/paths-in-matrix-whose-sum-is-divisible-by-k)

[中文文档](/solution/2400-2499/2435.Paths%20in%20Matrix%20Whose%20Sum%20Is%20Divisible%20by%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên <code>grid</code> kích thước <code>m x n</code>, được đánh chỉ số từ <strong>0</strong>, và một số nguyên <code>k</code>. Hiện tại bạn đang ở vị trí <code>(0, 0)</code> và muốn đến vị trí <code>(m - 1, n - 1)</code>, chỉ được di chuyển <strong>xuống</strong> hoặc <strong>sang phải</strong>.</p>

<p>Hãy trả về <em>số lượng đường đi mà tổng các phần tử trên đường đi chia hết cho </em><code>k</code>. Vì đáp án có thể rất lớn, hãy trả về <strong>phần dư</strong> khi chia cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2435.Paths%20in%20Matrix%20Whose%20Sum%20Is%20Divisible%20by%20K/images/image-20220813183124-1.png" style="width: 437px; height: 200px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[5,2,4],[3,0,5],[0,7,2]], k = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có hai đường đi mà tổng các phần tử chia hết cho k.
Đường đi đầu tiên được tô đỏ có tổng 5 + 2 + 4 + 5 + 2 = 18, chia hết cho 3.
Đường đi thứ hai được tô xanh có tổng 5 + 3 + 0 + 5 + 2 = 15, chia hết cho 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2435.Paths%20in%20Matrix%20Whose%20Sum%20Is%20Divisible%20by%20K/images/image-20220817112930-3.png" style="height: 85px; width: 132px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,0]], k = 5
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Đường đi được tô đỏ có tổng 0 + 0 = 0, chia hết cho 5.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2435.Paths%20in%20Matrix%20Whose%20Sum%20Is%20Divisible%20by%20K/images/image-20220812224605-3.png" style="width: 257px; height: 200px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[7,3,4,9],[2,3,6,2],[2,3,7,0]], k = 1
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Mọi số nguyên đều chia hết cho 1, nên tổng các phần tử trên mọi đường đi đều chia hết cho k.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ được di chuyển sang phải hoặc xuống dưới. Ma trận có $mn\le 5\times 10^4$ và $k\le 50$. Ta cần đếm số đường đi có tổng đồng dư $0$ modulo $k$, nên phần dư phải là một phần của trạng thái.
>
> Gọi $f[i][j][r]$ là số cách đến $(i,j)$ với tổng có phần dư $r\bmod K$, đi từ ô phía trên hoặc bên trái với phần dư $r-grid[i][j]$. Khởi tạo ô bắt đầu với giá trị $1$.

<!-- thinking:end -->

Ta ký hiệu $k$ trong đề bài là $K$, đồng thời ký hiệu $m$ và $n$ lần lượt là số hàng và số cột của ma trận $\textit{grid}$.

Định nghĩa $f[i][j][k]$ là số đường đi bắt đầu từ $(0, 0)$, đến vị trí $(i, j)$, sao cho tổng các phần tử trên đường đi modulo $K$ bằng $k$. Ban đầu, $f[0][0][\textit{grid}[0][0] \bmod K] = 1$. Đáp án cuối cùng là $f[m - 1][n - 1][0]$.

Ta có thể suy ra công thức chuyển trạng thái:

$$
f[i][j][k] = f[i - 1][j][(k - \textit{grid}[i][j])\bmod K] + f[i][j - 1][(k - \textit{grid}[i][j])\bmod K]
$$

Để tránh vấn đề với phép modulo âm, ta có thể thay $(k - \textit{grid}[i][j])\bmod K$ trong công thức trên bằng $((k - \textit{grid}[i][j] \bmod K) + K) \bmod K$.

Đáp án là $f[m - 1][n - 1][0]$.

Độ phức tạp thời gian là $O(m \times n \times K)$ và độ phức tạp không gian là $O(m \times n \times K)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận $\textit{grid}$, còn $K$ là số nguyên $k$ trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPaths(self, grid: List[List[int]], K: int) -> int:
        mod = 10**9 + 7
        m, n = len(grid), len(grid[0])
        f = [[[0] * K for _ in range(n)] for _ in range(m)]
        f[0][0][grid[0][0] % K] = 1
        for i in range(m):
            for j in range(n):
                for k in range(K):
                    k0 = ((k - grid[i][j] % K) + K) % K
                    if i:
                        f[i][j][k] += f[i - 1][j][k0]
                    if j:
                        f[i][j][k] += f[i][j - 1][k0]
                    f[i][j][k] %= mod
        return f[m - 1][n - 1][0]
```

#### Java

```java
class Solution {
    public int numberOfPaths(int[][] grid, int K) {
        final int mod = (int) 1e9 + 7;
        int m = grid.length, n = grid[0].length;
        int[][][] f = new int[m][n][K];
        f[0][0][grid[0][0] % K] = 1;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < K; ++k) {
                    int k0 = ((k - grid[i][j] % K) + K) % K;
                    if (i > 0) {
                        f[i][j][k] = (f[i][j][k] + f[i - 1][j][k0]) % mod;
                    }
                    if (j > 0) {
                        f[i][j][k] = (f[i][j][k] + f[i][j - 1][k0]) % mod;
                    }
                }
            }
        }
        return f[m - 1][n - 1][0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfPaths(vector<vector<int>>& grid, int K) {
        const int mod = 1e9 + 7;
        int m = grid.size(), n = grid[0].size();
        int f[m][n][K];
        memset(f, 0, sizeof(f));
        f[0][0][grid[0][0] % K] = 1;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < K; ++k) {
                    int k0 = ((k - grid[i][j] % K) + K) % K;
                    if (i > 0) {
                        f[i][j][k] = (f[i][j][k] + f[i - 1][j][k0]) % mod;
                    }
                    if (j > 0) {
                        f[i][j][k] = (f[i][j][k] + f[i][j - 1][k0]) % mod;
                    }
                }
            }
        }
        return f[m - 1][n - 1][0];
    }
};
```

#### Go

```go
func numberOfPaths(grid [][]int, K int) int {
	const mod = 1e9 + 7
	m, n := len(grid), len(grid[0])
	f := make([][][]int, m)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, K)
		}
	}
	f[0][0][grid[0][0]%K] = 1
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			for k := 0; k < K; k++ {
				k0 := ((k - grid[i][j]%K) + K) % K
				if i > 0 {
					f[i][j][k] = (f[i][j][k] + f[i-1][j][k0]) % mod
				}
				if j > 0 {
					f[i][j][k] = (f[i][j][k] + f[i][j-1][k0]) % mod
				}
			}
		}
	}
	return f[m-1][n-1][0]
}
```

#### TypeScript

```ts
function numberOfPaths(grid: number[][], K: number): number {
    const mod = 1e9 + 7;
    const m = grid.length;
    const n = grid[0].length;
    const f: number[][][] = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Array(K).fill(0)),
    );
    f[0][0][grid[0][0] % K] = 1;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            for (let k = 0; k < K; ++k) {
                const k0 = (k - (grid[i][j] % K) + K) % K;
                if (i > 0) {
                    f[i][j][k] = (f[i][j][k] + f[i - 1][j][k0]) % mod;
                }
                if (j > 0) {
                    f[i][j][k] = (f[i][j][k] + f[i][j - 1][k0]) % mod;
                }
            }
        }
    }
    return f[m - 1][n - 1][0];
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_paths(grid: Vec<Vec<i32>>, K: i32) -> i32 {
        const MOD: i32 = 1_000_000_007;
        let m = grid.len();
        let n = grid[0].len();
        let K = K as usize;
        let mut f = vec![vec![vec![0; K]; n]; m];
        f[0][0][grid[0][0] as usize % K] = 1;
        for i in 0..m {
            for j in 0..n {
                for k in 0..K {
                    let k0 = ((k + K - grid[i][j] as usize % K) % K) as usize;
                    if i > 0 {
                        f[i][j][k] = (f[i][j][k] + f[i - 1][j][k0]) % MOD;
                    }
                    if j > 0 {
                        f[i][j][k] = (f[i][j][k] + f[i][j - 1][k0]) % MOD;
                    }
                }
            }
        }
        f[m - 1][n - 1][0]
    }
}
```

#### C#

```cs
public class Solution {
    public int NumberOfPaths(int[][] grid, int k) {
        const int mod = (int) 1e9 + 7;
        int m = grid.Length, n = grid[0].Length;
        int[,,] f = new int[m, n, k];
        f[0, 0, grid[0][0] % k] = 1;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int p = 0; p < k; ++p) {
                    int k0 = ((p - grid[i][j] % k) + k) % k;
                    if (i > 0) {
                        f[i, j, p] = (f[i, j, p] + f[i - 1, j, k0]) % mod;
                    }
                    if (j > 0) {
                        f[i, j, p] = (f[i, j, p] + f[i, j - 1, k0]) % mod;
                    }
                }
            }
        }
        return f[m - 1, n - 1, 0];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
