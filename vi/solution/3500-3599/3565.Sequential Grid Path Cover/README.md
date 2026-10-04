---
comments: true
difficulty: Medium
tags:
    - Recursion
    - Array
    - Matrix
---

<!-- problem:start -->

# [3565. Sequential Grid Path Cover 🔒](https://leetcode.com/problems/sequential-grid-path-cover)

[中文文档](/solution/3500-3599/3565.Sequential%20Grid%20Path%20Cover/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng 2D <code>grid</code> có kích thước <code>m x n</code> và một số nguyên <code>k</code>. Có <code>k</code> ô trong <code>grid</code> chứa các giá trị từ 1 đến <code>k</code> <strong>mỗi giá trị đúng một lần</strong>, các ô còn lại có giá trị 0.</p>

<p>Bạn có thể bắt đầu tại bất kỳ ô nào và di chuyển từ một ô đến các ô kề (lên, xuống, trái hoặc phải). Hãy tìm một đường đi trong <code>grid</code> thỏa mãn:</p>

<ul>
    <li>Đi qua mỗi ô trong <code>grid</code> <strong>đúng một lần</strong>.</li>
    <li>Đi qua các ô có giá trị từ 1 đến <code>k</code> <strong>theo đúng thứ tự</strong>.</li>
</ul>

<p>Trả về một mảng 2D <code>result</code> có kích thước <code>(m * n) x 2</code>, trong đó <code>result[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn ô thứ <code>i<sup>th</sup></code> được đi qua trong đường đi. Nếu có nhiều đường đi như vậy, bạn có thể trả về <strong>bất kỳ</strong> đường đi nào.</p>

<p>Nếu không tồn tại đường đi thỏa mãn, trả về một mảng <strong>rỗng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,0,0],[0,1,2]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[0,0],[1,0],[1,1],[1,2],[0,2],[0,1]]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3565.Sequential%20Grid%20Path%20Cover/images/ezgifcom-animated-gif-maker1.gif" style="width: 200px; height: 160px;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,0,4],[3,0,2]], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại đường đi nào thỏa mãn các điều kiện.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= m == grid.length &lt;= 5</code></li>
    <li><code>1 &lt;= n == grid[i].length &lt;= 5</code></li>
    <li><code>1 &lt;= k &lt;= m * n</code></li>
    <li><code>0 &lt;= grid[i][j] &lt;= k</code></li>
    <li><code>grid</code> chứa tất cả các số nguyên từ 1 đến <code>k</code> <strong>đúng một lần</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: State Compression + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Kích thước của grid không vượt quá $6 \times 6$ và ta cần tìm một đường đi Hamilton đi qua các giá trị đặc biệt theo thứ tự $1,2,\ldots$. Ta có thể dùng một bit mask để lưu tập các ô đã đi qua.
>
> Thực hiện DFS từ mọi ô có giá trị $0$ hoặc $1$. Một ô kề hợp lệ nếu chưa được đi qua và có giá trị $0$ hoặc giá trị cần tìm tiếp theo $v$; khi đi qua $v$, ta tăng $v$. Quay lui và thử mọi điểm bắt đầu.

<!-- thinking:end -->

Lưu ý rằng kích thước ma trận không vượt quá $6 \times 6$, nên ta có thể dùng state compression để biểu diễn các ô đã đi qua. Ta dùng một số nguyên $\textit{st}$ để biểu diễn các ô đã đi qua, trong đó bit thứ $i$ bằng 1 nghĩa là ô thứ $i$ đã được đi qua, còn bằng 0 nghĩa là chưa đi qua.

Tiếp theo, ta lần lượt chọn mỗi ô làm điểm bắt đầu. Nếu ô đó có giá trị 0 hoặc 1, ta bắt đầu depth-first search (DFS) từ ô đó. Trong DFS, ta thêm ô hiện tại vào đường đi và đánh dấu ô này đã được đi qua. Sau đó, ta kiểm tra giá trị của ô hiện tại. Nếu giá trị đó bằng $v$, ta tăng $v$ lên 1. Tiếp theo, ta thử di chuyển theo bốn hướng đến các ô kề. Nếu ô kề chưa được đi qua và có giá trị 0 hoặc $v$, ta tiếp tục DFS.

Nếu DFS tìm được một đường đi hoàn chỉnh, ta trả về đường đi đó. Nếu không thể tìm được đường đi hoàn chỉnh, ta quay lui, bỏ đánh dấu ô hiện tại và thử các hướng khác.

Độ phức tạp thời gian là $O(m^2 \times n^2)$, và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPath(self, grid: List[List[int]], k: int) -> List[List[int]]:
        def f(i: int, j: int) -> int:
            return i * n + j

        def dfs(i: int, j: int, v: int):
            nonlocal st
            path.append([i, j])
            if len(path) == m * n:
                return True
            st |= 1 << f(i, j)
            if grid[i][j] == v:
                v += 1
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if (
                    0 <= x < m
                    and 0 <= y < n
                    and (st & 1 << f(x, y)) == 0
                    and grid[x][y] in (0, v)
                ):
                    if dfs(x, y, v):
                        return True
            path.pop()
            st ^= 1 << f(i, j)
            return False

        m, n = len(grid), len(grid[0])
        st = 0
        path = []
        dirs = (-1, 0, 1, 0, -1)
        for i in range(m):
            for j in range(n):
                if grid[i][j] in (0, 1):
                    if dfs(i, j, 1):
                        return path
                    path.clear()
                    st = 0
        return []
