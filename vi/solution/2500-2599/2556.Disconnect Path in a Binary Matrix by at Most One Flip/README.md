---
comments: true
difficulty: Medium
rating: 2368
source: Biweekly Contest 97 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [2556. Disconnect Path in a Binary Matrix by at Most One Flip](https://leetcode.com/problems/disconnect-path-in-a-binary-matrix-by-at-most-one-flip)

[中文文档](/solution/2500-2599/2556.Disconnect%20Path%20in%20a%20Binary%20Matrix%20by%20at%20Most%20One%20Flip/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận <strong>nhị phân</strong> <code>m x n</code> <strong>được đánh chỉ số từ 0</strong> <code>grid</code>. Bạn có thể di chuyển từ một ô <code>(row, col)</code> đến ô <code>(row + 1, col)</code> hoặc <code>(row, col + 1)</code> có giá trị <code>1</code>. Ma trận được gọi là <strong>mất kết nối</strong> nếu không có đường đi từ <code>(0, 0)</code> đến <code>(m - 1, n - 1)</code>.</p>

<p>Bạn có thể đảo giá trị của <strong>tối đa một</strong> ô (có thể không đảo ô nào). Bạn <strong>không thể đảo</strong> các ô <code>(0, 0)</code> và <code>(m - 1, n - 1)</code>.</p>

<p>Trả về <code>true</code> <em>nếu có thể làm cho ma trận mất kết nối hoặc </em><code>false</code><em> nếu không thể</em>.</p>

<p><strong>Lưu ý</strong> rằng đảo một ô sẽ thay đổi giá trị của ô đó từ <code>0</code> thành <code>1</code> hoặc từ <code>1</code> thành <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2556.Disconnect%20Path%20in%20a%20Binary%20Matrix%20by%20at%20Most%20One%20Flip/images/yetgrid2drawio.png" style="width: 441px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[1,0,0],[1,1,1]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể thay đổi ô được hiển thị trong hình trên. Khi đó không còn đường đi từ (0, 0) đến (2, 2) trong ma trận kết quả.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2556.Disconnect%20Path%20in%20a%20Binary%20Matrix%20by%20at%20Most%20One%20Flip/images/yetgrid3drawio.png" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[1,0,1],[1,1,1]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể thay đổi tối đa một ô để không còn đường đi từ (0, 0) đến (2, 2).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 1000</code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>grid[0][0] == grid[m - 1][n - 1] == 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lần duyệt DFS

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có thể di chuyển sang phải hoặc xuống dưới. Ta có thể đảo tối đa một ô không phải điểm đầu hoặc điểm cuối để ngắt kết nối giữa $(0,0)$ và đích. Thử mọi cách đảo sẽ có độ phức tạp bậc ba.
>
> Hai đường đi không giao nhau tại đỉnh bên trong không thể bị ngắt chỉ bằng một lần đảo. Duyệt DFS một lần và đặt các ô đã thăm thành 0 (sau đó khôi phục hai đầu mút), rồi duyệt DFS lần hai: nếu lần hai vẫn tìm thấy đường đi, nghĩa là còn một đường đi không giao nhau, nên không thể ngắt kết nối bằng một lần đảo.

<!-- thinking:end -->

Đầu tiên, ta thực hiện một lần duyệt DFS để xác định có đường đi từ $(0, 0)$ đến $(m - 1, n - 1)$ hay không, và ký hiệu kết quả là $a$. Trong quá trình DFS, ta đặt giá trị của các ô đã thăm thành $0$ để tránh thăm lại.

Tiếp theo, ta đặt giá trị của $(0, 0)$ và $(m - 1, n - 1)$ thành $1$, rồi thực hiện một lần duyệt DFS khác để xác định có đường đi từ $(0, 0)$ đến $(m - 1, n - 1)$ hay không, và ký hiệu kết quả là $b$. Trong quá trình DFS, ta đặt giá trị của các ô đã thăm thành $0$ để tránh thăm lại.

Cuối cùng, nếu cả $a$ và $b$ đều là `true`, ta trả về `false`; ngược lại, ta trả về `true`.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPossibleToCutPath(self, grid: List[List[int]]) -> bool:
        def dfs(i, j):
            if i >= m or j >= n or grid[i][j] == 0:
                return False
            grid[i][j] = 0
            if i == m - 1 and j == n - 1:
                return True
            return dfs(i + 1, j) or dfs(i, j + 1)

        m, n = len(grid), len(grid[0])
        a = dfs(0, 0)
        grid[0][0] = grid[-1][-1] = 1
        b = dfs(0, 0)
        return not (a and b)
```

#### Java

```java
class Solution {
    private int[][] grid;
    private int m;
    private int n;

    public boolean isPossibleToCutPath(int[][] grid) {
        this.grid = grid;
        m = grid.length;
        n = grid[0].length;
        boolean a = dfs(0, 0);
        grid[0][0] = 1;
        grid[m - 1][n - 1] = 1;
        boolean b = dfs(0, 0);
        return !(a && b);
    }

    private boolean dfs(int i, int j) {
        if (i >= m || j >= n || grid[i][j] == 0) {
            return false;
        }
        if (i == m - 1 && j == n - 1) {
            return true;
        }
        grid[i][j] = 0;
        return dfs(i + 1, j) || dfs(i, j + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPossibleToCutPath(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        function<bool(int, int)> dfs = [&](int i, int j) -> bool {
            if (i >= m || j >= n || grid[i][j] == 0) {
                return false;
            }
            if (i == m - 1 && j == n - 1) {
                return true;
            }
            grid[i][j] = 0;
            return dfs(i + 1, j) || dfs(i, j + 1);
        };
        bool a = dfs(0, 0);
        grid[0][0] = grid[m - 1][n - 1] = 1;
        bool b = dfs(0, 0);
        return !(a && b);
    }
};
```

#### Go

```go
func isPossibleToCutPath(grid [][]int) bool {
	m, n := len(grid), len(grid[0])
	var dfs func(i, j int) bool
	dfs = func(i, j int) bool {
		if i >= m || j >= n || grid[i][j] == 0 {
			return false
		}
		if i == m-1 && j == n-1 {
			return true
		}
		grid[i][j] = 0
		return dfs(i+1, j) || dfs(i, j+1)
	}
	a := dfs(0, 0)
	grid[0][0], grid[m-1][n-1] = 1, 1
	b := dfs(0, 0)
	return !(a && b)
}
```

#### TypeScript

```ts
function isPossibleToCutPath(grid: number[][]): boolean {
    const m = grid.length;
    const n = grid[0].length;

    const dfs = (i: number, j: number): boolean => {
        if (i >= m || j >= n || grid[i][j] !== 1) {
            return false;
        }
        grid[i][j] = 0;
        if (i === m - 1 && j === n - 1) {
            return true;
        }
        return dfs(i + 1, j) || dfs(i, j + 1);
    };
    const a = dfs(0, 0);
    grid[0][0] = 1;
    grid[m - 1][n - 1] = 1;
    const b = dfs(0, 0);
    return !(a && b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
