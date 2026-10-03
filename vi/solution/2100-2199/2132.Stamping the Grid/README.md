---
comments: true
difficulty: Hard
rating: 2364
source: Biweekly Contest 69 Q4
tags:
    - Greedy
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [2132. Stamping the Grid](https://leetcode.com/problems/stamping-the-grid)

[中文文档](/solution/2100-2199/2132.Stamping%20the%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận nhị phân <code>m x n</code> <code>grid</code>, trong đó mỗi ô là <code>0</code> (trống) hoặc <code>1</code> (đã được chiếm).</p>

<p>Sau đó, bạn được cho các con tem có kích thước <code>stampHeight x stampWidth</code>. Ta muốn đặt các con tem sao cho tuân theo các <strong>giới hạn</strong> và <strong>yêu cầu</strong> sau:</p>

<ol>
	<li>Phủ tất cả các ô <strong>trống</strong>.</li>
	<li>Không phủ lên bất kỳ ô nào đã <strong>bị chiếm</strong>.</li>
	<li>Có thể đặt <strong>bao nhiêu</strong> con tem tùy ý.</li>
	<li>Các con tem có thể <strong>chồng lên nhau</strong>.</li>
	<li>Không được phép <strong>xoay</strong> con tem.</li>
	<li>Các con tem phải nằm hoàn toàn <strong>bên trong</strong> ma trận.</li>
</ol>

<p>Trả về <code>true</code> <em>nếu có thể đặt các con tem theo đúng các giới hạn và yêu cầu trên. Nếu không, trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2132.Stamping%20the%20Grid/images/ex1.png" style="width: 180px; height: 237px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0]], stampHeight = 4, stampWidth = 3
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có hai con tem chồng lên nhau (được đánh số 1 và 2 trong hình) có thể phủ tất cả các ô trống.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2132.Stamping%20the%20Grid/images/ex2.png" style="width: 170px; height: 179px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,0],[0,1,0,0],[0,0,1,0],[0,0,0,1]], stampHeight = 2, stampWidth = 2
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có cách nào đặt các con tem lên tất cả các ô trống mà không để chúng vượt ra ngoài ma trận.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[r].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>grid[r][c]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>1 &lt;= stampHeight, stampWidth &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum Hai Chiều + Mảng Hiệu Hai Chiều

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi con tem chỉ được đặt trên các ô trống, đồng thời mọi ô trống phải được phủ ít nhất một lần. Nếu kiểm tra từng ô cho mỗi vị trí đặt tem, độ phức tạp sẽ là $O(mn\cdot hw)$ và quá chậm.
>
> Prefix sum 2 chiều kiểm tra một khối $h\times w$ trong $O(1)$; các vị trí đặt tem hợp lệ được ghi lại trong mảng hiệu 2 chiều, sau đó khôi phục để kiểm tra độ phủ của mọi ô trống.
>
> Xây dựng prefix sum của các ô đã chiếm $s$, tăng mảng hiệu $d$ tại mỗi vị trí góc trên bên trái hợp lệ, khôi phục $d$, rồi trả về kết quả thất bại nếu vẫn còn ô trống có độ phủ bằng $0$.

<!-- thinking:end -->

Theo mô tả bài toán, mọi ô trống phải được phủ bởi một con tem và không con tem nào được phủ lên ô đã bị chiếm. Do đó, ta có thể duyệt qua ma trận hai chiều; với mỗi ô, nếu tất cả các ô trong vùng có kích thước $stampHeight \times stampWidth$ và ô hiện tại là góc trên bên trái đều trống (tức là không bị chiếm), ta có thể đặt một con tem tại ô đó.

Để nhanh chóng xác định tất cả các ô trong một vùng có trống hay không, ta có thể sử dụng prefix sum hai chiều. Gọi $s_{i,j}$ là số ô đã bị chiếm trong ma trận con từ $(1,1)$ đến $(i,j)$ của ma trận hai chiều. Khi đó, $s_{i, j} = s_{i - 1, j} + s_{i, j - 1} - s_{i - 1, j - 1} + grid_{i-1, j-1}$.

