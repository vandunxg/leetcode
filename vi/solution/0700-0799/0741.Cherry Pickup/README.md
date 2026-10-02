---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [741. Cherry Pickup](https://leetcode.com/problems/cherry-pickup)

[中文文档](/solution/0700-0799/0741.Cherry%20Pickup/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>grid</code> kích thước <code>n x n</code> biểu diễn một khu vườn cherry, mỗi ô chứa một trong ba giá trị nguyên có thể có.</p>

<ul>
	<li><code>0</code> nghĩa là ô trống và bạn có thể đi qua,</li>
	<li><code>1</code> nghĩa là ô có cherry mà bạn có thể hái rồi đi qua,</li>
	<li><code>-1</code> nghĩa là ô có gai chắn đường.</li>
</ul>

<p>Hãy trả về <em>số cherry tối đa có thể thu thập theo các quy tắc sau</em>:</p>

<ul>
	<li>Bắt đầu tại vị trí <code>(0, 0)</code> và đi đến <code>(n - 1, n - 1)</code> bằng cách di chuyển sang phải hoặc xuống dưới qua các ô hợp lệ (ô có giá trị <code>0</code> hoặc <code>1</code>).</li>
	<li>Sau khi đến <code>(n - 1, n - 1)</code>, quay về <code>(0, 0)</code> bằng cách di chuyển sang trái hoặc lên trên qua các ô hợp lệ.</li>
	<li>Khi đi qua ô trên đường có cherry, bạn hái quả đó và ô trở thành ô trống <code>0</code>.</li>
	<li>Nếu không có đường đi hợp lệ giữa <code>(0, 0)</code> và <code>(n - 1, n - 1)</code>, bạn không thể thu thập cherry nào.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0741.Cherry%20Pickup/images/grid.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,-1],[1,0,-1],[1,1,1]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Người chơi bắt đầu tại (0, 0), đi xuống, xuống, sang phải, sang phải để đến (2, 2).
Trong lượt đi này, người chơi hái được 4 quả cherry và ma trận trở thành [[0,1,-1],[0,0,-1],[0,0,0]].
Sau đó, người chơi đi sang trái, lên trên, lên trên, sang trái để về điểm xuất phát và hái thêm một quả cherry.
Tổng cộng hái được 5 quả, đây là số lượng tối đa có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,-1],[1,-1,1],[-1,1,1]]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>grid[i][j]</code> là <code>-1</code>, <code>0</code> hoặc <code>1</code>.</li>
	<li><code>grid[0][0] != -1</code></li>
	<li><code>grid[n - 1][n - 1] != -1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Đi đến góc xa nhất rồi quay về; $n\le 50$. Xét hai đường đi độc lập sẽ có quá nhiều trường hợp, đồng thời khó đảm bảo cherry ở phần giao nhau chỉ được tính một lần.
>
> Có thể xem hành trình khứ hồi là hai lượt đi, mỗi lượt gồm $k$ bước từ điểm xuất phát. Khi số bước bằng nhau, cột được xác định từ hàng, nên trạng thái là $(k,i_1,i_2)$. Ô trùng nhau chỉ được tính một lần; ô có gai không thể đi qua.
>
> $f[k][i_1][i_2]$ được chuyển từ các hàng trước đó $i_1$ hoặc $i_1-1$ và $i_2$ hoặc $i_2-1$, cộng với số cherry hiện tại. Đáp án là $\max(0, f[2n-2][n-1][n-1])$.

<!-- thinking:end -->

Theo mô tả bài toán, người chơi xuất phát từ $(0, 0)$, đến $(n-1, n-1)$ rồi quay lại điểm xuất phát $(0, 0)$. Ta có thể xem như người chơi đi từ $(0, 0)$ đến $(n-1, n-1)$ hai lần.

Vì vậy, định nghĩa $f[k][i_1][i_2]$ là số cherry tối đa có thể hái khi cả hai lượt đi đều đã đi $k$ bước và lần lượt đến $(i_1, k-i_1)$ và $(i_2, k-i_2)$. Ban đầu, $f[0][0][0] = grid[0][0]$. Các giá trị ban đầu khác của $f[k][i_1][i_2]$ là âm vô cùng. Đáp án là $\max(0, f[2n-2][n-1][n-1])$.

