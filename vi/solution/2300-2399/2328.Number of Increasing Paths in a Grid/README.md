---
comments: true
difficulty: Hard
rating: 2001
source: Weekly Contest 300 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Memoization
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [2328. Number of Increasing Paths in a Grid](https://leetcode.com/problems/number-of-increasing-paths-in-a-grid)

[中文文档](/solution/2300-2399/2328.Number%20of%20Increasing%20Paths%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>m x n</code> <code>grid</code>, trong đó bạn có thể di chuyển từ một ô đến bất kỳ ô kề nào theo cả <code>4</code> hướng.</p>

<p>Hãy trả về <em>số lượng đường đi <strong>tăng</strong> <strong>nghiêm ngặt</strong> trong grid, sao cho bạn có thể bắt đầu từ <strong>bất kỳ</strong> ô nào và kết thúc ở <strong>bất kỳ</strong> ô nào. </em>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Hai đường đi được xem là khác nhau nếu chúng không có chính xác cùng một dãy các ô đã đi qua.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2328.Number%20of%20Increasing%20Paths%20in%20a%20Grid/images/griddrawio-4.png" style="width: 181px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1],[3,4]]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Các đường đi tăng nghiêm ngặt là:
- Các đường đi có độ dài 1: [1], [1], [3], [4].
- Các đường đi có độ dài 2: [1 -&gt; 3], [1 -&gt; 4], [3 -&gt; 4].
- Các đường đi có độ dài 3: [1 -&gt; 3 -&gt; 4].
Tổng số đường đi là 4 + 3 + 1 = 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1],[2]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các đường đi tăng nghiêm ngặt là:
- Các đường đi có độ dài 1: [1], [2].
- Các đường đi có độ dài 2: [1 -&gt; 2].
Tổng số đường đi là 2 + 1 = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 1000</code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi tăng nghiêm ngặt có thể bắt đầu ở bất kỳ ô nào và có độ dài bất kỳ. Với tối đa $10^5$ ô, không thể liệt kê tất cả. Các đường đi từ một ô chỉ phụ thuộc vào những ô kề có giá trị lớn hơn.
>
> Dùng memoization cho $dfs(i,j)$, gồm chính ô đó và các ô kề lớn hơn theo bốn hướng. Mỗi ô chỉ được giải một lần; tổng các kết quả trên toàn bộ grid là đáp án.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i, j)$, biểu diễn số lượng đường đi tăng nghiêm ngặt có thể đi được trên đồ thị grid khi bắt đầu từ hàng $i$, cột $j$. Khi đó, đáp án là $\sum_{i=0}^{m-1} \sum_{j=0}^{n-1} dfs(i, j)$. Trong quá trình tìm kiếm, ta có thể dùng mảng hai chiều $f$ để lưu các kết quả đã tính, tránh tính lại.

Quá trình tính hàm $dfs(i, j)$ như sau:

- Nếu $f[i][j]$ khác $0$, nghĩa là giá trị đã được tính, ta trả về trực tiếp $f[i][j]$;
- Nếu không, ta khởi tạo $f[i][j] = 1$, sau đó duyệt qua bốn hướng từ $(i, j)$. Nếu ô $(x, y)$ theo một hướng nào đó thỏa mãn $0 \leq x \lt m$, $0 \leq y \lt n$ và $grid[i][j] \lt grid[x][y]$, ta có thể đi từ ô $(i, j)$ đến ô $(x, y)$, khi đó các số trên đường đi tăng nghiêm ngặt, nên $f[i][j] += dfs(x, y)$.

Cuối cùng, ta trả về $f[i][j]$.

Đáp án là $\sum_{i=0}^{m-1} \sum_{j=0}^{n-1} dfs(i, j)$.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của đồ thị grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPaths(self, grid: List[List[int]]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            ans = 1
            for a, b in pairwise((-1, 0, 1, 0, -1)):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and grid[i][j] < grid[x][y]:
                    ans = (ans + dfs(x, y)) % mod
            return ans

        mod = 10**9 + 7
        m, n = len(grid), len(grid[0])
        return sum(dfs(i, j) for i in range(m) for j in range(n)) % mod
```

#### Java

```java
class Solution {
    private int[][] f;
    private int[][] grid;
    private int m;
    private int n;
    private final int mod = (int) 1e9 + 7;

    public int countPaths(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        f = new int[m][n];
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans = (ans + dfs(i, j)) % mod;
            }
        }
        return ans;
    }

    private int dfs(int i, int j) {
        if (f[i][j] != 0) {
            return f[i][j];
        }
        int ans = 1;
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[i][j] < grid[x][y]) {
                ans = (ans + dfs(x, y)) % mod;
            }
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countPaths(vector<vector<int>>& grid) {
        const int mod = 1e9 + 7;
        int m = grid.size(), n = grid[0].size();
        int f[m][n];
        memset(f, 0, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (f[i][j]) {
                return f[i][j];
            }
            int ans = 1;
            int dirs[5] = {-1, 0, 1, 0, -1};
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && grid[i][j] < grid[x][y]) {
                    ans = (ans + dfs(x, y)) % mod;
                }
            }
            return f[i][j] = ans;
        };
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans = (ans + dfs(i, j)) % mod;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countPaths(grid [][]int) (ans int) {
	const mod = 1e9 + 7
	m, n := len(grid), len(grid[0])
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, n)
	}
	var dfs func(int, int) int
	dfs = func(i, j int) int {
		if f[i][j] != 0 {
			return f[i][j]
		}
		f[i][j] = 1
		dirs := [5]int{-1, 0, 1, 0, -1}
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n && grid[i][j] < grid[x][y] {
				f[i][j] = (f[i][j] + dfs(x, y)) % mod
			}
		}
		return f[i][j]
	}
	for i, row := range grid {
		for j := range row {
			ans = (ans + dfs(i, j)) % mod
		}
	}
	return
}
```

#### TypeScript

```ts
function countPaths(grid: number[][]): number {
    const mod = 1e9 + 7;
    const m = grid.length;
    const n = grid[0].length;
    const f = new Array(m).fill(0).map(() => new Array(n).fill(0));
    const dfs = (i: number, j: number): number => {
        if (f[i][j]) {
            return f[i][j];
        }
        let ans = 1;
        const dirs: number[] = [-1, 0, 1, 0, -1];
        for (let k = 0; k < 4; ++k) {
            const x = i + dirs[k];
            const y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && grid[i][j] < grid[x][y]) {
                ans = (ans + dfs(x, y)) % mod;
            }
        }
        return (f[i][j] = ans);
    };
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            ans = (ans + dfs(i, j)) % mod;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
