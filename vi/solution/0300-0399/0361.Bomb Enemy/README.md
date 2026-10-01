---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [361. Bomb Enemy 🔒](https://leetcode.com/problems/bomb-enemy)

[中文文档](/solution/0300-0399/0361.Bomb%20Enemy/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> <code>grid</code>, trong đó mỗi ô là tường <code>&#39;W&#39;</code>, kẻ địch <code>&#39;E&#39;</code> hoặc ô trống <code>&#39;0&#39;</code>. Hãy trả về <em>số kẻ địch tối đa có thể tiêu diệt bằng một quả bom</em>. Chỉ có thể đặt bom vào ô trống.</p>

<p>Bom tiêu diệt tất cả kẻ địch trên cùng hàng và cột tính từ vị trí đặt bom cho đến khi gặp tường, vì bom không thể phá hủy tường.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0361.Bomb%20Enemy/images/bomb1-grid.jpg" style="width: 600px; height: 187px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[&quot;0&quot;,&quot;E&quot;,&quot;0&quot;,&quot;0&quot;],[&quot;E&quot;,&quot;0&quot;,&quot;W&quot;,&quot;E&quot;],[&quot;0&quot;,&quot;E&quot;,&quot;0&quot;,&quot;0&quot;]]
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0361.Bomb%20Enemy/images/bomb2-grid.jpg" style="width: 500px; height: 194px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[&quot;W&quot;,&quot;W&quot;,&quot;W&quot;],[&quot;0&quot;,&quot;0&quot;,&quot;0&quot;],[&quot;E&quot;,&quot;E&quot;,&quot;E&quot;]]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>grid[i][j]</code> là <code>&#39;W&#39;</code>, <code>&#39;E&#39;</code> hoặc <code>&#39;0&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bom đặt ở ô trống sẽ tiêu diệt kẻ địch theo bốn hướng cho đến khi gặp tường. Nếu quét từ từng ô trống, độ phức tạp là $O(mn(m+n))$. Các ô nằm trong cùng một đoạn không bị tường ngăn cách có chung số lượng kẻ địch.
>
> Quét từng hàng theo cả hai chiều, rồi quét từng cột theo cả hai chiều; cộng số kẻ địch trong đoạn hiện tại vào mỗi ô. Tổng bốn hướng tại ô trống là số kẻ địch bị tiêu diệt. Lấy giá trị lớn nhất, hoặc trả về $0$ nếu không có ô trống.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxKilledEnemies(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        g = [[0] * n for _ in range(m)]
        for i in range(m):
            t = 0
            for j in range(n):
                if grid[i][j] == 'W':
                    t = 0
                elif grid[i][j] == 'E':
                    t += 1
                g[i][j] += t
            t = 0
            for j in range(n - 1, -1, -1):
                if grid[i][j] == 'W':
                    t = 0
                elif grid[i][j] == 'E':
                    t += 1
                g[i][j] += t
        for j in range(n):
            t = 0
            for i in range(m):
                if grid[i][j] == 'W':
                    t = 0
                elif grid[i][j] == 'E':
                    t += 1
                g[i][j] += t
            t = 0
            for i in range(m - 1, -1, -1):
                if grid[i][j] == 'W':
                    t = 0
                elif grid[i][j] == 'E':
                    t += 1
                g[i][j] += t
        return max(
            [g[i][j] for i in range(m) for j in range(n) if grid[i][j] == '0'],
            default=0,
        )
```

#### Java

```java
class Solution {
    public int maxKilledEnemies(char[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[][] g = new int[m][n];
        for (int i = 0; i < m; ++i) {
            int t = 0;
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 'W') {
                    t = 0;
                } else if (grid[i][j] == 'E') {
                    ++t;
                }
                g[i][j] += t;
            }
            t = 0;
            for (int j = n - 1; j >= 0; --j) {
                if (grid[i][j] == 'W') {
                    t = 0;
                } else if (grid[i][j] == 'E') {
                    ++t;
                }
                g[i][j] += t;
            }
        }
        for (int j = 0; j < n; ++j) {
            int t = 0;
            for (int i = 0; i < m; ++i) {
                if (grid[i][j] == 'W') {
                    t = 0;
                } else if (grid[i][j] == 'E') {
                    ++t;
                }
                g[i][j] += t;
            }
            t = 0;
            for (int i = m - 1; i >= 0; --i) {
                if (grid[i][j] == 'W') {
                    t = 0;
                } else if (grid[i][j] == 'E') {
                    ++t;
                }
                g[i][j] += t;
            }
        }
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '0') {
                    ans = Math.max(ans, g[i][j]);
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
    int maxKilledEnemies(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> g(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            int t = 0;
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 'W')
                    t = 0;
                else if (grid[i][j] == 'E')
                    ++t;
                g[i][j] += t;
            }
            t = 0;
            for (int j = n - 1; j >= 0; --j) {
                if (grid[i][j] == 'W')
                    t = 0;
                else if (grid[i][j] == 'E')
                    ++t;
                g[i][j] += t;
            }
        }
        for (int j = 0; j < n; ++j) {
            int t = 0;
            for (int i = 0; i < m; ++i) {
                if (grid[i][j] == 'W')
                    t = 0;
                else if (grid[i][j] == 'E')
                    ++t;
                g[i][j] += t;
            }
            t = 0;
            for (int i = m - 1; i >= 0; --i) {
                if (grid[i][j] == 'W')
                    t = 0;
                else if (grid[i][j] == 'E')
                    ++t;
                g[i][j] += t;
            }
        }
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '0') ans = max(ans, g[i][j]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxKilledEnemies(grid [][]byte) int {
	m, n := len(grid), len(grid[0])
	g := make([][]int, m)
	for i := range g {
		g[i] = make([]int, n)
	}
	for i := 0; i < m; i++ {
		t := 0
		for j := 0; j < n; j++ {
			if grid[i][j] == 'W' {
				t = 0
			} else if grid[i][j] == 'E' {
				t++
			}
			g[i][j] += t
		}
		t = 0
		for j := n - 1; j >= 0; j-- {
			if grid[i][j] == 'W' {
				t = 0
			} else if grid[i][j] == 'E' {
				t++
			}
			g[i][j] += t
		}
	}
	for j := 0; j < n; j++ {
		t := 0
		for i := 0; i < m; i++ {
			if grid[i][j] == 'W' {
				t = 0
			} else if grid[i][j] == 'E' {
				t++
			}
			g[i][j] += t
		}
		t = 0
		for i := m - 1; i >= 0; i-- {
			if grid[i][j] == 'W' {
				t = 0
			} else if grid[i][j] == 'E' {
				t++
			}
			g[i][j] += t
		}
	}
	ans := 0
	for i, row := range grid {
		for j, v := range row {
			if v == '0' && ans < g[i][j] {
				ans = g[i][j]
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