Tiếp theo, với $(i, j)$ là góc trên bên trái, chiều cao và chiều rộng lần lượt là $stampHeight$ và $stampWidth$, tọa độ góc dưới bên phải của ma trận con là $(x, y) = (i + stampHeight - 1, j + stampWidth - 1)$. Ta có thể tính số ô đã bị chiếm trong ma trận con này bằng $s_{x, y} - s_{x, j - 1} - s_{i - 1, y} + s_{i - 1, j - 1}$. Nếu số ô đã bị chiếm trong ma trận con bằng $0$, ta có thể đặt một con tem tại $(i, j)$. Sau khi đặt tem, tất cả các ô trong vùng $stampHeight \times stampWidth$ này sẽ trở thành các ô đã bị chiếm. Ta có thể dùng mảng hiệu hai chiều $d$ để ghi lại thay đổi này:

$$
\begin{aligned}
d_{i, j} &\leftarrow d_{i, j} + 1 \\
d_{i, y + 1} &\leftarrow d_{i, y + 1} - 1 \\
d_{x + 1, j} &\leftarrow d_{x + 1, j} - 1 \\
d_{x + 1, y + 1} &\leftarrow d_{x + 1, y + 1} + 1
\end{aligned}
$$

Cuối cùng, ta thực hiện phép tính prefix sum trên mảng hiệu hai chiều $d$ để tìm số lần mỗi ô được phủ bởi một con tem. Nếu một ô không bị chiếm và số lần nó được phủ bởi tem bằng $0$, ta không thể đặt tem tại ô đó, vì vậy cần trả về $\texttt{false}$. Nếu tất cả các "ô chưa bị chiếm" đều được phủ thành công bởi các con tem, trả về $\texttt{true}$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là chiều cao và chiều rộng của ma trận hai chiều.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def possibleToStamp(
        self, grid: List[List[int]], stampHeight: int, stampWidth: int
    ) -> bool:
        m, n = len(grid), len(grid[0])
        s = [[0] * (n + 1) for _ in range(m + 1)]
        for i, row in enumerate(grid, 1):
            for j, v in enumerate(row, 1):
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + v
        d = [[0] * (n + 2) for _ in range(m + 2)]
        for i in range(1, m - stampHeight + 2):
            for j in range(1, n - stampWidth + 2):
                x, y = i + stampHeight - 1, j + stampWidth - 1
                if s[x][y] - s[x][j - 1] - s[i - 1][y] + s[i - 1][j - 1] == 0:
                    d[i][j] += 1
                    d[i][y + 1] -= 1
                    d[x + 1][j] -= 1
                    d[x + 1][y + 1] += 1
        for i, row in enumerate(grid, 1):
            for j, v in enumerate(row, 1):
                d[i][j] += d[i - 1][j] + d[i][j - 1] - d[i - 1][j - 1]
                if v == 0 and d[i][j] == 0:
                    return False
        return True
