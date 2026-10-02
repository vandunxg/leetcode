---
comments: true
difficulty: Medium
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [688. Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard)

[中文文档](/solution/0600-0699/0688.Knight%20Probability%20in%20Chessboard/README.md)

## Mô tả

<!-- description:start -->

<p>Trên bàn cờ <code>n x n</code>, một quân mã bắt đầu ở ô <code>(row, column)</code> và cố gắng đi đúng <code>k</code> nước. Hàng và cột được <strong>đánh chỉ số từ 0</strong>, nên ô trên cùng bên trái là <code>(0, 0)</code>, còn ô dưới cùng bên phải là <code>(n - 1, n - 1)</code>.</p>

<p>Quân mã có thể đi theo tám hướng như minh họa bên dưới. Mỗi nước đi gồm hai ô theo một hướng chính, rồi một ô theo hướng vuông góc.</p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0688.Knight%20Probability%20in%20Chessboard/images/knight.png" style="width: 300px; height: 300px;" />
<p>Mỗi lần đến lượt đi, quân mã chọn ngẫu nhiên đồng xác suất một trong tám nước đi có thể (kể cả nước đi ra ngoài bàn cờ) rồi di chuyển đến đó.</p>

<p>Quân mã tiếp tục di chuyển cho đến khi đi đủ <code>k</code> nước hoặc đi ra ngoài bàn cờ.</p>

<p>Hãy trả về <em>xác suất quân mã vẫn còn trên bàn cờ sau khi dừng di chuyển</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, k = 2, row = 0, column = 0
<strong>Đầu ra:</strong> 0.06250
<strong>Giải thích:</strong> Có hai nước đi (đến (1,2), (2,1)) giúp quân mã vẫn ở trên bàn cờ.
Từ mỗi vị trí đó, cũng có hai nước đi giúp quân mã vẫn ở trên bàn cờ.
Xác suất tổng cộng để quân mã ở lại trên bàn cờ là 0.0625.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, k = 0, row = 0, column = 0
<strong>Đầu ra:</strong> 1.00000
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 25</code></li>
	<li><code>0 &lt;= k &lt;= 100</code></li>
	<li><code>0 &lt;= row, column &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Cần tính xác suất quân mã còn trên bàn cờ $n\times n$ sau $k$ nước đi. Không thể duyệt hết $8^k$ khả năng khi $k\le 100$.
>
> $f[h][i][j]$ là xác suất quân mã vẫn ở trên bàn cờ sau $h$ nước đi nữa, nếu bắt đầu tại $(i,j)$. Trường hợp cơ sở $h=0$ có xác suất $1$; nếu không, lấy trung bình $f[h-1]$ của tám vị trí đến nằm trong bàn cờ.

<!-- thinking:end -->

Ta định nghĩa $f[h][i][j]$ là xác suất quân mã vẫn ở trên bàn cờ sau khi đi $h$ nước, bắt đầu từ vị trí $(i, j)$. Đáp án cuối cùng là $f[k][\textit{row}][\textit{column}]$.

Khi $h=0$, quân mã chắc chắn vẫn ở trên bàn cờ, nên xác suất là $1$, tức $f[0][i][j]=1$.

Khi $h \gt 0$, xác suất quân mã ở vị trí $(i, j)$ được tính từ xác suất của $8$ vị trí có thể đi đến đó ở bước trước, cụ thể là

$$
f[h][i][j] = \sum_{x, y} f[h - 1][x][y] \times \frac{1}{8}
$$

trong đó $(x, y)$ là một trong $8$ vị trí mà quân mã có thể đi tới từ $(i, j)$.

Đáp án cuối cùng là $f[k][\textit{row}][\textit{column}]$.

Độ phức tạp thời gian là $O(k \times n^2)$ và độ phức tạp không gian là $O(k \times n^2)$. Ở đây, $k$ là số nước đi được cho và $n$ là kích thước bàn cờ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def knightProbability(self, n: int, k: int, row: int, column: int) -> float:
        f = [[[0] * n for _ in range(n)] for _ in range(k + 1)]
        for i in range(n):
            for j in range(n):
                f[0][i][j] = 1
        for h in range(1, k + 1):
            for i in range(n):
                for j in range(n):
                    for a, b in pairwise((-2, -1, 2, 1, -2, 1, 2, -1, -2)):
                        x, y = i + a, j + b
                        if 0 <= x < n and 0 <= y < n:
                            f[h][i][j] += f[h - 1][x][y] / 8
        return f[k][row][column]
