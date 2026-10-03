---
comments: true
difficulty: Medium
rating: 1372
source: Biweekly Contest 92 Q2
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [2482. Difference Between Ones and Zeros in Row and Column](https://leetcode.com/problems/difference-between-ones-and-zeros-in-row-and-column)

[中文文档](/solution/2400-2499/2482.Difference%20Between%20Ones%20and%20Zeros%20in%20Row%20and%20Column/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận nhị phân <code>m x n</code> <strong>được đánh chỉ số từ 0</strong> <code>grid</code>.</p>

<p>Một ma trận hiệu <code>m x n</code> <strong>được đánh chỉ số từ 0</strong> <code>diff</code> được tạo theo quy trình sau:</p>

<ul>
	<li>Gọi số lượng số 1 trong hàng thứ <code>i<sup>th</sup></code> là <code>onesRow<sub>i</sub></code>.</li>
	<li>Gọi số lượng số 1 trong cột thứ <code>j<sup>th</sup></code> là <code>onesCol<sub>j</sub></code>.</li>
	<li>Gọi số lượng số 0 trong hàng thứ <code>i<sup>th</sup></code> là <code>zerosRow<sub>i</sub></code>.</li>
	<li>Gọi số lượng số 0 trong cột thứ <code>j<sup>th</sup></code> là <code>zerosCol<sub>j</sub></code>.</li>
	<li><code>diff[i][j] = onesRow<sub>i</sub> + onesCol<sub>j</sub> - zerosRow<sub>i</sub> - zerosCol<sub>j</sub></code></li>
</ul>

<p>Trả về <em>ma trận hiệu </em><code>diff</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2482.Difference%20Between%20Ones%20and%20Zeros%20in%20Row%20and%20Column/images/image-20221106171729-5.png" style="width: 400px; height: 208px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,1],[1,0,1],[0,0,1]]
<strong>Đầu ra:</strong> [[0,0,4],[0,0,4],[-2,-2,2]]
<strong>Giải thích:</strong>
- diff[0][0] = <code>onesRow<sub>0</sub> + onesCol<sub>0</sub> - zerosRow<sub>0</sub> - zerosCol<sub>0</sub></code> = 2 + 1 - 1 - 2 = 0
- diff[0][1] = <code>onesRow<sub>0</sub> + onesCol<sub>1</sub> - zerosRow<sub>0</sub> - zerosCol<sub>1</sub></code> = 2 + 1 - 1 - 2 = 0
- diff[0][2] = <code>onesRow<sub>0</sub> + onesCol<sub>2</sub> - zerosRow<sub>0</sub> - zerosCol<sub>2</sub></code> = 2 + 3 - 1 - 0 = 4
- diff[1][0] = <code>onesRow<sub>1</sub> + onesCol<sub>0</sub> - zerosRow<sub>1</sub> - zerosCol<sub>0</sub></code> = 2 + 1 - 1 - 2 = 0
- diff[1][1] = <code>onesRow<sub>1</sub> + onesCol<sub>1</sub> - zerosRow<sub>1</sub> - zerosCol<sub>1</sub></code> = 2 + 1 - 1 - 2 = 0
- diff[1][2] = <code>onesRow<sub>1</sub> + onesCol<sub>2</sub> - zerosRow<sub>1</sub> - zerosCol<sub>2</sub></code> = 2 + 3 - 1 - 0 = 4
- diff[2][0] = <code>onesRow<sub>2</sub> + onesCol<sub>0</sub> - zerosRow<sub>2</sub> - zerosCol<sub>0</sub></code> = 1 + 1 - 2 - 2 = -2
- diff[2][1] = <code>onesRow<sub>2</sub> + onesCol<sub>1</sub> - zerosRow<sub>2</sub> - zerosCol<sub>1</sub></code> = 1 + 1 - 2 - 2 = -2
- diff[2][2] = <code>onesRow<sub>2</sub> + onesCol<sub>2</sub> - zerosRow<sub>2</sub> - zerosCol<sub>2</sub></code> = 1 + 3 - 2 - 0 = 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2482.Difference%20Between%20Ones%20and%20Zeros%20in%20Row%20and%20Column/images/image-20221106171747-6.png" style="width: 358px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[1,1,1]]
<strong>Đầu ra:</strong> [[5,5,5],[5,5,5]]
<strong>Giải thích:</strong>
- diff[0][0] = onesRow<sub>0</sub> + onesCol<sub>0</sub> - zerosRow<sub>0</sub> - zerosCol<sub>0</sub> = 3 + 2 - 0 - 0 = 5
- diff[0][1] = onesRow<sub>0</sub> + onesCol<sub>1</sub> - zerosRow<sub>0</sub> - zerosCol<sub>1</sub> = 3 + 2 - 0 - 0 = 5
- diff[0][2] = onesRow<sub>0</sub> + onesCol<sub>2</sub> - zerosRow<sub>0</sub> - zerosCol<sub>2</sub> = 3 + 2 - 0 - 0 = 5
- diff[1][0] = onesRow<sub>1</sub> + onesCol<sub>0</sub> - zerosRow<sub>1</sub> - zerosCol<sub>0</sub> = 3 + 2 - 0 - 0 = 5
- diff[1][1] = onesRow<sub>1</sub> + onesCol<sub>1</sub> - zerosRow<sub>1</sub> - zerosCol<sub>1</sub> = 3 + 2 - 0 - 0 = 5
- diff[1][2] = onesRow<sub>1</sub> + onesCol<sub>2</sub> - zerosRow<sub>1</sub> - zerosCol<sub>2</sub> = 3 + 2 - 0 - 0 = 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>grid[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $diff[i][j]=onesRow_i+onesCol_j-zerosRow_i-zerosCol_j$, và số lượng số 0 bằng số cột hoặc số hàng trừ đi số lượng số 1. Vì $mn\le 10^5$, ta đếm số lượng số 1 trong mỗi hàng và cột, sau đó điền giá trị cho từng ô.

<!-- thinking:end -->

Ta có thể giải bài toán này bằng cách mô phỏng quy trình như mô tả trong đề bài.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m + n)$ nếu không tính phần không gian dùng cho đáp án. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def onesMinusZeros(self, grid: List[List[int]]) -> List[List[int]]:
        m, n = len(grid), len(grid[0])
        rows = [0] * m
        cols = [0] * n
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                rows[i] += v
                cols[j] += v
        diff = [[0] * n for _ in range(m)]
        for i, r in enumerate(rows):
            for j, c in enumerate(cols):
                diff[i][j] = r + c - (n - r) - (m - c)
        return diff
