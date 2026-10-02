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

# [883. Projection Area of 3D Shapes](https://leetcode.com/problems/projection-area-of-3d-shapes)

[中文文档](/solution/0800-0899/0883.Projection%20Area%20of%203D%20Shapes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>grid</code> kích thước <code>n x n</code>, trong đó ta đặt một số khối lập phương <code>1 x 1 x 1</code> sao cho các cạnh song song với các trục <code>x</code>, <code>y</code> và <code>z</code>.</p>

<p>Mỗi giá trị <code>v = grid[i][j]</code> biểu thị một chồng gồm <code>v</code> khối lập phương đặt trên ô <code>(i, j)</code>.</p>

<p>Ta xét hình chiếu của các khối lập phương lên các mặt phẳng <code>xy</code>, <code>yz</code> và <code>zx</code>.</p>

<p><strong>Hình chiếu</strong> giống như cái bóng, biến hình <strong>3 chiều</strong> thành một hình trên mặt phẳng <strong>2 chiều</strong>. Ta nhìn thấy “bóng” khi quan sát các khối lập phương từ trên xuống, từ phía trước và từ bên cạnh.</p>

<p>Hãy trả về <em>tổng diện tích của cả ba hình chiếu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0883.Projection%20Area%20of%203D%20Shapes/images/shadow.png" style="width: 800px; height: 214px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2],[3,4]]
<strong>Đầu ra:</strong> 17
<strong>Giải thích:</strong> Đây là ba hình chiếu (“bóng”) của hình khối lên các mặt phẳng song song với các trục.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[2]]
<strong>Đầu ra:</strong> 5
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[1,0],[0,2]]
<strong>Đầu ra:</strong> 8
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

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Diện tích ba hình chiếu lần lượt là số ô khác 0, tổng giá trị lớn nhất của từng hàng và tổng giá trị lớn nhất của từng cột. Vì $n\le 50$, một lượt duyệt là đủ để tính cả ba.
>
> Đếm các ô có $v>0$ khi nhìn từ trên xuống, lấy $\max$ trên từng hàng và cột cho hai hướng nhìn còn lại, rồi cộng các kết quả.

<!-- thinking:end -->

Ta có thể tính riêng diện tích của ba hình chiếu.

- Diện tích hình chiếu lên mặt phẳng xy: Mỗi giá trị khác 0 tạo thành một ô được chiếu lên mặt phẳng xy, nên diện tích bằng số lượng giá trị khác 0.
- Diện tích hình chiếu lên mặt phẳng yz: Giá trị lớn nhất của mỗi hàng.
- Diện tích hình chiếu lên mặt phẳng zx: Giá trị lớn nhất của mỗi cột.

Cuối cùng, cộng diện tích của cả ba hình chiếu.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của <code>grid</code>. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def projectionArea(self, grid: List[List[int]]) -> int:
        xy = sum(v > 0 for row in grid for v in row)
        yz = sum(max(row) for row in grid)
        zx = sum(max(col) for col in zip(*grid))
        return xy + yz + zx
```

#### Java

```java
class Solution {
    public int projectionArea(int[][] grid) {
        int xy = 0, yz = 0, zx = 0;
        for (int i = 0, n = grid.length; i < n; ++i) {
            int maxYz = 0;
            int maxZx = 0;
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] > 0) {
                    ++xy;
                }
                maxYz = Math.max(maxYz, grid[i][j]);
                maxZx = Math.max(maxZx, grid[j][i]);
            }
            yz += maxYz;
            zx += maxZx;
        }
        return xy + yz + zx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int projectionArea(vector<vector<int>>& grid) {
        int xy = 0, yz = 0, zx = 0;
        for (int i = 0, n = grid.size(); i < n; ++i) {
            int maxYz = 0, maxZx = 0;
            for (int j = 0; j < n; ++j) {
                xy += grid[i][j] > 0;
                maxYz = max(maxYz, grid[i][j]);
                maxZx = max(maxZx, grid[j][i]);
            }
            yz += maxYz;
            zx += maxZx;
        }
        return xy + yz + zx;
    }
};
```

#### Go

```go
func projectionArea(grid [][]int) int {
	xy, yz, zx := 0, 0, 0
	for i, row := range grid {
		maxYz, maxZx := 0, 0
		for j, v := range row {
			if v > 0 {
				xy++
			}
			maxYz = max(maxYz, v)
			maxZx = max(maxZx, grid[j][i])
		}
		yz += maxYz
		zx += maxZx
	}
	return xy + yz + zx
}
```

#### TypeScript

```ts
function projectionArea(grid: number[][]): number {
    const xy: number = grid.flat().filter(v => v > 0).length;
    const yz: number = grid.reduce((acc, row) => acc + Math.max(...row), 0);
    const zx: number = grid[0]
        .map((_, i) => Math.max(...grid.map(row => row[i])))
        .reduce((acc, val) => acc + val, 0);
    return xy + yz + zx;
}
```

#### Rust

```rust
impl Solution {
    pub fn projection_area(grid: Vec<Vec<i32>>) -> i32 {
        let xy: i32 = grid
            .iter()
            .map(|row| row.iter().filter(|&&v| v > 0).count() as i32)
            .sum();
        let yz: i32 = grid.iter().map(|row| *row.iter().max().unwrap_or(&0)).sum();
        let zx: i32 = (0..grid[0].len())
            .map(|i| grid.iter().map(|row| row[i]).max().unwrap_or(0))
            .sum();
        xy + yz + zx
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
