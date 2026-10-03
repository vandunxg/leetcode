---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [2510. Check if There is a Path With Equal Number of 0's And 1's 🔒](https://leetcode.com/problems/check-if-there-is-a-path-with-equal-number-of-0s-and-1s)

[中文文档](/solution/2500-2599/2510.Check%20if%20There%20is%20a%20Path%20With%20Equal%20Number%20of%200%27s%20And%201%27s/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận <strong>được đánh chỉ số từ 0</strong> <code>m x n</code> <strong>nhị phân</strong> <code>grid</code>. Bạn có thể di chuyển từ ô <code>(row, col)</code> đến một trong các ô <code>(row + 1, col)</code> hoặc <code>(row, col + 1)</code>.</p>

<p>Trả về <code>true</code><em> nếu tồn tại một đường đi từ </em><code>(0, 0)</code><em> đến </em><code>(m - 1, n - 1)</code><em> đi qua số lượng </em><code>0</code><em> và </em><code>1</code><em> <strong>bằng nhau</strong></em>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2510.Check%20if%20There%20is%20a%20Path%20With%20Equal%20Number%20of%200%27s%20And%201%27s/images/yetgriddrawio-4.png" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,0,0],[0,1,0,0],[1,0,1,0]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Đường đi màu xanh trong hình trên là một đường đi hợp lệ vì có 3 ô chứa giá trị 1 và 3 ô chứa giá trị 0. Vì tồn tại một đường đi hợp lệ, ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2510.Check%20if%20There%20is%20a%20Path%20With%20Equal%20Number%20of%200%27s%20And%201%27s/images/yetgrid2drawio-1.png" style="width: 151px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0],[0,0,1],[1,0,0]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có đường đi nào trong ma trận này chứa số lượng 0 và 1 bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 100</code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi từ góc trên bên trái đến góc dưới bên phải chỉ di chuyển sang phải hoặc xuống dưới, nên có độ dài $m+n-1$. Độ dài lẻ không thể chia đều cho số lượng $0$ và $1$. Việc liệt kê toàn bộ đường đi là quá lớn với $m,n\le 100$.
>
> Mục tiêu là có $s=(m+n-1)/2$ số 1 và cùng số lượng số 0. Trạng thái $(i,j,k)$ biểu diễn vị trí $(i,j)$ với $k$ số 1 đã đi qua; loại bỏ trạng thái khi $k$ hoặc số lượng số 0 đã vượt quá $s$. Dùng ghi nhớ cho ta $O(mn(m+n))$ trạng thái.

<!-- thinking:end -->

Theo mô tả bài toán, ta biết rằng số lượng số 0 và 1 trên đường đi từ góc trên bên trái đến góc dưới bên phải là bằng nhau, và tổng số phần tử là $m + n - 1$, nghĩa là số lượng số 0 và 1 đều là $(m + n - 1) / 2$.

Do đó, ta có thể dùng tìm kiếm có ghi nhớ, bắt đầu từ góc trên bên trái và di chuyển sang phải hoặc xuống dưới cho đến khi đến góc dưới bên phải, để kiểm tra xem số lượng số 0 và 1 trên đường đi có bằng nhau hay không.

Độ phức tạp thời gian là $O(m \times n \times (m + n))$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isThereAPath(self, grid: List[List[int]]) -> bool:
        @cache
        def dfs(i, j, k):
            if i >= m or j >= n:
                return False
            k += grid[i][j]
            if k > s or i + j + 1 - k > s:
                return False
            if i == m - 1 and j == n - 1:
                return k == s
            return dfs(i + 1, j, k) or dfs(i, j + 1, k)

        m, n = len(grid), len(grid[0])
        s = m + n - 1
        if s & 1:
            return False
        s >>= 1
        return dfs(0, 0, 0)
```

#### Java

```java
class Solution {
    private int s;
    private int m;
    private int n;
    private int[][] grid;
    private Boolean[][][] f;

    public boolean isThereAPath(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        s = m + n - 1;
        f = new Boolean[m][n][s];
        if (s % 2 == 1) {
            return false;
        }
        s >>= 1;
        return dfs(0, 0, 0);
    }

    private boolean dfs(int i, int j, int k) {
        if (i >= m || j >= n) {
            return false;
        }
        k += grid[i][j];
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        if (k > s || i + j + 1 - k > s) {
            return false;
        }
        if (i == m - 1 && j == n - 1) {
            return k == s;
        }
        f[i][j][k] = dfs(i + 1, j, k) || dfs(i, j + 1, k);
        return f[i][j][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isThereAPath(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int s = m + n - 1;
        if (s & 1) return false;
        int f[m][n][s];
        s >>= 1;
        memset(f, -1, sizeof f);
        function<bool(int, int, int)> dfs = [&](int i, int j, int k) -> bool {
            if (i >= m || j >= n) return false;
            k += grid[i][j];
            if (f[i][j][k] != -1) return f[i][j][k];
            if (k > s || i + j + 1 - k > s) return false;
            if (i == m - 1 && j == n - 1) return k == s;
            f[i][j][k] = dfs(i + 1, j, k) || dfs(i, j + 1, k);
            return f[i][j][k];
        };
        return dfs(0, 0, 0);
    }
};
```

#### Go

```go
func isThereAPath(grid [][]int) bool {
	m, n := len(grid), len(grid[0])
	s := m + n - 1
	if s%2 == 1 {
		return false
	}
	s >>= 1
	f := [100][100][200]int{}
	var dfs func(i, j, k int) bool
	dfs = func(i, j, k int) bool {
		if i >= m || j >= n {
			return false
		}
		k += grid[i][j]
		if f[i][j][k] != 0 {
			return f[i][j][k] == 1
		}
		f[i][j][k] = 2
		if k > s || i+j+1-k > s {
			return false
		}
		if i == m-1 && j == n-1 {
			return k == s
		}
		res := dfs(i+1, j, k) || dfs(i, j+1, k)
		if res {
			f[i][j][k] = 1
		}
		return res
	}
	return dfs(0, 0, 0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
