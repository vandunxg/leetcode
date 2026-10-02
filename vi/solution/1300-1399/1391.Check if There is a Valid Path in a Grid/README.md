---
comments: true
difficulty: Medium
rating: 1745
source: Weekly Contest 181 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Matrix
---

<!-- problem:start -->

# [1391. Check if There is a Valid Path in a Grid](https://leetcode.com/problems/check-if-there-is-a-valid-path-in-a-grid)

[中文文档](/solution/1300-1399/1391.Check%20if%20There%20is%20a%20Valid%20Path%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho lưới <code>m x n</code> <code>grid</code>. Mỗi ô trong <code>grid</code> biểu diễn một đoạn đường. Đoạn đường tại <code>grid[i][j]</code> có thể là:</p>

<ul>
	<li><code>1</code> nghĩa là đường nối ô bên trái với ô bên phải.</li>
	<li><code>2</code> nghĩa là đường nối ô bên trên với ô bên dưới.</li>
	<li><code>3</code> nghĩa là đường nối ô bên trái với ô bên dưới.</li>
	<li><code>4</code> nghĩa là đường nối ô bên phải với ô bên dưới.</li>
	<li><code>5</code> nghĩa là đường nối ô bên trái với ô bên trên.</li>
	<li><code>6</code> nghĩa là đường nối ô bên phải với ô bên trên.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1391.Check%20if%20There%20is%20a%20Valid%20Path%20in%20a%20Grid/images/main.png" style="width: 450px; height: 708px;" />
<p>Ban đầu, bạn đứng trên đoạn đường ở ô trên cùng bên trái <code>(0, 0)</code>. Đường đi hợp lệ trong lưới bắt đầu từ ô trên cùng bên trái <code>(0, 0)</code> và kết thúc tại ô dưới cùng bên phải <code>(m - 1, n - 1)</code>. <strong>Đường đi chỉ được men theo các đoạn đường</strong>.</p>

<p><strong>Lưu ý</strong> rằng bạn <strong>không được phép</strong> thay đổi bất kỳ đoạn đường nào.</p>

<p>Trả về <code>true</code><em> nếu có đường đi hợp lệ trong lưới, nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1391.Check%20if%20There%20is%20a%20Valid%20Path%20in%20a%20Grid/images/e1.png" style="width: 455px; height: 311px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[2,4,3],[6,5,2]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Như hình minh họa, bạn có thể bắt đầu tại ô (0, 0) và đi qua các ô trong lưới để đến (m - 1, n - 1).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1391.Check%20if%20There%20is%20a%20Valid%20Path%20in%20a%20Grid/images/e2.png" style="width: 455px; height: 293px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2,1],[1,2,1]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Như hình minh họa, đoạn đường ở ô (0, 0) không nối với đoạn đường nào ở các ô khác, nên bạn sẽ bị mắc kẹt tại ô (0, 0).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,1,2]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Bạn sẽ bị mắc kẹt tại ô (0, 1) và không thể đến ô (0, 2).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 6</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi trong sáu loại đường nối một cặp cạnh nhất định; ta cần xác định có thể đi từ ô trên cùng bên trái đến ô dưới cùng bên phải hay không. Xem mỗi ô là một node; chỉ nối hai node khi hai đoạn đường tương ứng thông nhau. Union-find gộp các node kề nhau có thể đi qua; ô đầu và ô cuối phải thuộc cùng một component.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasValidPath(self, grid: List[List[int]]) -> bool:
        m, n = len(grid), len(grid[0])
        p = list(range(m * n))

        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        def left(i, j):
            if j > 0 and grid[i][j - 1] in (1, 4, 6):
                p[find(i * n + j)] = find(i * n + j - 1)

        def right(i, j):
            if j < n - 1 and grid[i][j + 1] in (1, 3, 5):
                p[find(i * n + j)] = find(i * n + j + 1)

        def up(i, j):
            if i > 0 and grid[i - 1][j] in (2, 3, 4):
                p[find(i * n + j)] = find((i - 1) * n + j)

        def down(i, j):
            if i < m - 1 and grid[i + 1][j] in (2, 5, 6):
                p[find(i * n + j)] = find((i + 1) * n + j)

        for i in range(m):
            for j in range(n):
                e = grid[i][j]
                if e == 1:
                    left(i, j)
                    right(i, j)
                elif e == 2:
                    up(i, j)
                    down(i, j)
                elif e == 3:
                    left(i, j)
                    down(i, j)
                elif e == 4:
                    right(i, j)
                    down(i, j)
                elif e == 5:
                    left(i, j)
                    up(i, j)
                else:
                    right(i, j)
                    up(i, j)
        return find(0) == find(m * n - 1)
```

#### Java

```java
class Solution {
    private int[] p;
    private int[][] grid;
    private int m;
    private int n;

