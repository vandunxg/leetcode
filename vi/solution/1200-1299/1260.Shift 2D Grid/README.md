---
comments: true
difficulty: Easy
rating: 1337
source: Weekly Contest 163 Q1
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [1260. Shift 2D Grid](https://leetcode.com/problems/shift-2d-grid)

[中文文档](/solution/1200-1299/1260.Shift%202D%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận 2D <code>grid</code> kích thước <code>m x n</code>&nbsp;và số nguyên <code>k</code>. Hãy dịch chuyển <code>grid</code>&nbsp;<code>k</code> lần.</p>

<p>Trong một lần dịch chuyển:</p>

<ul>
	<li>Phần tử tại <code>grid[i][j]</code> chuyển đến <code>grid[i][j + 1]</code>.</li>
	<li>Phần tử tại <code>grid[i][n - 1]</code> chuyển đến <code>grid[i + 1][0]</code>.</li>
	<li>Phần tử tại <code>grid[m&nbsp;- 1][n - 1]</code> chuyển đến <code>grid[0][0]</code>.</li>
</ul>

<p>Trả về <em>ma trận 2D</em> sau khi thực hiện thao tác dịch chuyển <code>k</code> lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1260.Shift%202D%20Grid/images/e1.png" style="width: 400px; height: 178px;" />
<pre>
<strong>Đầu vào:</strong> <code>grid</code> = [[1,2,3],[4,5,6],[7,8,9]], k = 1
<strong>Đầu ra:</strong> [[9,1,2],[3,4,5],[6,7,8]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1260.Shift%202D%20Grid/images/e2.png" style="width: 400px; height: 166px;" />
<pre>
<strong>Đầu vào:</strong> <code>grid</code> = [[3,8,1,9],[19,7,2,5],[4,6,11,10],[12,0,21,13]], k = 4
<strong>Đầu ra:</strong> [[12,0,21,13],[3,8,1,9],[19,7,2,5],[4,6,11,10]]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> <code>grid</code> = [[1,2,3],[4,5,6],[7,8,9]], k = 9
<strong>Đầu ra:</strong> [[1,2,3],[4,5,6],[7,8,9]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m ==&nbsp;grid.length</code></li>
	<li><code>n ==&nbsp;grid[i].length</code></li>
	<li><code>1 &lt;= m &lt;= 50</code></li>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>-1000 &lt;= grid[i][j] &lt;= 1000</code></li>
	<li><code>0 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Làm phẳng mảng 2D

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần dịch chuyển đưa ô cuối lên đầu và đẩy các ô còn lại sang phải; tương đương xoay phải mảng 1D thu được khi làm phẳng ma trận theo thứ tự từng hàng đi $k$ vị trí. Vì $m,n \le 50$, ta tính chỉ số cuối của từng phần tử thay vì xoay $k$ lần.
>
> Cộng $k$ vào chỉ số phẳng $i\cdot n+j$ rồi lấy modulo $mn$ sẽ cho ta vị trí hàng và cột tương ứng trong ma trận mới. Chỉ cần duyệt một lần để ánh xạ các phần tử.

<!-- thinking:end -->

Theo đề bài, nếu làm phẳng ma trận 2D thành mảng 1D, mỗi lần dịch chuyển sẽ đưa các phần tử sang phải một vị trí, đồng thời chuyển phần tử cuối cùng lên vị trí đầu tiên.

Vì vậy, ta làm phẳng ma trận 2D thành mảng 1D, sau đó tính vị trí cuối cùng $idx = (x, y)$ của mỗi phần tử và cập nhật mảng kết quả bằng `ans[x][y] = grid[i][j]`.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận `grid`. Ta duyệt `grid` một lần để tính vị trí cuối cùng của mỗi phần tử. Nếu không tính phần bộ nhớ của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shiftGrid(self, grid: List[List[int]], k: int) -> List[List[int]]:
        m, n = len(grid), len(grid[0])
        ans = [[0] * n for _ in range(m)]
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                x, y = divmod((i * n + j + k) % (m * n), n)
                ans[x][y] = v
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> shiftGrid(int[][] grid, int k) {
        int m = grid.length, n = grid[0].length;
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            List<Integer> row = new ArrayList<>();
            for (int j = 0; j < n; ++j) {
                row.add(0);
            }
            ans.add(row);
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int idx = (i * n + j + k) % (m * n);
                int x = idx / n, y = idx % n;
                ans.get(x).set(y, grid[i][j]);
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
    vector<vector<int>> shiftGrid(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> ans(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int idx = (i * n + j + k) % (m * n);
                int x = idx / n, y = idx % n;
                ans[x][y] = grid[i][j];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func shiftGrid(grid [][]int, k int) [][]int {
	m, n := len(grid), len(grid[0])
	ans := make([][]int, m)
	for i := range ans {
		ans[i] = make([]int, n)
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			idx := (i*n + j + k) % (m * n)
			x, y := idx/n, idx%n
			ans[x][y] = grid[i][j]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function shiftGrid(grid: number[][], k: number): number[][] {
    const [m, n] = [grid.length, grid[0].length];
    const ans: number[][] = Array.from({ length: m }, () => Array.from({ length: n }, () => 0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            const idx = (i * n + j + k) % (m * n);
            const [x, y] = [Math.floor(idx / n), idx % n];
            ans[x][y] = grid[i][j];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
