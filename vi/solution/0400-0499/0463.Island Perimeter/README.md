---
comments: true
difficulty: Easy
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [463. Island Perimeter](https://leetcode.com/problems/island-perimeter)

[中文文档](/solution/0400-0499/0463.Island%20Perimeter/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>grid</code> kích thước <code>row x col</code> biểu diễn bản đồ, trong đó <code>grid[i][j] = 1</code> là đất liền và <code>grid[i][j] = 0</code> là nước.</p>

<p>Các ô trong grid kết nối với nhau theo chiều <strong>ngang/dọc</strong> (không tính đường chéo). <code>grid</code> được bao quanh hoàn toàn bởi nước và chỉ có đúng một hòn đảo (tức là một hoặc nhiều ô đất liền kết nối với nhau).</p>

<p>Hòn đảo không có &quot;hồ&quot;, nghĩa là vùng nước bên trong không kết nối với vùng nước bao quanh đảo. Mỗi ô là hình vuông cạnh dài 1. Grid có dạng hình chữ nhật, chiều rộng và chiều cao không vượt quá 100. Hãy xác định chu vi hòn đảo.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0463.Island%20Perimeter/images/island.png" style="width: 221px; height: 213px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Chu vi gồm 16 đoạn màu vàng trong hình trên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1]]
<strong>Đầu ra:</strong> 4
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0]]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>row == grid.length</code></li>
	<li><code>col == grid[i].length</code></li>
	<li><code>1 &lt;= row, col &lt;= 100</code></li>
	<li><code>grid[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li>Trong <code>grid</code> có đúng một hòn đảo.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đảo kết nối theo 4 hướng; chu vi bằng $4$ lần số ô đất trừ đi hai lần số cạnh chung. Kiểm tra đủ bốn ô láng giềng sẽ cần kiểm tra biên.
>
> Cộng $4$ cho mỗi ô đất; nếu ô bên dưới hoặc bên phải cũng là đất thì cạnh đó được dùng chung, nên trừ $2$. Chỉ xét xuống dưới và sang phải để mỗi cạnh bên trong chỉ được tính một lần.
>
> Không cần graph hay DFS; chỉ cần duyệt grid một lượt để tính chu vi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        ans = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    ans += 4
                    if i < m - 1 and grid[i + 1][j] == 1:
                        ans -= 2
                    if j < n - 1 and grid[i][j + 1] == 1:
                        ans -= 2
        return ans
```

#### Java

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int ans = 0;
        int m = grid.length;
        int n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    ans += 4;
                    if (i < m - 1 && grid[i + 1][j] == 1) {
                        ans -= 2;
                    }
                    if (j < n - 1 && grid[i][j + 1] == 1) {
                        ans -= 2;
                    }
                }
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
    int islandPerimeter(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    ans += 4;
                    if (i < m - 1 && grid[i + 1][j] == 1) ans -= 2;
                    if (j < n - 1 && grid[i][j + 1] == 1) ans -= 2;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func islandPerimeter(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	ans := 0
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == 1 {
				ans += 4
				if i < m-1 && grid[i+1][j] == 1 {
					ans -= 2
				}
				if j < n-1 && grid[i][j+1] == 1 {
					ans -= 2
				}
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function islandPerimeter(grid: number[][]): number {
    let m = grid.length,
        n = grid[0].length;
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            let top = 0,
                left = 0;
            if (i > 0) {
                top = grid[i - 1][j];
            }
            if (j > 0) {
                left = grid[i][j - 1];
            }
            let cur = grid[i][j];
            if (cur != top) ++ans;
            if (cur != left) ++ans;
        }
    }
    // 最后一行， 最后一列
    for (let i = 0; i < m; ++i) {
        if (grid[i][n - 1] == 1) ++ans;
    }
    for (let j = 0; j < n; ++j) {
        if (grid[m - 1][j] == 1) ++ans;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
