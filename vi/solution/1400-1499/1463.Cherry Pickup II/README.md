---
comments: true
difficulty: Hard
rating: 1956
source: Biweekly Contest 27 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [1463. Cherry Pickup II](https://leetcode.com/problems/cherry-pickup-ii)

[中文文档](/solution/1400-1499/1463.Cherry%20Pickup%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <code>rows x cols</code> <code>grid</code> biểu diễn một cánh đồng anh đào, trong đó <code>grid[i][j]</code> là số quả anh đào có thể thu thập tại ô <code>(i, j)</code>.</p>

<p>Bạn có hai robot có thể thu thập anh đào:</p>

<ul>
	<li><strong>Robot #1</strong> ở <strong>góc trên bên trái</strong> <code>(0, 0)</code>, và</li>
	<li><strong>Robot #2</strong> ở <strong>góc trên bên phải</strong> <code>(0, cols - 1)</code>.</li>
</ul>

<p>Trả về <em>số quả anh đào lớn nhất có thể thu thập bằng cả hai robot theo các quy tắc sau</em>:</p>

<ul>
	<li>Từ ô <code>(i, j)</code>, robot có thể di chuyển đến ô <code>(i + 1, j - 1)</code>, <code>(i + 1, j)</code> hoặc <code>(i + 1, j + 1)</code>.</li>
	<li>Khi một robot đi qua một ô, robot sẽ nhặt toàn bộ số quả anh đào và ô đó trở thành ô trống.</li>
	<li>Khi cả hai robot ở cùng một ô, chỉ một robot nhặt số quả anh đào tại đó.</li>
	<li>Cả hai robot không được đi ra ngoài <code>grid</code> tại bất kỳ thời điểm nào.</li>
	<li>Cả hai robot phải đi đến hàng cuối cùng trong <code>grid</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1463.Cherry%20Pickup%20II/images/sample_1_1802.png" style="width: 374px; height: 501px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[3,1,1],[2,5,1],[1,5,5],[2,1,1]]
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong> Đường đi của robot #1 và #2 lần lượt được biểu diễn bằng màu xanh lá và xanh dương.
Số quả anh đào robot #1 thu được, (3 + 2 + 5 + 2) = 12.
Số quả anh đào robot #2 thu được, (1 + 5 + 5 + 1) = 12.
Tổng số quả anh đào: 12 + 12 = 24.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1463.Cherry%20Pickup%20II/images/sample_2_1802.png" style="width: 500px; height: 452px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,0,0,0,1],[2,0,0,0,0,3,0],[2,0,9,0,0,0,0],[0,3,0,5,4,0,0],[1,0,2,3,0,0,6]]
<strong>Đầu ra:</strong> 28
<strong>Giải thích:</strong> Đường đi của robot #1 và #2 lần lượt được biểu diễn bằng màu xanh lá và xanh dương.
Số quả anh đào robot #1 thu được, (1 + 9 + 5 + 2) = 17.
Số quả anh đào robot #2 thu được, (1 + 3 + 4 + 3) = 11.
Tổng số quả anh đào: 17 + 11 = 28.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>rows == grid.length</code></li>
	<li><code>cols == grid[i].length</code></li>
	<li><code>2 &lt;= rows, cols &lt;= 70</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Hai robot cùng đi xuống từng hàng, mỗi robot có thể đi sang trái, đứng yên hoặc sang phải. $rows,cols\le 70$. Sau khi đồng bộ theo hàng, trạng thái là hàng và hai cột.
>
> $f[i][j_1][j_2]$ là điểm tốt nhất khi cả hai robot đang ở hàng $i$. Một ô chung chỉ được tính một lần. Chuyển trạng thái từ chín cặp cột lân cận. Lấy giá trị lớn nhất ở hàng cuối.

<!-- thinking:end -->

Ta định nghĩa $f[i][j_1][j_2]$ là số quả anh đào lớn nhất có thể thu thập khi hai robot ở vị trí $j_1$ và $j_2$ trên hàng thứ $i$. Ban đầu, $f[0][0][n-1] = grid[0][0] + grid[0][n-1]$, các giá trị còn lại là $-1$. Đáp án là $\max_{0 \leq j_1, j_2 < n} f[m-1][j_1][j_2]$.

