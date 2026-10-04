---
comments: true
difficulty: Medium
rating: 1489
source: Biweekly Contest 103 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Matrix
---

<!-- problem:start -->

# [2658. Maximum Number of Fish in a Grid](https://leetcode.com/problems/maximum-number-of-fish-in-a-grid)

[中文文档](/solution/2600-2699/2658.Maximum%20Number%20of%20Fish%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận 2D <code>grid</code> được đánh chỉ số từ <strong>0</strong>, có kích thước <code>m x n</code>, trong đó <code>(r, c)</code> biểu diễn:</p>

<ul>
	<li>Một ô <strong>đất liền</strong> nếu <code>grid[r][c] = 0</code>, hoặc</li>
	<li>Một ô <strong>nước</strong> chứa <code>grid[r][c]</code> con cá nếu <code>grid[r][c] &gt; 0</code>.</li>
</ul>

<p>Một người câu cá có thể bắt đầu tại bất kỳ ô <strong>nước</strong> nào <code>(r, c)</code> và thực hiện các thao tác sau bao nhiêu lần tùy ý:</p>

<ul>
	<li>Bắt toàn bộ cá tại ô <code>(r, c)</code>, hoặc</li>
	<li>Di chuyển đến bất kỳ ô <strong>nước</strong> nào kề với ô hiện tại.</li>
</ul>

<p>Hãy trả về <em>số lượng cá <strong>lớn nhất</strong> mà người câu cá có thể bắt được nếu chọn ô bắt đầu tối ưu, hoặc </em><code>0</code> nếu không tồn tại ô nước nào.</p>

<p>Một ô <strong>kề</strong> với ô <code>(r, c)</code> là một trong các ô <code>(r, c + 1)</code>, <code>(r, c - 1)</code>, <code>(r + 1, c)</code> hoặc <code>(r - 1, c)</code> nếu ô đó tồn tại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2658.Maximum%20Number%20of%20Fish%20in%20a%20Grid/images/example.png" style="width: 241px; height: 161px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,2,1,0],[4,0,0,3],[1,0,0,4],[0,3,2,0]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Người câu cá có thể bắt đầu tại ô <code>(1,3)</code> và bắt 3 con cá, sau đó di chuyển đến ô <code>(2,3)</code>&nbsp;và bắt 4 con cá.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2658.Maximum%20Number%20of%20Fish%20in%20a%20Grid/images/example2.png" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,1]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Người câu cá có thể bắt đầu tại ô (0,0) hoặc (3,3) và bắt được một con cá.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Một chuyến đi có thể đi qua một thành phần nước liên thông 4 hướng và cộng số cá trong thành phần đó. Vì lưới có kích thước tối đa $10 \times 10$, ta bắt đầu DFS/BFS từ mỗi ô nước; các ô đất liền sẽ ngăn cách các thành phần.
>
> Ta đặt giá trị các ô nước đã thăm về 0 để tránh sử dụng lại; tổng lớn nhất của một thành phần là đáp án, hoặc là $0$ nếu không có thành phần nào.

<!-- thinking:end -->

Theo mô tả bài toán, ta chỉ cần tìm số cá trong mỗi vùng nước liên thông rồi lấy giá trị lớn nhất. Vì vậy, ta có thể dùng phương pháp tìm kiếm theo chiều sâu để giải bài toán này.

Ta định nghĩa hàm $dfs(i, j)$, biểu thị số cá lớn nhất có thể bắt được khi bắt đầu từ ô ở hàng thứ $i$ và cột thứ $j$. Logic thực thi của hàm $dfs(i, j)$ như sau:

Ta dùng biến $cnt$ để ghi nhận số cá trong ô hiện tại, sau đó đặt số cá trong ô hiện tại về $0$, biểu thị rằng ô này đã được bắt cá. Tiếp theo, ta duyệt qua bốn hướng của ô hiện tại. Nếu ô $(x, y)$ theo một hướng nào đó nằm trong lưới và là ô nước, ta đệ quy gọi hàm $dfs(x, y)$ rồi cộng giá trị trả về vào $cnt$. Cuối cùng, trả về $cnt$.

Trong hàm chính, ta duyệt qua tất cả các ô $(i, j)$. Nếu ô hiện tại là ô nước, ta gọi hàm $dfs(i, j)$, lấy giá trị lớn nhất trong các giá trị trả về làm đáp án rồi trả về kết quả đó.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxFish(self, grid: List[List[int]]) -> int:
        def dfs(i: int, j: int) -> int:
            cnt = grid[i][j]
            grid[i][j] = 0
            for a, b in pairwise((-1, 0, 1, 0, -1)):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and grid[x][y]:
                    cnt += dfs(x, y)
            return cnt

        m, n = len(grid), len(grid[0])
        ans = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j]:
                    ans = max(ans, dfs(i, j))
        return ans
```

#### Java

```java
class Solution {
    private int[][] grid;
    private int m;
    private int n;

    public int findMaxFish(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] > 0) {
                    ans = Math.max(ans, dfs(i, j));
                }
            }
        }
        return ans;
    }

    private int dfs(int i, int j) {
        int cnt = grid[i][j];
        grid[i][j] = 0;
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] > 0) {
                cnt += dfs(x, y);
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMaxFish(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int ans = 0;
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            int cnt = grid[i][j];
            grid[i][j] = 0;
            int dirs[5] = {-1, 0, 1, 0, -1};
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y]) {
                    cnt += dfs(x, y);
                }
            }
            return cnt;
        };
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    ans = max(ans, dfs(i, j));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findMaxFish(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	dirs := [5]int{-1, 0, 1, 0, -1}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		cnt := grid[i][j]
		grid[i][j] = 0
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n && grid[x][y] > 0 {
				cnt += dfs(x, y)
			}
		}
		return cnt
	}
	for i := range grid {
		for j := range grid[i] {
			if grid[i][j] > 0 {
				ans = max(ans, dfs(i, j))
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function findMaxFish(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;

    const dirs = [-1, 0, 1, 0, -1];
    const dfs = (i: number, j: number): number => {
        let cnt = grid[i][j];
        grid[i][j] = 0;
        for (let k = 0; k < 4; ++k) {
            const x = i + dirs[k];
            const y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] > 0) {
                cnt += dfs(x, y);
            }
        }
        return cnt;
    };

    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] > 0) {
                ans = Math.max(ans, dfs(i, j));
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