Từ mô tả bài toán, ta có công thức chuyển trạng thái:

$$
f[k][i_1][i_2] = \max(f[k-1][x_1][x_2] + t, f[k][i_1][i_2])
$$

Trong đó, $t$ là số cherry tại các vị trí $(i_1, k-i_1)$ và $(i_2, k-i_2)$; $x_1, x_2$ lần lượt là vị trí ở bước trước của $(i_1, k-i_1)$ và $(i_2, k-i_2)$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^3)$, trong đó $n$ là độ dài cạnh của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def cherryPickup(self, grid: List[List[int]]) -> int:
        n = len(grid)
        f = [[[-inf] * n for _ in range(n)] for _ in range((n << 1) - 1)]
        f[0][0][0] = grid[0][0]
        for k in range(1, (n << 1) - 1):
            for i1 in range(n):
                for i2 in range(n):
                    j1, j2 = k - i1, k - i2
                    if (
                        not 0 <= j1 < n
                        or not 0 <= j2 < n
                        or grid[i1][j1] == -1
                        or grid[i2][j2] == -1
                    ):
                        continue
                    t = grid[i1][j1]
                    if i1 != i2:
                        t += grid[i2][j2]
                    for x1 in range(i1 - 1, i1 + 1):
                        for x2 in range(i2 - 1, i2 + 1):
                            if x1 >= 0 and x2 >= 0:
                                f[k][i1][i2] = max(f[k][i1][i2], f[k - 1][x1][x2] + t)
        return max(0, f[-1][-1][-1])
