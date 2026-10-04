---
comments: true
difficulty: Easy
rating: 1234
source: Weekly Contest 462 Q1
tags:
    - Array
    - Two Pointers
    - Matrix
---

<!-- problem:start -->

# [3643. Flip Square Submatrix Vertically](https://leetcode.com/problems/flip-square-submatrix-vertically)

[中文文档](/solution/3600-3699/3643.Flip%20Square%20Submatrix%20Vertically/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>m x n</code> là <code>grid</code>, cùng ba số nguyên <code>x</code>, <code>y</code> và <code>k</code>.</p>

<p>Các số nguyên <code>x</code> và <code>y</code> biểu diễn chỉ số hàng và cột của góc <strong>trên bên trái</strong> của một ma trận con <strong>hình vuông</strong>, còn số nguyên <code>k</code> biểu diễn kích thước (độ dài cạnh) của ma trận con hình vuông đó.</p>

<p>Nhiệm vụ của bạn là lật ma trận con bằng cách đảo ngược thứ tự các hàng theo chiều dọc.</p>

<p>Trả về ma trận sau khi cập nhật.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3643.Flip%20Square%20Submatrix%20Vertically/images/gridexmdrawio.png" style="width: 300px; height: 116px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = </span>[[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]]<span class="example-io">, x = 1, y = 0, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,2,3,4],[13,14,15,8],[9,10,11,12],[5,6,7,16]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sơ đồ phía trên minh họa grid trước và sau khi biến đổi.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3643.Flip%20Square%20Submatrix%20Vertically/images/gridexm2drawio.png" style="width: 350px; height: 68px;" />​​​​​​
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[3,4,2,3],[2,3,4,2]], x = 0, y = 2, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[3,4,4,2],[2,3,2,3]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sơ đồ phía trên minh họa grid trước và sau khi biến đổi.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 100</code></li>
	<li><code>0 &lt;= x &lt; m</code></li>
	<li><code>0 &lt;= y &lt; n</code></li>
	<li><code>1 &lt;= k &lt;= min(m - x, n - y)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Lật hình vuông $k\times k$ có góc trên bên trái là $(x,y)$ qua đường trung trực nằm ngang. Vì $k\le 50$, ta có thể hoán đổi trực tiếp ngay trên ma trận.
>
> Với $i\in [x,x+\lfloor k/2\rfloor)$, hoán đổi đoạn $[y,y+k)$ với hàng $x+k-1-(i-x)$.
>
> Các cột không thay đổi, nên vòng lặp bên trong luôn nằm trong hình vuông.

<!-- thinking:end -->

Bắt đầu từ hàng $x$ và lật tổng cộng $\lfloor \frac{k}{2} \rfloor$ hàng.

Với mỗi hàng $i$, ta cần hoán đổi nó với hàng tương ứng $i_2$, trong đó $i_2 = x + k - 1 - (i - x)$.

Trong quá trình hoán đổi, ta cần duyệt $j \in [y, y + k)$ và hoán đổi $\text{grid}[i][j]$ với $\text{grid}[i_2][j]$.

Cuối cùng, trả về ma trận sau khi cập nhật.

Độ phức tạp thời gian là $O(k^2)$, trong đó $k$ là độ dài cạnh của ma trận con. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseSubmatrix(
        self, grid: List[List[int]], x: int, y: int, k: int
    ) -> List[List[int]]:
        for i in range(x, x + k // 2):
            i2 = x + k - 1 - (i - x)
            for j in range(y, y + k):
                grid[i][j], grid[i2][j] = grid[i2][j], grid[i][j]
        return grid
```

#### Java

```java
class Solution {
    public int[][] reverseSubmatrix(int[][] grid, int x, int y, int k) {
        for (int i = x; i < x + k / 2; i++) {
            int i2 = x + k - 1 - (i - x);
            for (int j = y; j < y + k; j++) {
                int t = grid[i][j];
                grid[i][j] = grid[i2][j];
                grid[i2][j] = t;
            }
        }
        return grid;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> reverseSubmatrix(vector<vector<int>>& grid, int x, int y, int k) {
        for (int i = x; i < x + k / 2; i++) {
            int i2 = x + k - 1 - (i - x);
            for (int j = y; j < y + k; j++) {
                swap(grid[i][j], grid[i2][j]);
            }
        }
        return grid;
    }
};
```

#### Go

```go
func reverseSubmatrix(grid [][]int, x int, y int, k int) [][]int {
	for i := x; i < x+k/2; i++ {
		i2 := x + k - 1 - (i - x)
		for j := y; j < y+k; j++ {
			grid[i][j], grid[i2][j] = grid[i2][j], grid[i][j]
		}
	}
	return grid
}
```

#### TypeScript

```ts
function reverseSubmatrix(grid: number[][], x: number, y: number, k: number): number[][] {
    for (let i = x; i < x + Math.floor(k / 2); i++) {
        const i2 = x + k - 1 - (i - x);
        for (let j = y; j < y + k; j++) {
            [grid[i][j], grid[i2][j]] = [grid[i2][j], grid[i][j]];
        }
    }
    return grid;
}
```

#### Rust

```rust
impl Solution {
    pub fn reverse_submatrix(
        mut grid: Vec<Vec<i32>>,
        x: i32,
        y: i32,
        k: i32,
    ) -> Vec<Vec<i32>> {
        let x = x as usize;
        let y = y as usize;
        let k = k as usize;

        for i in x..(x + k / 2) {
            let i2 = x + k - 1 - (i - x);
            for j in y..(y + k) {
                let t = grid[i][j];
                grid[i][j] = grid[i2][j];
                grid[i2][j] = t;
            }
        }

        grid
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
