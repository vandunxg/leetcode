---
comments: true
difficulty: Easy
rating: 1200
source: Weekly Contest 299 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [2319. Check if Matrix Is X-Matrix](https://leetcode.com/problems/check-if-matrix-is-x-matrix)

[中文文档](/solution/2300-2399/2319.Check%20if%20Matrix%20Is%20X-Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Một ma trận vuông được gọi là <strong>X-Matrix</strong> nếu <strong>đồng thời</strong> thỏa mãn cả hai điều kiện sau:</p>

<ol>
	<li>Tất cả phần tử trên các đường chéo của ma trận đều <strong>khác 0</strong>.</li>
	<li>Tất cả phần tử còn lại đều bằng 0.</li>
</ol>

<p>Cho một mảng số nguyên 2D <code>grid</code> có kích thước <code>n x n</code>, biểu diễn một ma trận vuông. Hãy trả về <code>true</code><em> nếu </em><code>grid</code><em> là một X-Matrix</em>. Ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2319.Check%20if%20Matrix%20Is%20X-Matrix/images/ex1.jpg" style="width: 311px; height: 320px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[2,0,0,1],[0,3,1,0],[0,5,2,0],[4,0,0,2]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Tham khảo sơ đồ ở trên.
Một X-Matrix phải có các phần tử màu xanh (các đường chéo) khác 0 và các phần tử màu đỏ bằng 0.
Do đó, grid là một X-Matrix.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2319.Check%20if%20Matrix%20Is%20X-Matrix/images/ex2.jpg" style="width: 238px; height: 246px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[5,7,0],[0,3,1],[0,5,0]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Tham khảo sơ đồ ở trên.
Một X-Matrix phải có các phần tử màu xanh (các đường chéo) khác 0 và các phần tử màu đỏ bằng 0.
Do đó, grid không phải là một X-Matrix.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>3 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một X-Matrix cần có các phần tử khác 0 trên hai đường chéo và các phần tử còn lại bằng 0. Vì $n \le 100$, ta chỉ cần duyệt toàn bộ ma trận để kiểm tra.
>
> Nếu $i=j$ hoặc $i+j=n-1$ thì từ chối phần tử bằng 0; ngược lại, từ chối phần tử khác 0. Trả về ngay khi phát hiện một ô không hợp lệ; không cần thêm cấu trúc dữ liệu nào.

<!-- thinking:end -->

Ta có thể duyệt trực tiếp ma trận và kiểm tra xem mỗi phần tử có thỏa mãn các điều kiện của một ma trận $X$ hay không. Nếu có phần tử không thỏa mãn điều kiện, lập tức trả về $\textit{false}$. Nếu tất cả phần tử đều thỏa mãn điều kiện sau khi duyệt xong, trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là số hàng hoặc số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkXMatrix(self, grid: List[List[int]]) -> bool:
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                if i == j or i + j == len(grid) - 1:
                    if v == 0:
                        return False
                elif v:
                    return False
        return True
```

#### Java

```java
class Solution {
    public boolean checkXMatrix(int[][] grid) {
        int n = grid.length;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i == j || i + j == n - 1) {
                    if (grid[i][j] == 0) {
                        return false;
                    }
                } else if (grid[i][j] != 0) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkXMatrix(vector<vector<int>>& grid) {
        int n = grid.size();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i == j || i + j == n - 1) {
                    if (!grid[i][j]) {
                        return false;
                    }
                } else if (grid[i][j]) {
                    return false;
                }
            }
        }
        return true;
    }
};
```

#### Go

```go
func checkXMatrix(grid [][]int) bool {
	for i, row := range grid {
		for j, v := range row {
			if i == j || i+j == len(row)-1 {
				if v == 0 {
					return false
				}
			} else if v != 0 {
				return false
			}
		}
	}
	return true
}
```

#### TypeScript

```ts
function checkXMatrix(grid: number[][]): boolean {
    const n = grid.length;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (i == j || i + j == n - 1) {
                if (!grid[i][j]) {
                    return false;
                }
            } else if (grid[i][j]) {
                return false;
            }
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn check_x_matrix(grid: Vec<Vec<i32>>) -> bool {
        let n = grid.len();
        for i in 0..n {
            for j in 0..n {
                if i == j || i + j == n - 1 {
                    if grid[i][j] == 0 {
                        return false;
                    }
                } else if grid[i][j] != 0 {
                    return false;
                }
            }
        }
        true
    }
}
```

#### C#

```cs
public class Solution {
    public bool CheckXMatrix(int[][] grid) {
        int n = grid.Length;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i == j || i + j == n - 1) {
                    if (grid[i][j] == 0) {
                        return false;
                    }
                } else if (grid[i][j] != 0) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

#### C

```c
bool checkXMatrix(int** grid, int gridSize, int* gridColSize) {
    for (int i = 0; i < gridSize; i++) {
        for (int j = 0; j < gridSize; j++) {
            if (i == j || i + j == gridSize - 1) {
                if (grid[i][j] == 0) {
                    return false;
                }
            } else if (grid[i][j] != 0) {
                return false;
            }
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
