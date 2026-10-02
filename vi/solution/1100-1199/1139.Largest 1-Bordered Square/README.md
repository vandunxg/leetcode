---
comments: true
difficulty: Medium
rating: 1744
source: Weekly Contest 147 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [1139. Largest 1-Bordered Square](https://leetcode.com/problems/largest-1-bordered-square)

[中文文档](/solution/1100-1199/1139.Largest%201-Bordered%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>grid</code> hai chiều gồm các giá trị <code>0</code> và <code>1</code>, hãy trả về số phần tử của ma trận con <strong>hình vuông</strong> lớn nhất có toàn bộ các ô trên <strong>viền</strong> đều là <code>1</code>, hoặc trả về <code>0</code> nếu không có ma trận con như vậy trong <code>grid</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[1,0,1],[1,1,1]]
<strong>Đầu ra:</strong> 9
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0,0]]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= grid.length &lt;= 100</code></li>
	<li><code>1 &lt;= grid[0].length &lt;= 100</code></li>
	<li><code>grid[i][j]</code> is <code>0</code> or <code>1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Nếu kiểm tra lần lượt bốn cạnh của từng hình vuông $k\times k$, độ phức tạp của việc duyệt bậc ba sẽ tăng thêm hệ số $k$. Ta lưu độ dài dãy liên tiếp các số $1$ theo hướng xuống dưới và sang phải bắt đầu từ mỗi ô; khi đó có thể kiểm tra độ dài mỗi cạnh trong $O(1)$.
>
> Duyệt $k$ từ lớn đến nhỏ và lần lượt xét góc trên bên trái; trường hợp hợp lệ đầu tiên có diện tích lớn nhất.

<!-- thinking:end -->

Ta có thể dùng phương pháp prefix sum để tiền xử lý số lượng số 1 liên tiếp theo hướng xuống dưới và sang phải từ mỗi vị trí, lần lượt lưu trong `down[i][j]` và `right[i][j]`.

Sau đó, ta duyệt độ dài cạnh $k$ của hình vuông, bắt đầu từ cạnh lớn nhất. Với mỗi $k$, ta duyệt vị trí góc trên bên trái $(i, j)$. Nếu thỏa điều kiện, ta trả về $k^2$.

Độ phức tạp thời gian là $O(m \times n \times \min(m, n))$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largest1BorderedSquare(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        down = [[0] * n for _ in range(m)]
        right = [[0] * n for _ in range(m)]
        for i in range(m - 1, -1, -1):
            for j in range(n - 1, -1, -1):
                if grid[i][j]:
                    down[i][j] = down[i + 1][j] + 1 if i + 1 < m else 1
                    right[i][j] = right[i][j + 1] + 1 if j + 1 < n else 1
        for k in range(min(m, n), 0, -1):
            for i in range(m - k + 1):
                for j in range(n - k + 1):
                    if (
                        down[i][j] >= k
                        and right[i][j] >= k
                        and right[i + k - 1][j] >= k
                        and down[i][j + k - 1] >= k
                    ):
                        return k * k
        return 0
```

#### Java

```java
class Solution {
    public int largest1BorderedSquare(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] down = new int[m][n];
        int[][] right = new int[m][n];
        for (int i = m - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (grid[i][j] == 1) {
                    down[i][j] = i + 1 < m ? down[i + 1][j] + 1 : 1;
                    right[i][j] = j + 1 < n ? right[i][j + 1] + 1 : 1;
                }
            }
        }
        for (int k = Math.min(m, n); k > 0; --k) {
            for (int i = 0; i <= m - k; ++i) {
                for (int j = 0; j <= n - k; ++j) {
                    if (down[i][j] >= k && right[i][j] >= k && right[i + k - 1][j] >= k
                        && down[i][j + k - 1] >= k) {
                        return k * k;
                    }
                }
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largest1BorderedSquare(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int down[m][n];
        int right[m][n];
        memset(down, 0, sizeof down);
        memset(right, 0, sizeof right);
        for (int i = m - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (grid[i][j] == 1) {
                    down[i][j] = i + 1 < m ? down[i + 1][j] + 1 : 1;
                    right[i][j] = j + 1 < n ? right[i][j + 1] + 1 : 1;
                }
            }
        }
        for (int k = min(m, n); k > 0; --k) {
            for (int i = 0; i <= m - k; ++i) {
                for (int j = 0; j <= n - k; ++j) {
                    if (down[i][j] >= k && right[i][j] >= k && right[i + k - 1][j] >= k
                        && down[i][j + k - 1] >= k) {
                        return k * k;
                    }
                }
            }
        }
        return 0;
    }
};
```

#### Go

```go
func largest1BorderedSquare(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	down := make([][]int, m)
	right := make([][]int, m)
	for i := range down {
		down[i] = make([]int, n)
		right[i] = make([]int, n)
	}
	for i := m - 1; i >= 0; i-- {
		for j := n - 1; j >= 0; j-- {
			if grid[i][j] == 1 {
				down[i][j], right[i][j] = 1, 1
				if i+1 < m {
					down[i][j] += down[i+1][j]
				}
				if j+1 < n {
					right[i][j] += right[i][j+1]
				}
			}
		}
	}
	for k := min(m, n); k > 0; k-- {
		for i := 0; i <= m-k; i++ {
			for j := 0; j <= n-k; j++ {
				if down[i][j] >= k && right[i][j] >= k && right[i+k-1][j] >= k && down[i][j+k-1] >= k {
					return k * k
				}
			}
		}
	}
	return 0
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