```

#### Java

```java
class Solution {
    public boolean possibleToStamp(int[][] grid, int stampHeight, int stampWidth) {
        int m = grid.length, n = grid[0].length;
        int[][] s = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + grid[i - 1][j - 1];
            }
        }
        int[][] d = new int[m + 2][n + 2];
        for (int i = 1; i + stampHeight - 1 <= m; ++i) {
            for (int j = 1; j + stampWidth - 1 <= n; ++j) {
                int x = i + stampHeight - 1, y = j + stampWidth - 1;
                if (s[x][y] - s[x][j - 1] - s[i - 1][y] + s[i - 1][j - 1] == 0) {
                    d[i][j]++;
                    d[i][y + 1]--;
                    d[x + 1][j]--;
                    d[x + 1][y + 1]++;
                }
            }
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                d[i][j] += d[i - 1][j] + d[i][j - 1] - d[i - 1][j - 1];
                if (grid[i - 1][j - 1] == 0 && d[i][j] == 0) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool possibleToStamp(vector<vector<int>>& grid, int stampHeight, int stampWidth) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> s(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + grid[i - 1][j - 1];
            }
        }

        vector<vector<int>> d(m + 2, vector<int>(n + 2));
        for (int i = 1; i + stampHeight - 1 <= m; ++i) {
            for (int j = 1; j + stampWidth - 1 <= n; ++j) {
                int x = i + stampHeight - 1, y = j + stampWidth - 1;
                if (s[x][y] - s[x][j - 1] - s[i - 1][y] + s[i - 1][j - 1] == 0) {
                    d[i][j]++;
                    d[i][y + 1]--;
                    d[x + 1][j]--;
                    d[x + 1][y + 1]++;
                }
            }
        }

        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                d[i][j] += d[i - 1][j] + d[i][j - 1] - d[i - 1][j - 1];
                if (grid[i - 1][j - 1] == 0 && d[i][j] == 0) {
                    return false;
                }
            }
        }
        return true;
    }
};
```

#### Go

```go
func possibleToStamp(grid [][]int, stampHeight int, stampWidth int) bool {
	m, n := len(grid), len(grid[0])
	s := make([][]int, m+1)
	for i := range s {
		s[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + grid[i-1][j-1]
		}
	}

	d := make([][]int, m+2)
	for i := range d {
		d[i] = make([]int, n+2)
	}

	for i := 1; i+stampHeight-1 <= m; i++ {
		for j := 1; j+stampWidth-1 <= n; j++ {
			x, y := i+stampHeight-1, j+stampWidth-1
			if s[x][y]-s[x][j-1]-s[i-1][y]+s[i-1][j-1] == 0 {
				d[i][j]++
				d[i][y+1]--
				d[x+1][j]--
				d[x+1][y+1]++
			}
		}
	}

	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			d[i][j] += d[i-1][j] + d[i][j-1] - d[i-1][j-1]
			if grid[i-1][j-1] == 0 && d[i][j] == 0 {
				return false
			}
		}
	}
	return true
}
```

#### TypeScript

```ts
function possibleToStamp(grid: number[][], stampHeight: number, stampWidth: number): boolean {
    const m = grid.length;
    const n = grid[0].length;
    const s: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + grid[i - 1][j - 1];
        }
    }

    const d: number[][] = Array.from({ length: m + 2 }, () => Array(n + 2).fill(0));
    for (let i = 1; i + stampHeight - 1 <= m; ++i) {
        for (let j = 1; j + stampWidth - 1 <= n; ++j) {
            const [x, y] = [i + stampHeight - 1, j + stampWidth - 1];
            if (s[x][y] - s[x][j - 1] - s[i - 1][y] + s[i - 1][j - 1] === 0) {
                d[i][j]++;
                d[i][y + 1]--;
                d[x + 1][j]--;
                d[x + 1][y + 1]++;
            }
        }
    }

    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            d[i][j] += d[i - 1][j] + d[i][j - 1] - d[i - 1][j - 1];
            if (grid[i - 1][j - 1] === 0 && d[i][j] === 0) {
                return false;
            }
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn possible_to_stamp(grid: Vec<Vec<i32>>, stamp_height: i32, stamp_width: i32) -> bool {
        let n: usize = grid.len();
        let m: usize = grid[0].len();

        let mut prefix_vec: Vec<Vec<i32>> = vec![vec![0; m + 1]; n + 1];

        // Initialize the prefix vector
        for i in 0..n {
            for j in 0..m {
                prefix_vec[i + 1][j + 1] =
                    prefix_vec[i][j + 1] + prefix_vec[i + 1][j] - prefix_vec[i][j] + grid[i][j];
            }
        }

        let mut diff_vec: Vec<Vec<i32>> = vec![vec![0; m + 1]; n + 1];

        // Construct the difference vector based on prefix sum vector
        for i in 0..n {
            for j in 0..m {
                // If the value of the cell is one, just bypass this
                if grid[i][j] == 1 {
                    continue;
                }
                // Otherwise, try stick the stamp
                let x: usize = i + (stamp_height as usize);
                let y: usize = j + (stamp_width as usize);
                // Check the bound
                if x <= n && y <= m {
                    // If the region can be sticked (All cells are empty, which means the sum will be zero)
                    if prefix_vec[x][y] - prefix_vec[x][j] - prefix_vec[i][y] + prefix_vec[i][j]
                        == 0
                    {
                        // Update the difference vector
                        diff_vec[i][j] += 1;
                        diff_vec[x][y] += 1;

                        diff_vec[x][j] -= 1;
                        diff_vec[i][y] -= 1;
                    }
                }
            }
        }

        let mut check_vec: Vec<Vec<i32>> = vec![vec![0; m + 1]; n + 1];

        // Check the prefix sum of difference vector, to determine if there is any empty cell left
        for i in 0..n {
            for j in 0..m {
                // If the value of the cell is one, just bypass this
                if grid[i][j] == 1 {
                    continue;
                }
                // Otherwise, check if the region is empty, by calculating the prefix sum of difference vector
                check_vec[i + 1][j + 1] =
                    check_vec[i][j + 1] + check_vec[i + 1][j] - check_vec[i][j] + diff_vec[i][j];
                if check_vec[i + 1][j + 1] == 0 {
                    return false;
                }
            }
        }

        true
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @param {number} stampHeight
 * @param {number} stampWidth
 * @return {boolean}
 */
var possibleToStamp = function (grid, stampHeight, stampWidth) {
    const m = grid.length;
    const n = grid[0].length;
    const s = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + grid[i - 1][j - 1];
        }
    }

    const d = Array.from({ length: m + 2 }, () => Array(n + 2).fill(0));
    for (let i = 1; i + stampHeight - 1 <= m; ++i) {
        for (let j = 1; j + stampWidth - 1 <= n; ++j) {
            const [x, y] = [i + stampHeight - 1, j + stampWidth - 1];
            if (s[x][y] - s[x][j - 1] - s[i - 1][y] + s[i - 1][j - 1] === 0) {
                d[i][j]++;
                d[i][y + 1]--;
                d[x + 1][j]--;
                d[x + 1][y + 1]++;
            }
        }
    }

    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            d[i][j] += d[i - 1][j] + d[i][j - 1] - d[i - 1][j - 1];
            if (grid[i - 1][j - 1] === 0 && d[i][j] === 0) {
                return false;
            }
        }
    }
    return true;
};
```

#### Kotlin

```kotlin
class Solution {
    fun possibleToStamp(grid: Array<IntArray>, stampHeight: Int, stampWidth: Int): Boolean {
        val m = grid.size
        val n = grid[0].size

        var prefix_sums_matrix = Array(m + 1) { IntArray(n + 1) }
        var diff_matrix = Array(m + 1) { IntArray(n + 1) }
        var sum_matrix = Array(m + 1) { IntArray(n + 1) }

        for (i in 0..<m) {
            for (j in 0..<n) {
                prefix_sums_matrix[i + 1][j + 1] =
                    prefix_sums_matrix[i + 1][j] +
                    prefix_sums_matrix[i][j + 1] -
                    prefix_sums_matrix[i][j] +
                    grid[i][j]
            }
        }

        for (i in 0..<m) {
            for (j in 0..<n) {
                if (grid[i][j] != 0) {
                    continue
                }

                val bottom = i + stampHeight
                val right = j + stampWidth

                if (bottom > m || right > n) {
                    continue
                }

                val sum = prefix_sums_matrix[bottom][right] -
                    prefix_sums_matrix[bottom][j] -
                    prefix_sums_matrix[i][right] +
                    prefix_sums_matrix[i][j]

                if (sum == 0) {
                    diff_matrix[i][j] += 1
                    diff_matrix[bottom][right] += 1

                    diff_matrix[i][right] -= 1
                    diff_matrix[bottom][j] -= 1
                }
            }
        }

        for (i in 0..<m) {
            for (j in 0..<n) {
                if (grid[i][j] != 0) {
                    continue
                }

                val sum = sum_matrix[i][j + 1] +
                    sum_matrix[i + 1][j] -
                    sum_matrix[i][j] +
                    diff_matrix[i][j]

                if (sum == 0) {
                    return false
                }

                sum_matrix[i + 1][j + 1] = sum
            }
        }

        return true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
