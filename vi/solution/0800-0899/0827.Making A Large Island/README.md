---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Matrix
---

<!-- problem:start -->

# [827. Making A Large Island](https://leetcode.com/problems/making-a-large-island)

[中文文档](/solution/0800-0899/0827.Making%20A%20Large%20Island/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>n x n</code> <code>grid</code>. Bạn được phép đổi <strong>nhiều nhất một</strong> <code>0</code> thành <code>1</code>.</p>

<p>Hãy trả về <em>diện tích của <strong>đảo</strong> lớn nhất trong</em> <code>grid</code> <em>sau khi thực hiện thao tác này</em>.</p>

<p><strong>Đảo</strong> là một nhóm các ô <code>1</code> liên thông theo bốn hướng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0],[0,1]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Đổi một ô 0 thành 1 để nối hai ô 1, khi đó ta được một hòn đảo có diện tích = 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1],[1,0]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Đổi ô 0 thành 1 để mở rộng hòn đảo, khi đó chỉ còn một hòn đảo có diện tích = 4.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1],[1,1]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Không thể đổi ô 0 nào thành 1; chỉ có một hòn đảo với diện tích = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể đổi nhiều nhất một ô $0$ và cần tìm hòn đảo lớn nhất. Vì $n\le 500$, nếu flood fill từ từng ô $0$ thì sẽ duyệt lại cùng những hòn đảo nhiều lần.
>
> Trước tiên, dùng DFS gán nhãn cho từng hòn đảo và lưu diện tích của nó. Sau đó, với mỗi ô $0$, cộng diện tích của các đảo kề nhau không trùng lặp rồi cộng thêm một. Nếu toàn bộ grid là số 1, đáp án là đảo hiện có lớn nhất.

<!-- thinking:end -->

Ta có thể gán một mã định danh riêng cho mỗi thành phần liên thông, dùng mảng $p$ để ghi nhận thành phần mà mỗi vị trí thuộc về; tức là, $p[i][j]$ biểu thị mã thành phần liên thông của $(i, j)$. Dùng mảng $cnt$ để lưu kích thước của từng thành phần; cụ thể, $cnt[root]$ là kích thước của thành phần có mã $root$.

Trước tiên, duyệt toàn bộ ma trận. Với mỗi vị trí có $grid[i][j] = 1$ và $p[i][j] = 0$, ta chạy depth-first search, đánh dấu các ô thuộc thành phần liên thông bằng $root$ và đếm kích thước thành phần đó.

