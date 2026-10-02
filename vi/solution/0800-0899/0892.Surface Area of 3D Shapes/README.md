---
comments: true
difficulty: Easy
tags:
    - Geometry
    - Array
    - Math
    - Matrix
---

<!-- problem:start -->

# [892. Surface Area of 3D Shapes](https://leetcode.com/problems/surface-area-of-3d-shapes)

[中文文档](/solution/0800-0899/0892.Surface%20Area%20of%203D%20Shapes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho lưới <code>n x n</code> tên <code>grid</code>, trong đó bạn đã đặt một số khối lập phương <code>1 x 1 x 1</code>. Mỗi giá trị <code>v = grid[i][j]</code> biểu diễn một tháp gồm <code>v</code> khối lập phương xếp chồng lên ô <code>(i, j)</code>.</p>

<p>Sau khi xếp các khối lập phương này, bạn dán những khối kề trực tiếp với nhau, tạo thành một số hình khối 3D không đều.</p>

<p>Trả về <em>tổng diện tích bề mặt của các hình khối thu được</em>.</p>

<p><strong>Lưu ý:</strong> Mặt đáy của mỗi hình khối cũng được tính vào diện tích bề mặt.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0892.Surface%20Area%20of%203D%20Shapes/images/tmp-grid2.jpg" style="width: 162px; height: 162px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2],[3,4]]
<strong>Đầu ra:</strong> 34
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0892.Surface%20Area%20of%203D%20Shapes/images/tmp-grid4.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[1,0,1],[1,1,1]]
<strong>Đầu ra:</strong> 32
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0892.Surface%20Area%20of%203D%20Shapes/images/tmp-grid5.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[2,2,2],[2,1,2],[2,2,2]]
<strong>Đầu ra:</strong> 46
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length == grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Diện tích bề mặt là tổng diện tích các mặt khối lập phương không bị che. Vì $n\le 50$, mỗi ô không rỗng đóng góp $2+4v$, sau đó ta trừ diện tích các mặt tiếp giáp với ô phía bắc và phía tây.
>
> Diện tích phần tiếp giáp là $2\cdot\min(v,\textit{neighbor})$. Ô trống không đóng góp diện tích.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def surfaceArea(self, grid: List[List[int]]) -> int:
        ans = 0
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                if v:
                    ans += 2 + v * 4
                    if i:
                        ans -= min(v, grid[i - 1][j]) * 2
                    if j:
                        ans -= min(v, grid[i][j - 1]) * 2
        return ans
```

#### Java

```java
class Solution {
    public int surfaceArea(int[][] grid) {
        int n = grid.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] > 0) {
                    ans += 2 + grid[i][j] * 4;
                    if (i > 0) {
                        ans -= Math.min(grid[i][j], grid[i - 1][j]) * 2;
                    }
                    if (j > 0) {
                        ans -= Math.min(grid[i][j], grid[i][j - 1]) * 2;
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
    int surfaceArea(vector<vector<int>>& grid) {
        int n = grid.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    ans += 2 + grid[i][j] * 4;
                    if (i) ans -= min(grid[i][j], grid[i - 1][j]) * 2;
                    if (j) ans -= min(grid[i][j], grid[i][j - 1]) * 2;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func surfaceArea(grid [][]int) int {
	ans := 0
	for i, row := range grid {
		for j, v := range row {
			if v > 0 {
				ans += 2 + v*4
				if i > 0 {
					ans -= min(v, grid[i-1][j]) * 2
				}
				if j > 0 {
					ans -= min(v, grid[i][j-1]) * 2
				}
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