```

#### Java

```java
class Solution {
    public int cherryPickup(int[][] grid) {
        int n = grid.length;
        int[][][] f = new int[n * 2][n][n];
        f[0][0][0] = grid[0][0];
        for (int k = 1; k < n * 2 - 1; ++k) {
            for (int i1 = 0; i1 < n; ++i1) {
                for (int i2 = 0; i2 < n; ++i2) {
                    int j1 = k - i1, j2 = k - i2;
                    f[k][i1][i2] = Integer.MIN_VALUE;
                    if (j1 < 0 || j1 >= n || j2 < 0 || j2 >= n || grid[i1][j1] == -1
                        || grid[i2][j2] == -1) {
                        continue;
                    }
                    int t = grid[i1][j1];
                    if (i1 != i2) {
                        t += grid[i2][j2];
                    }
                    for (int x1 = i1 - 1; x1 <= i1; ++x1) {
                        for (int x2 = i2 - 1; x2 <= i2; ++x2) {
                            if (x1 >= 0 && x2 >= 0) {
                                f[k][i1][i2] = Math.max(f[k][i1][i2], f[k - 1][x1][x2] + t);
                            }
                        }
                    }
                }
            }
        }
        return Math.max(0, f[n * 2 - 2][n - 1][n - 1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int cherryPickup(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<vector<int>>> f(n << 1, vector<vector<int>>(n, vector<int>(n, -1e9)));
        f[0][0][0] = grid[0][0];
        for (int k = 1; k < n * 2 - 1; ++k) {
            for (int i1 = 0; i1 < n; ++i1) {
                for (int i2 = 0; i2 < n; ++i2) {
                    int j1 = k - i1, j2 = k - i2;
                    if (j1 < 0 || j1 >= n || j2 < 0 || j2 >= n || grid[i1][j1] == -1 || grid[i2][j2] == -1) {
                        continue;
                    }
                    int t = grid[i1][j1];
                    if (i1 != i2) {
                        t += grid[i2][j2];
                    }
                    for (int x1 = i1 - 1; x1 <= i1; ++x1) {
                        for (int x2 = i2 - 1; x2 <= i2; ++x2) {
                            if (x1 >= 0 && x2 >= 0) {
                                f[k][i1][i2] = max(f[k][i1][i2], f[k - 1][x1][x2] + t);
                            }
                        }
                    }
                }
            }
        }
        return max(0, f[n * 2 - 2][n - 1][n - 1]);
    }
};
```

#### Go

```go
func cherryPickup(grid [][]int) int {
	n := len(grid)
	f := make([][][]int, (n<<1)-1)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, n)
		}
	}
	f[0][0][0] = grid[0][0]
	for k := 1; k < (n<<1)-1; k++ {
		for i1 := 0; i1 < n; i1++ {
			for i2 := 0; i2 < n; i2++ {
				f[k][i1][i2] = int(-1e9)
				j1, j2 := k-i1, k-i2
				if j1 < 0 || j1 >= n || j2 < 0 || j2 >= n || grid[i1][j1] == -1 || grid[i2][j2] == -1 {
					continue
				}
				t := grid[i1][j1]
				if i1 != i2 {
					t += grid[i2][j2]
				}
				for x1 := i1 - 1; x1 <= i1; x1++ {
					for x2 := i2 - 1; x2 <= i2; x2++ {
						if x1 >= 0 && x2 >= 0 {
							f[k][i1][i2] = max(f[k][i1][i2], f[k-1][x1][x2]+t)
						}
					}
				}
			}
		}
	}
	return max(0, f[n*2-2][n-1][n-1])
}
```

#### TypeScript

```ts
function cherryPickup(grid: number[][]): number {
    const n: number = grid.length;
    const f: number[][][] = Array.from({ length: n * 2 - 1 }, () =>
        Array.from({ length: n }, () => Array.from({ length: n }, () => -Infinity)),
    );
    f[0][0][0] = grid[0][0];
    for (let k = 1; k < n * 2 - 1; ++k) {
        for (let i1 = 0; i1 < n; ++i1) {
            for (let i2 = 0; i2 < n; ++i2) {
                const [j1, j2]: [number, number] = [k - i1, k - i2];
                if (
                    j1 < 0 ||
                    j1 >= n ||
                    j2 < 0 ||
                    j2 >= n ||
                    grid[i1][j1] == -1 ||
                    grid[i2][j2] == -1
                ) {
                    continue;
                }
                const t: number = grid[i1][j1] + (i1 != i2 ? grid[i2][j2] : 0);
                for (let x1 = i1 - 1; x1 <= i1; ++x1) {
                    for (let x2 = i2 - 1; x2 <= i2; ++x2) {
                        if (x1 >= 0 && x2 >= 0) {
                            f[k][i1][i2] = Math.max(f[k][i1][i2], f[k - 1][x1][x2] + t);
                        }
                    }
                }
            }
        }
    }
    return Math.max(0, f[n * 2 - 2][n - 1][n - 1]);
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var cherryPickup = function (grid) {
    const n = grid.length;
    const f = Array.from({ length: n * 2 - 1 }, () =>
        Array.from({ length: n }, () => Array.from({ length: n }, () => -Infinity)),
    );
    f[0][0][0] = grid[0][0];
    for (let k = 1; k < n * 2 - 1; ++k) {
        for (let i1 = 0; i1 < n; ++i1) {
            for (let i2 = 0; i2 < n; ++i2) {
                const [j1, j2] = [k - i1, k - i2];
                if (
                    j1 < 0 ||
                    j1 >= n ||
                    j2 < 0 ||
                    j2 >= n ||
                    grid[i1][j1] == -1 ||
                    grid[i2][j2] == -1
                ) {
                    continue;
                }
                const t = grid[i1][j1] + (i1 != i2 ? grid[i2][j2] : 0);
                for (let x1 = i1 - 1; x1 <= i1; ++x1) {
                    for (let x2 = i2 - 1; x2 <= i2; ++x2) {
                        if (x1 >= 0 && x2 >= 0) {
                            f[k][i1][i2] = Math.max(f[k][i1][i2], f[k - 1][x1][x2] + t);
                        }
                    }
                }
            }
        }
    }
    return Math.max(0, f[n * 2 - 2][n - 1][n - 1]);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