```

#### Java

```java
class Solution {
    private int m, n;
    private long st = 0;
    private List<List<Integer>> path = new ArrayList<>();
    private final int[] dirs = {-1, 0, 1, 0, -1};

    private int f(int i, int j) {
        return i * n + j;
    }

    private boolean dfs(int i, int j, int v, int[][] grid) {
        path.add(Arrays.asList(i, j));
        if (path.size() == m * n) {
            return true;
        }
        st |= 1L << f(i, j);
        if (grid[i][j] == v) {
            v += 1;
        }
        for (int t = 0; t < 4; t++) {
            int a = dirs[t], b = dirs[t + 1];
            int x = i + a, y = j + b;
            if (0 <= x && x < m && 0 <= y && y < n && (st & (1L << f(x, y))) == 0
                && (grid[x][y] == 0 || grid[x][y] == v)) {
                if (dfs(x, y, v, grid)) {
                    return true;
                }
            }
        }
        path.remove(path.size() - 1);
        st ^= 1L << f(i, j);
        return false;
    }

    public List<List<Integer>> findPath(int[][] grid, int k) {
        m = grid.length;
        n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0 || grid[i][j] == 1) {
                    if (dfs(i, j, 1, grid)) {
                        return path;
                    }
                    path.clear();
                    st = 0;
                }
            }
        }
        return List.of();
    }
}
```

#### C++

```cpp
class Solution {
    int m, n;
    unsigned long long st = 0;
    vector<vector<int>> path;
    int dirs[5] = {-1, 0, 1, 0, -1};

    int f(int i, int j) {
        return i * n + j;
    }

    bool dfs(int i, int j, int v, vector<vector<int>>& grid) {
        path.push_back({i, j});
        if (path.size() == static_cast<size_t>(m * n)) {
            return true;
        }
        st |= 1ULL << f(i, j);
        if (grid[i][j] == v) {
            v += 1;
        }
        for (int t = 0; t < 4; ++t) {
            int a = dirs[t], b = dirs[t + 1];
            int x = i + a, y = j + b;
            if (0 <= x && x < m && 0 <= y && y < n && (st & (1ULL << f(x, y))) == 0
                && (grid[x][y] == 0 || grid[x][y] == v)) {
                if (dfs(x, y, v, grid)) {
                    return true;
                }
            }
        }
        path.pop_back();
        st ^= 1ULL << f(i, j);
        return false;
    }

public:
    vector<vector<int>> findPath(vector<vector<int>>& grid, int k) {
        m = grid.size();
        n = grid[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0 || grid[i][j] == 1) {
                    if (dfs(i, j, 1, grid)) {
                        return path;
                    }
                    path.clear();
                    st = 0;
                }
            }
        }
        return {};
    }
};
```

#### Go

```go
func findPath(grid [][]int, k int) [][]int {
    _ = k
    m := len(grid)
    n := len(grid[0])
    var st uint64
    path := [][]int{}
    dirs := []int{-1, 0, 1, 0, -1}

    f := func(i, j int) int { return i*n + j }

    var dfs func(int, int, int) bool
    dfs = func(i, j, v int) bool {
        path = append(path, []int{i, j})
        if len(path) == m*n {
            return true
        }
        idx := f(i, j)
        st |= 1 << idx
        if grid[i][j] == v {
            v++
        }
        for t := 0; t < 4; t++ {
            a, b := dirs[t], dirs[t+1]
            x, y := i+a, j+b
            if 0 <= x && x < m && 0 <= y && y < n {
                idx2 := f(x, y)
                if (st>>idx2)&1 == 0 && (grid[x][y] == 0 || grid[x][y] == v) {
                    if dfs(x, y, v) {
                        return true
                    }
                }
            }
        }
        path = path[:len(path)-1]
        st ^= 1 << idx
        return false
    }

    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == 0 || grid[i][j] == 1 {
                if dfs(i, j, 1) {
                    return path
                }
                path = path[:0]
                st = 0
            }
        }
    }
    return [][]int{}
}
```

#### TypeScript

```ts
function findPath(grid: number[][], k: number): number[][] {
    const m = grid.length;
    const n = grid[0].length;

    const dirs = [-1, 0, 1, 0, -1];
    const path: number[][] = [];
    let st = 0;

    function f(i: number, j: number): number {
        return i * n + j;
    }

    function dfs(i: number, j: number, v: number): boolean {
        path.push([i, j]);
        if (path.length === m * n) {
            return true;
        }

        st |= 1 << f(i, j);
        if (grid[i][j] === v) {
            v += 1;
        }

        for (let d = 0; d < 4; d++) {
            const x = i + dirs[d];
            const y = j + dirs[d + 1];
            const pos = f(x, y);
            if (
                x >= 0 &&
                x < m &&
                y >= 0 &&
                y < n &&
                (st & (1 << pos)) === 0 &&
                (grid[x][y] === 0 || grid[x][y] === v)
            ) {
                if (dfs(x, y, v)) {
                    return true;
                }
            }
        }

        path.pop();
        st ^= 1 << f(i, j);
        return false;
    }

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 0 || grid[i][j] === 1) {
                st = 0;
                path.length = 0;
                if (dfs(i, j, 1)) {
                    return path;
                }
            }
        }
    }

    return [];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