Xét $f[i][j_1][j_2]$. Nếu $j_1 \neq j_2$, số quả anh đào mà các robot có thể thu thập ở hàng thứ $i$ là $grid[i][j_1] + grid[i][j_2]$. Nếu $j_1 = j_2$, số quả anh đào mà các robot có thể thu thập ở hàng thứ $i$ là $grid[i][j_1]$. Ta có thể liệt kê trạng thái trước đó của hai robot $f[i-1][y1][y2]$, trong đó $y_1, y_2$ là vị trí của hai robot ở hàng thứ $(i-1)$, khi đó $y_1 \in \{j_1-1, j_1, j_1+1\}$ và $y_2 \in \{j_2-1, j_2, j_2+1\}$. Công thức chuyển trạng thái như sau:

$$
f[i][j_1][j_2] = \max_{y_1 \in \{j_1-1, j_1, j_1+1\}, y_2 \in \{j_2-1, j_2, j_2+1\}} f[i-1][y_1][y_2] + \begin{cases} grid[i][j_1] + grid[i][j_2], & j_1 \neq j_2 \\ grid[i][j_1], & j_1 = j_2 \end{cases}
$$

Trong đó bỏ qua $f[i-1][y_1][y_2]$ nếu giá trị này là $-1$.

Đáp án cuối cùng là $\max_{0 \leq j_1, j_2 < n} f[m-1][j_1][j_2]$.

