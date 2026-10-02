---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Hash Table
    - Matrix
    - Sorting
    - Hash Function
---

<!-- problem:start -->

# [694. Number of Distinct Islands 🔒](https://leetcode.com/problems/number-of-distinct-islands)

[中文文档](/solution/0600-0699/0694.Number%20of%20Distinct%20Islands/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>m x n</code> <code>grid</code>. Một hòn đảo là nhóm các ô <code>1</code> (đại diện cho đất) kết nối với nhau theo <strong>4 hướng</strong> (ngang hoặc dọc). Có thể giả sử cả bốn cạnh của grid đều được bao quanh bởi nước.</p>

<p>Hai hòn đảo được xem là giống nhau khi và chỉ khi có thể tịnh tiến một hòn đảo để trùng với hòn đảo kia (không xoay hoặc phản chiếu).</p>

<p>Trả về <em>số lượng hòn đảo <b>khác nhau</b></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0694.Number%20of%20Distinct%20Islands/images/distinctisland1-1-grid.jpg" style="width: 413px; height: 334px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0,0,0],[1,1,0,0,0],[0,0,0,1,1],[0,0,0,1,1]]
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0694.Number%20of%20Distinct%20Islands/images/distinctisland1-2-grid.jpg" style="width: 413px; height: 334px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0,1,1],[1,0,0,0,0],[0,0,0,0,1],[1,1,0,1,1]]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai hòn đảo giống nhau khi tịnh tiến có thể trùng nhau; phép xoay không được tính. Tọa độ tuyệt đối cần được quy về cùng một gốc.
>
> DFS ghi lại cả hướng di chuyển lẫn bước quay lui. Các hình dạng khác nhau sẽ tạo ra chuỗi khác nhau; dùng set lưu các chuỗi đó để đếm đáp án. Đánh dấu ô đất thành $0$ khi đã duyệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numDistinctIslands(self, grid: List[List[int]]) -> int:
        def dfs(i: int, j: int, k: int):
            grid[i][j] = 0
            path.append(str(k))
            dirs = (-1, 0, 1, 0, -1)
            for h in range(1, 5):
                x, y = i + dirs[h - 1], j + dirs[h]
                if 0 <= x < m and 0 <= y < n and grid[x][y]:
                    dfs(x, y, h)
            path.append(str(-k))

        paths = set()
        path = []
        m, n = len(grid), len(grid[0])
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if x:
                    dfs(i, j, 0)
                    paths.add("".join(path))
                    path.clear()
        return len(paths)
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] grid;
    private StringBuilder path = new StringBuilder();

    public int numDistinctIslands(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        Set<String> paths = new HashSet<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    dfs(i, j, 0);
                    paths.add(path.toString());
                    path.setLength(0);
                }
            }
        }
        return paths.size();
    }

    private void dfs(int i, int j, int k) {
        grid[i][j] = 0;
        path.append(k);
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int h = 1; h < 5; ++h) {
            int x = i + dirs[h - 1];
            int y = j + dirs[h];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                dfs(x, y, h);
            }
        }
        path.append(k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numDistinctIslands(vector<vector<int>>& grid) {
        unordered_set<string> paths;
        string path;
        int m = grid.size(), n = grid[0].size();
        int dirs[5] = {-1, 0, 1, 0, -1};

        function<void(int, int, int)> dfs = [&](int i, int j, int k) {
            grid[i][j] = 0;
            path += to_string(k);
            for (int h = 1; h < 5; ++h) {
                int x = i + dirs[h - 1], y = j + dirs[h];
                if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y]) {
                    dfs(x, y, h);
                }
            }
            path += to_string(k);
        };

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    dfs(i, j, 0);
                    paths.insert(path);
                    path.clear();
                }
            }
        }
        return paths.size();
    }
};
```

#### Go

```go
func numDistinctIslands(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	paths := map[string]bool{}
	path := []byte{}
	dirs := [5]int{-1, 0, 1, 0, -1}
	var dfs func(i, j, k int)
	dfs = func(i, j, k int) {
		grid[i][j] = 0
		path = append(path, byte(k))
		for h := 1; h < 5; h++ {
			x, y := i+dirs[h-1], j+dirs[h]
			if x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1 {
				dfs(x, y, h)
			}
		}
		path = append(path, byte(k))
	}
	for i, row := range grid {
		for j, x := range row {
			if x == 1 {
				dfs(i, j, 0)
				paths[string(path)] = true
				path = path[:0]
			}
		}
	}
	return len(paths)
}
```

#### TypeScript

```ts
function numDistinctIslands(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const paths: Set<string> = new Set();
    const path: number[] = [];
    const dirs: number[] = [-1, 0, 1, 0, -1];
    const dfs = (i: number, j: number, k: number) => {
        grid[i][j] = 0;
        path.push(k);
        for (let h = 1; h < 5; ++h) {
            const [x, y] = [i + dirs[h - 1], j + dirs[h]];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y]) {
                dfs(x, y, h);
            }
        }
        path.push(k);
    };
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j]) {
                dfs(i, j, 0);
                paths.add(path.join(','));
                path.length = 0;
            }
        }
    }
    return paths.size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
