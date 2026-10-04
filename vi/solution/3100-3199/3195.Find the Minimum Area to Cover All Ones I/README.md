---
comments: true
difficulty: Medium
rating: 1348
source: Weekly Contest 403 Q2
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [3195. Find the Minimum Area to Cover All Ones I](https://leetcode.com/problems/find-the-minimum-area-to-cover-all-ones-i)

[中文文档](/solution/3100-3199/3195.Find%20the%20Minimum%20Area%20to%20Cover%20All%20Ones%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>nhị phân</strong> 2D <code>grid</code>. Hãy tìm một hình chữ nhật có các cạnh nằm ngang và dọc với <strong>diện tích nhỏ nhất</strong>, sao cho tất cả các số 1 trong <code>grid</code> đều nằm bên trong hình chữ nhật này.</p>

<p>Trả về <strong>diện tích nhỏ nhất</strong> có thể có của hình chữ nhật.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0,1,0],[1,0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3195.Find%20the%20Minimum%20Area%20to%20Cover%20All%20Ones%20I/images/examplerect0.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 279px; height: 198px;" /></p>

<p>Hình chữ nhật nhỏ nhất có chiều cao bằng 2 và chiều rộng bằng 3, nên diện tích là <code>2 * 3 = 6</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,0],[0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3195.Find%20the%20Minimum%20Area%20to%20Cover%20All%20Ones%20I/images/examplerect1.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 204px; height: 201px;" /></p>

<p>Hình chữ nhật nhỏ nhất có cả chiều cao và chiều rộng bằng 1, nên diện tích là <code>1 * 1 = 1</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= grid.length, grid[i].length &lt;= 1000</code></li>
    <li><code>grid[i][j]</code> chỉ có thể là 0 hoặc 1.</li>
    <li>Dữ liệu đầu vào được tạo sao cho <code>grid</code> có ít nhất một số 1.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm các biên nhỏ nhất và lớn nhất

<!-- thinking:start -->

> **Tư duy**
>
> Một hình chữ nhật có các cạnh song song với các trục phải bao phủ mọi số $1$. Nếu thử mọi hình chữ nhật, độ phức tạp sẽ là $O(m^2n^2)$.
>
> Hình chữ nhật tối ưu chính là bounding box của tất cả các số 1, được xác định bởi các hàng và cột ngoài cùng.
>
> Chỉ cần duyệt một lần để theo dõi $x_1,y_1,x_2,y_2$; diện tích là $(x_2-x_1+1)(y_2-y_1+1)$.

<!-- thinking:end -->

Ta có thể duyệt qua `grid`, tìm biên nhỏ nhất của tất cả các số `1`, ký hiệu là $(x_1, y_1)$, và biên lớn nhất, ký hiệu là $(x_2, y_2)$. Khi đó, diện tích của hình chữ nhật nhỏ nhất là $(x_2 - x_1 + 1) \times (y_2 - y_1 + 1)$.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của `grid`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumArea(self, grid: List[List[int]]) -> int:
        x1 = y1 = inf
        x2 = y2 = -inf
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if x == 1:
                    x1 = min(x1, i)
                    y1 = min(y1, j)
                    x2 = max(x2, i)
                    y2 = max(y2, j)
        return (x2 - x1 + 1) * (y2 - y1 + 1)
```

#### Java

```java
class Solution {
    public int minimumArea(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int x1 = m, y1 = n;
        int x2 = 0, y2 = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    x1 = Math.min(x1, i);
                    y1 = Math.min(y1, j);
                    x2 = Math.max(x2, i);
                    y2 = Math.max(y2, j);
                }
            }
        }
        return (x2 - x1 + 1) * (y2 - y1 + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumArea(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int x1 = m, y1 = n;
        int x2 = 0, y2 = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    x1 = min(x1, i);
                    y1 = min(y1, j);
                    x2 = max(x2, i);
                    y2 = max(y2, j);
                }
            }
        }
        return (x2 - x1 + 1) * (y2 - y1 + 1);
    }
};
```

#### Go

```go
func minimumArea(grid [][]int) int {
    x1, y1 := len(grid), len(grid[0])
    x2, y2 := 0, 0
    for i, row := range grid {
        for j, x := range row {
            if x == 1 {
                x1, y1 = min(x1, i), min(y1, j)
                x2, y2 = max(x2, i), max(y2, j)
            }
        }
    }
    return (x2 - x1 + 1) * (y2 - y1 + 1)
}
```

#### TypeScript

```ts
function minimumArea(grid: number[][]): number {
    const [m, n] = [grid.length, grid[0].length];
    let [x1, y1] = [m, n];
    let [x2, y2] = [0, 0];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 1) {
                x1 = Math.min(x1, i);
                y1 = Math.min(y1, j);
                x2 = Math.max(x2, i);
                y2 = Math.max(y2, j);
            }
        }
    }
    return (x2 - x1 + 1) * (y2 - y1 + 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_area(grid: Vec<Vec<i32>>) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let mut x1 = m as i32;
        let mut y1 = n as i32;
        let mut x2 = 0i32;
        let mut y2 = 0i32;

        for i in 0..m {
            for j in 0..n {
                if grid[i][j] == 1 {
                    x1 = x1.min(i as i32);
                    y1 = y1.min(j as i32);
                    x2 = x2.max(i as i32);
                    y2 = y2.max(j as i32);
                }
            }
        }

        (x2 - x1 + 1) * (y2 - y1 + 1)
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var minimumArea = function (grid) {
    const [m, n] = [grid.length, grid[0].length];
    let [x1, y1] = [m, n];
    let [x2, y2] = [0, 0];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 1) {
                x1 = Math.min(x1, i);
                y1 = Math.min(y1, j);
                x2 = Math.max(x2, i);
                y2 = Math.max(y2, j);
            }
        }
    }
    return (x2 - x1 + 1) * (y2 - y1 + 1);
};
```

#### C#

```cs
public class Solution {
    public int MinimumArea(int[][] grid) {
        int m = grid.Length, n = grid[0].Length;
        int x1 = m, y1 = n;
        int x2 = 0, y2 = 0;

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 1) {
                    x1 = Math.Min(x1, i);
                    y1 = Math.Min(y1, j);
                    x2 = Math.Max(x2, i);
                    y2 = Math.Max(y2, j);
                }
            }
        }

        return (x2 - x1 + 1) * (y2 - y1 + 1);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