Độ phức tạp thời gian là $O(m \times n^2)$ và độ phức tạp không gian là $O(m \times n^2)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của `grid`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def cherryPickup(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        f = [[[-1] * n for _ in range(n)] for _ in range(m)]
        f[0][0][n - 1] = grid[0][0] + grid[0][n - 1]
        for i in range(1, m):
            for j1 in range(n):
                for j2 in range(n):
                    x = grid[i][j1] + (0 if j1 == j2 else grid[i][j2])
                    for y1 in range(j1 - 1, j1 + 2):
                        for y2 in range(j2 - 1, j2 + 2):
                            if 0 <= y1 < n and 0 <= y2 < n and f[i - 1][y1][y2] != -1:
                                f[i][j1][j2] = max(f[i][j1][j2], f[i - 1][y1][y2] + x)
        return max(f[-1][j1][j2] for j1, j2 in product(range(n), range(n)))
```

#### Java

```java
class Solution {
    public int cherryPickup(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][][] f = new int[m][n][n];
        for (var g : f) {
            for (var h : g) {
                Arrays.fill(h, -1);
            }
        }
        f[0][0][n - 1] = grid[0][0] + grid[0][n - 1];
        for (int i = 1; i < m; ++i) {
            for (int j1 = 0; j1 < n; ++j1) {
                for (int j2 = 0; j2 < n; ++j2) {
                    int x = grid[i][j1] + (j1 == j2 ? 0 : grid[i][j2]);
                    for (int y1 = j1 - 1; y1 <= j1 + 1; ++y1) {
                        for (int y2 = j2 - 1; y2 <= j2 + 1; ++y2) {
                            if (y1 >= 0 && y1 < n && y2 >= 0 && y2 < n && f[i - 1][y1][y2] != -1) {
                                f[i][j1][j2] = Math.max(f[i][j1][j2], f[i - 1][y1][y2] + x);
                            }
                        }
                    }
                }
            }
        }
        int ans = 0;
        for (int j1 = 0; j1 < n; ++j1) {
            for (int j2 = 0; j2 < n; ++j2) {
                ans = Math.max(ans, f[m - 1][j1][j2]);
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
    int cherryPickup(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int f[m][n][n];
        memset(f, -1, sizeof(f));
        f[0][0][n - 1] = grid[0][0] + grid[0][n - 1];
        for (int i = 1; i < m; ++i) {
            for (int j1 = 0; j1 < n; ++j1) {
                for (int j2 = 0; j2 < n; ++j2) {
                    int x = grid[i][j1] + (j1 == j2 ? 0 : grid[i][j2]);
                    for (int y1 = j1 - 1; y1 <= j1 + 1; ++y1) {
                        for (int y2 = j2 - 1; y2 <= j2 + 1; ++y2) {
                            if (y1 >= 0 && y1 < n && y2 >= 0 && y2 < n && f[i - 1][y1][y2] != -1) {
                                f[i][j1][j2] = max(f[i][j1][j2], f[i - 1][y1][y2] + x);
                            }
                        }
                    }
                }
            }
        }
        int ans = 0;
        for (int j1 = 0; j1 < n; ++j1) {
            for (int j2 = 0; j2 < n; ++j2) {
                ans = max(ans, f[m - 1][j1][j2]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func cherryPickup(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	f := make([][][]int, m)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, n)
			for k := range f[i][j] {
				f[i][j][k] = -1
			}
		}
	}
	f[0][0][n-1] = grid[0][0] + grid[0][n-1]
	for i := 1; i < m; i++ {
		for j1 := 0; j1 < n; j1++ {
			for j2 := 0; j2 < n; j2++ {
				x := grid[i][j1]
				if j1 != j2 {
					x += grid[i][j2]
				}
				for y1 := j1 - 1; y1 <= j1+1; y1++ {
					for y2 := j2 - 1; y2 <= j2+1; y2++ {
						if y1 >= 0 && y1 < n && y2 >= 0 && y2 < n && f[i-1][y1][y2] != -1 {
							f[i][j1][j2] = max(f[i][j1][j2], f[i-1][y1][y2]+x)
						}
					}
				}
			}
		}
	}
	for j1 := 0; j1 < n; j1++ {
		ans = max(ans, slices.Max(f[m-1][j1]))
	}
	return
}
```

#### TypeScript

```ts
function cherryPickup(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const f = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Array.from({ length: n }, () => -1)),
    );
    f[0][0][n - 1] = grid[0][0] + grid[0][n - 1];
    for (let i = 1; i < m; ++i) {
        for (let j1 = 0; j1 < n; ++j1) {
            for (let j2 = 0; j2 < n; ++j2) {
                const x = grid[i][j1] + (j1 === j2 ? 0 : grid[i][j2]);
                for (let y1 = j1 - 1; y1 <= j1 + 1; ++y1) {
                    for (let y2 = j2 - 1; y2 <= j2 + 1; ++y2) {
                        if (y1 >= 0 && y1 < n && y2 >= 0 && y2 < n && f[i - 1][y1][y2] !== -1) {
                            f[i][j1][j2] = Math.max(f[i][j1][j2], f[i - 1][y1][y2] + x);
                        }
                    }
                }
            }
        }
    }
    return Math.max(...f[m - 1].flat());
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Lớp $i$ của phương pháp 1 chỉ phụ thuộc vào lớp $i-1$. Dùng hai bảng $n\times n$ luân phiên sẽ giảm một chiều mà không thay đổi các phép chuyển trạng thái.

<!-- thinking:end -->

Nhận thấy phép tính $f[i][j_1][j_2]$ chỉ liên quan đến $f[i-1][y_1][y_2]$. Vì vậy, ta có thể dùng mảng luân phiên để tối ưu độ phức tạp không gian. Sau khi tối ưu độ phức tạp không gian, độ phức tạp thời gian là $O(n^2)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def cherryPickup(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        f = [[-1] * n for _ in range(n)]
        g = [[-1] * n for _ in range(n)]
        f[0][n - 1] = grid[0][0] + grid[0][n - 1]
        for i in range(1, m):
            for j1 in range(n):
                for j2 in range(n):
                    x = grid[i][j1] + (0 if j1 == j2 else grid[i][j2])
                    for y1 in range(j1 - 1, j1 + 2):
                        for y2 in range(j2 - 1, j2 + 2):
                            if 0 <= y1 < n and 0 <= y2 < n and f[y1][y2] != -1:
                                g[j1][j2] = max(g[j1][j2], f[y1][y2] + x)
            f, g = g, f
        return max(f[j1][j2] for j1, j2 in product(range(n), range(n)))
```

#### Java

```java
class Solution {
    public int cherryPickup(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] f = new int[n][n];
        int[][] g = new int[n][n];
        for (int i = 0; i < n; ++i) {
            Arrays.fill(f[i], -1);
            Arrays.fill(g[i], -1);
        }
        f[0][n - 1] = grid[0][0] + grid[0][n - 1];
        for (int i = 1; i < m; ++i) {
            for (int j1 = 0; j1 < n; ++j1) {
                for (int j2 = 0; j2 < n; ++j2) {
                    int x = grid[i][j1] + (j1 == j2 ? 0 : grid[i][j2]);
                    for (int y1 = j1 - 1; y1 <= j1 + 1; ++y1) {
                        for (int y2 = j2 - 1; y2 <= j2 + 1; ++y2) {
                            if (y1 >= 0 && y1 < n && y2 >= 0 && y2 < n && f[y1][y2] != -1) {
                                g[j1][j2] = Math.max(g[j1][j2], f[y1][y2] + x);
                            }
                        }
                    }
                }
            }
            int[][] t = f;
            f = g;
            g = t;
        }
        int ans = 0;
        for (int j1 = 0; j1 < n; ++j1) {
            for (int j2 = 0; j2 < n; ++j2) {
                ans = Math.max(ans, f[j1][j2]);
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
    int cherryPickup(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> f(n, vector<int>(n, -1));
        vector<vector<int>> g(n, vector<int>(n, -1));
        f[0][n - 1] = grid[0][0] + grid[0][n - 1];
        for (int i = 1; i < m; ++i) {
            for (int j1 = 0; j1 < n; ++j1) {
                for (int j2 = 0; j2 < n; ++j2) {
                    int x = grid[i][j1] + (j1 == j2 ? 0 : grid[i][j2]);
                    for (int y1 = j1 - 1; y1 <= j1 + 1; ++y1) {
                        for (int y2 = j2 - 1; y2 <= j2 + 1; ++y2) {
                            if (y1 >= 0 && y1 < n && y2 >= 0 && y2 < n && f[y1][y2] != -1) {
                                g[j1][j2] = max(g[j1][j2], f[y1][y2] + x);
                            }
                        }
                    }
                }
            }
            swap(f, g);
        }
        int ans = 0;
        for (int j1 = 0; j1 < n; ++j1) {
            for (int j2 = 0; j2 < n; ++j2) {
                ans = max(ans, f[j1][j2]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func cherryPickup(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	f := make([][]int, n)
	g := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
		g[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -1
			g[i][j] = -1
		}
	}
	f[0][n-1] = grid[0][0] + grid[0][n-1]
	for i := 1; i < m; i++ {
		for j1 := 0; j1 < n; j1++ {
			for j2 := 0; j2 < n; j2++ {
				x := grid[i][j1]
				if j1 != j2 {
					x += grid[i][j2]
				}
				for y1 := j1 - 1; y1 <= j1+1; y1++ {
					for y2 := j2 - 1; y2 <= j2+1; y2++ {
						if y1 >= 0 && y1 < n && y2 >= 0 && y2 < n && f[y1][y2] != -1 {
							g[j1][j2] = max(g[j1][j2], f[y1][y2]+x)
						}
					}
				}
			}
		}
	}
	f, g = g, f
	for j1 := 0; j1 < n; j1++ {
		ans = max(ans, slices.Max(f[j1]))
	}
	return
}
```

#### TypeScript

```ts
function cherryPickup(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    let f: number[][] = Array.from({ length: n }, () => Array.from({ length: n }, () => -1));
    let g: number[][] = Array.from({ length: n }, () => Array.from({ length: n }, () => -1));
    f[0][n - 1] = grid[0][0] + grid[0][n - 1];
    for (let i = 1; i < m; ++i) {
        for (let j1 = 0; j1 < n; ++j1) {
            for (let j2 = 0; j2 < n; ++j2) {
                const x = grid[i][j1] + (j1 === j2 ? 0 : grid[i][j2]);
                for (let y1 = j1 - 1; y1 <= j1 + 1; ++y1) {
                    for (let y2 = j2 - 1; y2 <= j2 + 1; ++y2) {
                        if (y1 >= 0 && y1 < n && y2 >= 0 && y2 < n && f[y1][y2] !== -1) {
                            g[j1][j2] = Math.max(g[j1][j2], f[y1][y2] + x);
                        }
                    }
                }
            }
        }
        [f, g] = [g, f];
    }
    return Math.max(...f.flat());
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
