---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Memoization
    - Array
    - Dynamic Programming
    - Directed Acyclic Graph
    - Matrix
---

<!-- problem:start -->

# [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix)

[中文文档](/solution/0300-0399/0329.Longest%20Increasing%20Path%20in%20a%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận số nguyên kích thước <code>m x n</code> <code>matrix</code>, hãy trả về <em>độ dài của đường đi tăng dài nhất trong </em><code>matrix</code>.</p>

<p>Từ mỗi ô, bạn có thể di chuyển theo một trong bốn hướng: trái, phải, lên hoặc xuống. Bạn <strong>không được</strong> di chuyển <strong>chéo</strong> hoặc đi <strong>ra ngoài biên</strong> (tức là không được vòng từ cạnh này sang cạnh đối diện).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0329.Longest%20Increasing%20Path%20in%20a%20Matrix/images/grid1.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[9,9,4],[6,6,8],[2,1,1]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Đường đi tăng dài nhất là <code>[1, 2, 6, 9]</code>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0329.Longest%20Increasing%20Path%20in%20a%20Matrix/images/tmp-grid.jpg" style="width: 253px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[3,4,5],[3,2,6],[2,2,1]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Đường đi tăng dài nhất là <code>[3, 4, 5, 6]</code>. Không được di chuyển theo đường chéo.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[1]]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>0 &lt;= matrix[i][j] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm đường đi tăng nghiêm ngặt dài nhất theo bốn hướng. DFS thông thường có thể bắt đầu lại từ cùng một ô nhiều lần. Điều kiện tăng nghiêm ngặt khiến đồ thị không có chu trình.
>
> $dfs(i,j)$ bằng $1$ cộng với kết quả tốt nhất trong các ô lân cận có giá trị lớn hơn. Memoization đảm bảo mỗi ô chỉ được tính một lần; đáp án là giá trị lớn nhất khi bắt đầu từ mọi ô.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(i, j)$ là độ dài của đường đi tăng dài nhất có thể tạo được khi bắt đầu từ tọa độ $(i, j)$ trong ma trận. Đáp án là $\max_{i, j} \textit{dfs}(i, j)$.

Hàm $dfs(i, j)$ hoạt động như sau:

- Nếu $(i, j)$ đã được duyệt, trả về ngay $\textit{f}(i, j)$;
- Nếu chưa được duyệt, ta xét bốn ô lân cận $(x, y)$. Nếu $0 \le x < m, 0 \le y < n$ và $matrix[x][y] > matrix[i][j]$, ta tiếp tục tìm kiếm từ $(x, y)$. Sau khi tìm xong, cập nhật $\textit{f}(i, j)$ thành $\textit{f}(i, j) = \max(\textit{f}(i, j), \textit{f}(x, y) + 1)$. Cuối cùng, trả về $\textit{f}(i, j)$.

Độ phức tạp thời gian và không gian đều là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

Bài toán tương tự:

- [2328. Number of Increasing Paths in a Grid](https://github.com/doocs/leetcode/blob/main/solution/2300-2399/2328.Number%20of%20Increasing%20Paths%20in%20a%20Grid/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            ans = 0
            for a, b in pairwise((-1, 0, 1, 0, -1)):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and matrix[x][y] > matrix[i][j]:
                    ans = max(ans, dfs(x, y))
            return ans + 1

        m, n = len(matrix), len(matrix[0])
        return max(dfs(i, j) for i in range(m) for j in range(n))
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] matrix;
    private int[][] f;

    public int longestIncreasingPath(int[][] matrix) {
        m = matrix.length;
        n = matrix[0].length;
        f = new int[m][n];
        this.matrix = matrix;
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans = Math.max(ans, dfs(i, j));
            }
        }
        return ans;
    }

    private int dfs(int i, int j) {
        if (f[i][j] != 0) {
            return f[i][j];
        }
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k];
            int y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[i][j]) {
                f[i][j] = Math.max(f[i][j], dfs(x, y));
            }
        }
        return ++f[i][j];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        int f[m][n];
        memset(f, 0, sizeof(f));
        int ans = 0;
        int dirs[5] = {-1, 0, 1, 0, -1};

        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (f[i][j]) {
                return f[i][j];
            }
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[i][j]) {
                    f[i][j] = max(f[i][j], dfs(x, y));
                }
            }
            return ++f[i][j];
        };

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans = max(ans, dfs(i, j));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestIncreasingPath(matrix [][]int) (ans int) {
	m, n := len(matrix), len(matrix[0])
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, n)
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if f[i][j] != 0 {
			return f[i][j]
		}
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if 0 <= x && x < m && 0 <= y && y < n && matrix[x][y] > matrix[i][j] {
				f[i][j] = max(f[i][j], dfs(x, y))
			}
		}
		f[i][j]++
		return f[i][j]
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			ans = max(ans, dfs(i, j))
		}
	}
	return
}
```

#### TypeScript

```ts
function longestIncreasingPath(matrix: number[][]): number {
    const m = matrix.length;
    const n = matrix[0].length;
    const f: number[][] = Array(m)
        .fill(0)
        .map(() => Array(n).fill(0));
    const dirs = [-1, 0, 1, 0, -1];
    const dfs = (i: number, j: number): number => {
        if (f[i][j] > 0) {
            return f[i][j];
        }
        for (let k = 0; k < 4; ++k) {
            const x = i + dirs[k];
            const y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[i][j]) {
                f[i][j] = Math.max(f[i][j], dfs(x, y));
            }
        }
        return ++f[i][j];
    };
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            ans = Math.max(ans, dfs(i, j));
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