```

#### Java

```java
class Solution {
    public int[][] onesMinusZeros(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] rows = new int[m];
        int[] cols = new int[n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int v = grid[i][j];
                rows[i] += v;
                cols[j] += v;
            }
        }
        int[][] diff = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                diff[i][j] = rows[i] + cols[j] - (n - rows[i]) - (m - cols[j]);
            }
        }
        return diff;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> onesMinusZeros(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int> rows(m);
        vector<int> cols(n);
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int v = grid[i][j];
                rows[i] += v;
                cols[j] += v;
            }
        }
        vector<vector<int>> diff(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                diff[i][j] = rows[i] + cols[j] - (n - rows[i]) - (m - cols[j]);
            }
        }
        return diff;
    }
};
```

#### Go

```go
func onesMinusZeros(grid [][]int) [][]int {
	m, n := len(grid), len(grid[0])
	rows := make([]int, m)
	cols := make([]int, n)
	diff := make([][]int, m)
	for i, row := range grid {
		diff[i] = make([]int, n)
		for j, v := range row {
			rows[i] += v
			cols[j] += v
		}
	}
	for i, r := range rows {
		for j, c := range cols {
			diff[i][j] = r + c - (n - r) - (m - c)
		}
	}
	return diff
}
```

#### TypeScript

```ts
function onesMinusZeros(grid: number[][]): number[][] {
    const m = grid.length;
    const n = grid[0].length;
    const rows = new Array(m).fill(0);
    const cols = new Array(n).fill(0);
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j]) {
                rows[i]++;
                cols[j]++;
            }
        }
    }
    const ans = Array.from({ length: m }, () => new Array(n).fill(0));
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            ans[i][j] = rows[i] + cols[j] - (m - rows[i]) - (n - cols[j]);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn ones_minus_zeros(grid: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let m = grid.len();
        let n = grid[0].len();
        let mut rows = vec![0; m];
        let mut cols = vec![0; n];
        for i in 0..m {
            for j in 0..n {
                if grid[i][j] == 1 {
                    rows[i] += 1;
                    cols[j] += 1;
                }
            }
        }
        let mut ans = vec![vec![0; n]; m];
        for i in 0..m {
            for j in 0..n {
                ans[i][j] = (rows[i] + cols[j] - (m - rows[i]) - (n - cols[j])) as i32;
            }
        }
        ans
    }
}
```

#### C

```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** onesMinusZeros(int** grid, int gridSize, int* gridColSize, int* returnSize, int** returnColumnSizes) {
    int* rows = malloc(sizeof(int) * gridSize);
    int* cols = malloc(sizeof(int) * gridColSize[0]);
    memset(rows, 0, sizeof(int) * gridSize);
    memset(cols, 0, sizeof(int) * gridColSize[0]);
    for (int i = 0; i < gridSize; i++) {
        for (int j = 0; j < gridColSize[0]; j++) {
            if (grid[i][j]) {
                rows[i]++;
                cols[j]++;
            }
        }
    }
    int** ans = malloc(sizeof(int*) * gridSize);
    for (int i = 0; i < gridSize; i++) {
        ans[i] = malloc(sizeof(int) * gridColSize[0]);
        for (int j = 0; j < gridColSize[0]; j++) {
            ans[i][j] = rows[i] + cols[j] - (gridSize - rows[i]) - (gridColSize[0] - cols[j]);
        }
    }
    *returnSize = gridSize;
    *returnColumnSizes = gridColSize;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