    public boolean hasValidPath(int[][] grid) {
        this.grid = grid;
        m = grid.length;
        n = grid[0].length;
        p = new int[m * n];
        for (int i = 0; i < p.length; ++i) {
            p[i] = i;
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int e = grid[i][j];
                if (e == 1) {
                    left(i, j);
                    right(i, j);
                } else if (e == 2) {
                    up(i, j);
                    down(i, j);
                } else if (e == 3) {
                    left(i, j);
                    down(i, j);
                } else if (e == 4) {
                    right(i, j);
                    down(i, j);
                } else if (e == 5) {
                    left(i, j);
                    up(i, j);
                } else {
                    right(i, j);
                    up(i, j);
                }
            }
        }
        return find(0) == find(m * n - 1);
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    private void left(int i, int j) {
        if (j > 0 && (grid[i][j - 1] == 1 || grid[i][j - 1] == 4 || grid[i][j - 1] == 6)) {
            p[find(i * n + j)] = find(i * n + j - 1);
        }
    }

    private void right(int i, int j) {
        if (j < n - 1 && (grid[i][j + 1] == 1 || grid[i][j + 1] == 3 || grid[i][j + 1] == 5)) {
            p[find(i * n + j)] = find(i * n + j + 1);
        }
    }

    private void up(int i, int j) {
        if (i > 0 && (grid[i - 1][j] == 2 || grid[i - 1][j] == 3 || grid[i - 1][j] == 4)) {
            p[find(i * n + j)] = find((i - 1) * n + j);
        }
    }

    private void down(int i, int j) {
        if (i < m - 1 && (grid[i + 1][j] == 2 || grid[i + 1][j] == 5 || grid[i + 1][j] == 6)) {
            p[find(i * n + j)] = find((i + 1) * n + j);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> p;

    bool hasValidPath(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        p.resize(m * n);
        for (int i = 0; i < p.size(); ++i) p[i] = i;
        auto left = [&](int i, int j) {
            if (j > 0 && (grid[i][j - 1] == 1 || grid[i][j - 1] == 4 || grid[i][j - 1] == 6)) {
                p[find(i * n + j)] = find(i * n + j - 1);
            }
        };
        auto right = [&](int i, int j) {
            if (j < n - 1 && (grid[i][j + 1] == 1 || grid[i][j + 1] == 3 || grid[i][j + 1] == 5)) {
                p[find(i * n + j)] = find(i * n + j + 1);
            }
        };
        auto up = [&](int i, int j) {
            if (i > 0 && (grid[i - 1][j] == 2 || grid[i - 1][j] == 3 || grid[i - 1][j] == 4)) {
                p[find(i * n + j)] = find((i - 1) * n + j);
            }
        };
        auto down = [&](int i, int j) {
            if (i < m - 1 && (grid[i + 1][j] == 2 || grid[i + 1][j] == 5 || grid[i + 1][j] == 6)) {
                p[find(i * n + j)] = find((i + 1) * n + j);
            }
        };
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int e = grid[i][j];
                if (e == 1) {
                    left(i, j);
                    right(i, j);
                } else if (e == 2) {
                    up(i, j);
                    down(i, j);
                } else if (e == 3) {
                    left(i, j);
                    down(i, j);
                } else if (e == 4) {
                    right(i, j);
                    down(i, j);
                } else if (e == 5) {
                    left(i, j);
                    up(i, j);
                } else {
                    right(i, j);
                    up(i, j);
                }
            }
        }
        return find(0) == find(m * n - 1);
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
};
```

#### Go

```go
func hasValidPath(grid [][]int) bool {
	m, n := len(grid), len(grid[0])
	p := make([]int, m*n)
	for i := range p {
		p[i] = i
	}
	var find func(x int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	left := func(i, j int) {
		if j > 0 && (grid[i][j-1] == 1 || grid[i][j-1] == 4 || grid[i][j-1] == 6) {
			p[find(i*n+j)] = find(i*n + j - 1)
		}
	}
	right := func(i, j int) {
		if j < n-1 && (grid[i][j+1] == 1 || grid[i][j+1] == 3 || grid[i][j+1] == 5) {
			p[find(i*n+j)] = find(i*n + j + 1)
		}
	}
	up := func(i, j int) {
		if i > 0 && (grid[i-1][j] == 2 || grid[i-1][j] == 3 || grid[i-1][j] == 4) {
			p[find(i*n+j)] = find((i-1)*n + j)
		}
	}
	down := func(i, j int) {
		if i < m-1 && (grid[i+1][j] == 2 || grid[i+1][j] == 5 || grid[i+1][j] == 6) {
			p[find(i*n+j)] = find((i+1)*n + j)
		}
	}
	for i, row := range grid {
		for j, e := range row {
			if e == 1 {
				left(i, j)
				right(i, j)
			} else if e == 2 {
				up(i, j)
				down(i, j)
			} else if e == 3 {
				left(i, j)
				down(i, j)
			} else if e == 4 {
				right(i, j)
				down(i, j)
			} else if e == 5 {
				left(i, j)
				up(i, j)
			} else {
				right(i, j)
				up(i, j)
			}
		}
	}
	return find(0) == find(m*n-1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
