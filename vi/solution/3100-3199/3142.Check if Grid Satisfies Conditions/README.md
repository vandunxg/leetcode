---
comments: true
difficulty: Easy
rating: 1303
source: Biweekly Contest 130 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [3142. Check if Grid Satisfies Conditions](https://leetcode.com/problems/check-if-grid-satisfies-conditions)

[Tài liệu tiếng Trung](/solution/3100-3199/3142.Check%20if%20Grid%20Satisfies%20Conditions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận 2D <code>grid</code> có kích thước <code>m x n</code>. Bạn cần kiểm tra xem mỗi ô <code>grid[i][j]</code> có:</p>

<ul>
	<li>Bằng ô ngay bên dưới, tức là <code>grid[i][j] == grid[i + 1][j]</code> (nếu ô đó tồn tại).</li>
	<li>Khác ô ngay bên phải, tức là <code>grid[i][j] != grid[i][j + 1]</code> (nếu ô đó tồn tại).</li>
</ul>

<p>Trả về <code>true</code> nếu <strong>tất cả</strong> các ô đều thỏa mãn những điều kiện này, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,0,2],[1,0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3142.Check%20if%20Grid%20Satisfies%20Conditions/images/examplechanged.png" style="width: 254px; height: 186px;padding: 10px; background: #fff; border-radius: .5rem;" /></strong></p>

<p>Tất cả các ô trong ma trận đều thỏa mãn các điều kiện.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,1,1],[0,0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3142.Check%20if%20Grid%20Satisfies%20Conditions/images/example21.png" style="width: 254px; height: 186px;padding: 10px; background: #fff; border-radius: .5rem;" /></strong></p>

<p>Tất cả các ô trong hàng đầu tiên đều bằng nhau.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1],[2],[3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3142.Check%20if%20Grid%20Satisfies%20Conditions/images/changed.png" style="width: 86px; height: 277px;padding: 10px; background: #fff; border-radius: .5rem;" /></p>

<p>Các ô trong cột đầu tiên có các giá trị khác nhau.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, m &lt;= 10</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các ô phải bằng ô bên dưới và khác ô bên phải. Bản chất đề bài đã là kiểm tra cục bộ.
>
> Một vi phạm là đủ để loại ma trận, vì vậy có thể dừng duyệt sớm.
>
> Duyệt qua mọi ô, so sánh ô đó với ô bên dưới và bên phải, rồi chỉ trả về true khi mọi cặp đều tuân theo quy tắc.

<!-- thinking:end -->

Ta có thể duyệt qua từng ô và kiểm tra xem ô đó có thỏa mãn các điều kiện được nêu trong đề bài hay không. Nếu có một ô không thỏa mãn điều kiện, ta trả về `false`; ngược lại, ta trả về `true`.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận `grid` tương ứng. Độ phức tạp không gian là $O(1)`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def satisfiesConditions(self, grid: List[List[int]]) -> bool:
        m, n = len(grid), len(grid[0])
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if i + 1 < m and x != grid[i + 1][j]:
                    return False
                if j + 1 < n and x == grid[i][j + 1]:
                    return False
        return True
```

#### Java

```java
class Solution {
    public boolean satisfiesConditions(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i + 1 < m && grid[i][j] != grid[i + 1][j]) {
                    return false;
                }
                if (j + 1 < n && grid[i][j] == grid[i][j + 1]) {
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
    bool satisfiesConditions(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i + 1 < m && grid[i][j] != grid[i + 1][j]) {
                    return false;
                }
                if (j + 1 < n && grid[i][j] == grid[i][j + 1]) {
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
func satisfiesConditions(grid [][]int) bool {
	m, n := len(grid), len(grid[0])
	for i, row := range grid {
		for j, x := range row {
			if i+1 < m && x != grid[i+1][j] {
				return false
			}
			if j+1 < n && x == grid[i][j+1] {
				return false
			}
		}
	}
	return true
}
```

#### TypeScript

```ts
function satisfiesConditions(grid: number[][]): boolean {
    const [m, n] = [grid.length, grid[0].length];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (i + 1 < m && grid[i][j] !== grid[i + 1][j]) {
                return false;
            }
            if (j + 1 < n && grid[i][j] === grid[i][j + 1]) {
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