Sau đó, duyệt lại toàn bộ ma trận. Với mỗi vị trí có $grid[i][j] = 0$, tìm các thành phần liên thông của bốn ô phía trên, dưới, trái và phải, cộng kích thước của các thành phần khác nhau đó rồi cộng thêm $1$ cho ô hiện tại. Kết quả là diện tích đảo khi đổi ô hiện tại thành $1$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestIsland(self, grid: List[List[int]]) -> int:
        def dfs(i: int, j: int):
            p[i][j] = root
            cnt[root] += 1
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < n and 0 <= y < n and grid[x][y] and p[x][y] == 0:
                    dfs(x, y)

        n = len(grid)
        cnt = Counter()
        p = [[0] * n for _ in range(n)]
        dirs = (-1, 0, 1, 0, -1)
        root = 0
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if x and p[i][j] == 0:
                    root += 1
                    dfs(i, j)
        ans = max(cnt.values() or [0])
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if x == 0:
                    s = set()
                    for a, b in pairwise(dirs):
                        x, y = i + a, j + b
                        if 0 <= x < n and 0 <= y < n:
                            s.add(p[x][y])
                    ans = max(ans, sum(cnt[root] for root in s) + 1)
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int root;
    private int[] cnt;
    private int[][] p;
    private int[][] grid;
    private final int[] dirs = {-1, 0, 1, 0, -1};

    public int largestIsland(int[][] grid) {
        n = grid.length;
        p = new int[n][n];
        this.grid = grid;
        cnt = new int[n * n + 1];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1 && p[i][j] == 0) {
                    ++root;
                    ans = Math.max(ans, dfs(i, j));
                }
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    Set<Integer> s = new HashSet<>();
                    for (int k = 0; k < 4; ++k) {
                        int x = i + dirs[k];
                        int y = j + dirs[k + 1];
                        if (x >= 0 && x < n && y >= 0 && y < n) {
                            s.add(p[x][y]);
                        }
                    }
                    int t = 1;
                    for (int x : s) {
                        t += cnt[x];
                    }
                    ans = Math.max(ans, t);
                }
            }
        }
        return ans;
    }

    private int dfs(int i, int j) {
        p[i][j] = root;
        ++cnt[root];
        for (int k = 0; k < 4; ++k) {
            int x = i + dirs[k];
            int y = j + dirs[k + 1];
            if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] == 1 && p[x][y] == 0) {
                dfs(x, y);
            }
        }
        return cnt[root];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        int p[n][n];
        memset(p, 0, sizeof(p));
        int cnt[n * n + 1];
        memset(cnt, 0, sizeof(cnt));
        const int dirs[5] = {-1, 0, 1, 0, -1};
        int root = 0;
        int ans = 0;
        function<int(int, int)> dfs = [&](int i, int j) {
            p[i][j] = root;
            ++cnt[root];
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k];
                int y = j + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] && !p[x][y]) {
                    dfs(x, y);
                }
            }
            return cnt[root];
        };
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] && !p[i][j]) {
                    ++root;
                    ans = max(ans, dfs(i, j));
                }
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (!grid[i][j]) {
                    unordered_set<int> s;
                    for (int k = 0; k < 4; ++k) {
                        int x = i + dirs[k];
                        int y = j + dirs[k + 1];
                        if (x >= 0 && x < n && y >= 0 && y < n) {
                            s.insert(p[x][y]);
                        }
                    }
                    int t = 1;
                    for (int x : s) {
                        t += cnt[x];
                    }
                    ans = max(ans, t);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestIsland(grid [][]int) int {
	n := len(grid)
	p := make([][]int, n)
	for i := range p {
		p[i] = make([]int, n)
	}
	cnt := make([]int, n*n+1)
	dirs := []int{-1, 0, 1, 0, -1}
	root := 0
	ans := 0

	var dfs func(int, int) int
	dfs = func(i, j int) int {
		p[i][j] = root
		cnt[root]++
		for k := 0; k < 4; k++ {
			x := i + dirs[k]
			y := j + dirs[k+1]
			if x >= 0 && x < n && y >= 0 && y < n && grid[x][y] == 1 && p[x][y] == 0 {
				dfs(x, y)
			}
		}
		return cnt[root]
	}

	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == 1 && p[i][j] == 0 {
				root++
				ans = max(ans, dfs(i, j))
			}
		}
	}

	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == 0 {
				s := make(map[int]struct{})
				for k := 0; k < 4; k++ {
					x := i + dirs[k]
					y := j + dirs[k+1]
					if x >= 0 && x < n && y >= 0 && y < n {
						s[p[x][y]] = struct{}{}
					}
				}
				t := 1
				for x := range s {
					t += cnt[x]
				}
				ans = max(ans, t)
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function largestIsland(grid: number[][]): number {
    const n = grid.length;
    const p = Array.from({ length: n }, () => Array(n).fill(0));
    const cnt = Array(n * n + 1).fill(0);
    const dirs = [-1, 0, 1, 0, -1];
    let root = 0;
    let ans = 0;

    const dfs = (i: number, j: number): number => {
        p[i][j] = root;
        cnt[root]++;
        for (let k = 0; k < 4; ++k) {
            const x = i + dirs[k];
            const y = j + dirs[k + 1];
            if (x >= 0 && x < n && y >= 0 && y < n && grid[x][y] === 1 && p[x][y] === 0) {
                dfs(x, y);
            }
        }
        return cnt[root];
    };

    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 1 && p[i][j] === 0) {
                root++;
                ans = Math.max(ans, dfs(i, j));
            }
        }
    }

    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 0) {
                const s = new Set<number>();
                for (let k = 0; k < 4; ++k) {
                    const x = i + dirs[k];
                    const y = j + dirs[k + 1];
                    if (x >= 0 && x < n && y >= 0 && y < n) {
                        s.add(p[x][y]);
                    }
                }
                let t = 1;
                for (const x of s) {
                    t += cnt[x];
                }
                ans = Math.max(ans, t);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn largest_island(grid: Vec<Vec<i32>>) -> i32 {
        let n = grid.len();
        let mut p = vec![vec![0; n]; n];
        let mut cnt = vec![0; n * n + 1];
        let dirs = [-1, 0, 1, 0, -1];
        let mut root = 0;
        let mut ans = 0;

        fn dfs(
            grid: &Vec<Vec<i32>>,
            p: &mut Vec<Vec<i32>>,
            cnt: &mut Vec<i32>,
            root: i32,
            i: usize,
            j: usize,
            dirs: &[i32; 5],
        ) -> i32 {
            p[i][j] = root;
            cnt[root as usize] += 1;
            for k in 0..4 {
                let x = (i as i32) + dirs[k];
                let y = (j as i32) + dirs[k + 1];
                if x >= 0
                    && (x as usize) < grid.len()
                    && y >= 0
                    && (y as usize) < grid.len()
                    && grid[x as usize][y as usize] == 1
                    && p[x as usize][y as usize] == 0
                {
                    dfs(grid, p, cnt, root, x as usize, y as usize, dirs);
                }
            }
            cnt[root as usize]
        }

        for i in 0..n {
            for j in 0..n {
                if grid[i][j] == 1 && p[i][j] == 0 {
                    root += 1;
                    ans = ans.max(dfs(&grid, &mut p, &mut cnt, root, i, j, &dirs));
                }
            }
        }

        for i in 0..n {
            for j in 0..n {
                if grid[i][j] == 0 {
                    let mut s = HashSet::new();
                    for k in 0..4 {
                        let x = (i as i32) + dirs[k];
                        let y = (j as i32) + dirs[k + 1];
                        if x >= 0 && (x as usize) < n && y >= 0 && (y as usize) < n {
                            s.insert(p[x as usize][y as usize]);
                        }
                    }
                    let mut t = 1;
                    for &x in &s {
                        t += cnt[x as usize];
                    }
                    ans = ans.max(t);
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