```

#### Java

```java
class Solution {
    public double knightProbability(int n, int k, int row, int column) {
        double[][][] f = new double[k + 1][n][n];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                f[0][i][j] = 1;
            }
        }
        int[] dirs = {-2, -1, 2, 1, -2, 1, 2, -1, -2};
        for (int h = 1; h <= k; ++h) {
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    for (int p = 0; p < 8; ++p) {
                        int x = i + dirs[p], y = j + dirs[p + 1];
                        if (x >= 0 && x < n && y >= 0 && y < n) {
                            f[h][i][j] += f[h - 1][x][y] / 8;
                        }
                    }
                }
            }
        }
        return f[k][row][column];
    }
}
```

#### C++

```cpp
class Solution {
public:
    double knightProbability(int n, int k, int row, int column) {
        double f[k + 1][n][n];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                f[0][i][j] = 1;
            }
        }
        int dirs[9] = {-2, -1, 2, 1, -2, 1, 2, -1, -2};
        for (int h = 1; h <= k; ++h) {
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    for (int p = 0; p < 8; ++p) {
                        int x = i + dirs[p], y = j + dirs[p + 1];
                        if (x >= 0 && x < n && y >= 0 && y < n) {
                            f[h][i][j] += f[h - 1][x][y] / 8;
                        }
                    }
                }
            }
        }
        return f[k][row][column];
    }
};
```

#### Go

```go
func knightProbability(n int, k int, row int, column int) float64 {
	f := make([][][]float64, k+1)
	for h := range f {
		f[h] = make([][]float64, n)
		for i := range f[h] {
			f[h][i] = make([]float64, n)
			for j := range f[h][i] {
				f[0][i][j] = 1
			}
		}
	}
	dirs := [9]int{-2, -1, 2, 1, -2, 1, 2, -1, -2}
	for h := 1; h <= k; h++ {
		for i := 0; i < n; i++ {
			for j := 0; j < n; j++ {
				for p := 0; p < 8; p++ {
					x, y := i+dirs[p], j+dirs[p+1]
					if x >= 0 && x < n && y >= 0 && y < n {
						f[h][i][j] += f[h-1][x][y] / 8
					}
				}
			}
		}
	}
	return f[k][row][column]
}
```

#### TypeScript

```ts
function knightProbability(n: number, k: number, row: number, column: number): number {
    const f = Array.from({ length: k + 1 }, () =>
        Array.from({ length: n }, () => Array(n).fill(0)),
    );
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            f[0][i][j] = 1;
        }
    }
    const dirs = [-2, -1, 2, 1, -2, 1, 2, -1, -2];
    for (let h = 1; h <= k; ++h) {
        for (let i = 0; i < n; ++i) {
            for (let j = 0; j < n; ++j) {
                for (let p = 0; p < 8; ++p) {
                    const x = i + dirs[p];
                    const y = j + dirs[p + 1];
                    if (x >= 0 && x < n && y >= 0 && y < n) {
                        f[h][i][j] += f[h - 1][x][y] / 8;
                    }
                }
            }
        }
    }
    return f[k][row][column];
}
```

#### Rust

```rust
impl Solution {
    pub fn knight_probability(n: i32, k: i32, row: i32, column: i32) -> f64 {
        let n = n as usize;
        let k = k as usize;

        let mut f = vec![vec![vec![0.0; n]; n]; k + 1];

        for i in 0..n {
            for j in 0..n {
                f[0][i][j] = 1.0;
            }
        }

        let dirs = [-2, -1, 2, 1, -2, 1, 2, -1, -2];

        for h in 1..=k {
            for i in 0..n {
                for j in 0..n {
                    for p in 0..8 {
                        let x = i as isize + dirs[p];
                        let y = j as isize + dirs[p + 1];

                        if x >= 0 && x < n as isize && y >= 0 && y < n as isize {
                            let x = x as usize;
                            let y = y as usize;
                            f[h][i][j] += f[h - 1][x][y] / 8.0;
                        }
                    }
                }
            }
        }

        f[k][row as usize][column as usize]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
