---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Matrix
---

<!-- problem:start -->

# [807. Max Increase to Keep City Skyline](https://leetcode.com/problems/max-increase-to-keep-city-skyline)

[中文文档](/solution/0800-0899/0807.Max%20Increase%20to%20Keep%20City%20Skyline/README.md)

## Mô tả

<!-- description:start -->

<p>Có một thành phố gồm các khối <code>n x n</code>, mỗi khối chứa một tòa nhà hình lăng trụ đứng đáy vuông. Cho ma trận số nguyên <code>grid</code> kích thước <code>n x n</code>, đánh chỉ số từ <strong>0</strong>, trong đó <code>grid[r][c]</code> biểu thị <strong>chiều cao</strong> của tòa nhà ở hàng <code>r</code>, cột <code>c</code>.</p>

<p><strong>Đường chân trời</strong> của thành phố là đường viền ngoài được tạo bởi các tòa nhà khi nhìn thành phố từ xa ở một phía. <strong>Đường chân trời</strong> nhìn từ mỗi hướng chính bắc, đông, nam và tây có thể khác nhau.</p>

<p>Ta được phép tăng chiều cao của <strong>bất kỳ số lượng tòa nhà nào với mức tăng tùy ý</strong> (mức tăng có thể khác nhau ở mỗi tòa nhà). Tòa nhà có chiều cao <code>0</code> cũng có thể được tăng chiều cao. Tuy nhiên, việc tăng chiều cao không được làm thay đổi <strong>đường chân trời</strong> của thành phố khi nhìn từ bất kỳ hướng chính nào.</p>

<p>Hãy trả về <em><strong>tổng mức tăng tối đa</strong> chiều cao của các tòa nhà mà vẫn <strong>không</strong> làm thay đổi <strong>đường chân trời</strong> của thành phố khi nhìn từ bất kỳ hướng chính nào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0807.Max%20Increase%20to%20Keep%20City%20Skyline/images/807-ex1.png" style="width: 700px; height: 603px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[3,0,8,4],[2,4,5,7],[9,2,6,3],[0,3,1,0]]
<strong>Đầu ra:</strong> 35
<strong>Giải thích:</strong> Chiều cao các tòa nhà được thể hiện ở giữa hình trên.
Đường chân trời khi nhìn từ mỗi hướng chính được vẽ màu đỏ.
Ma trận sau khi tăng chiều cao các tòa nhà mà không làm thay đổi đường chân trời là:
gridNew = [ [8, 4, 8, 7],
            [7, 4, 7, 7],
            [9, 4, 8, 7],
            [3, 3, 3, 3] ]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,0,0],[0,0,0],[0,0,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tăng chiều cao bất kỳ tòa nhà nào cũng sẽ làm thay đổi đường chân trời.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>n == grid[r].length</code></li>
	<li><code>2 &lt;= n &lt;= 50</code></li>
	<li><code>0 &lt;= grid[r][c] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Đường chân trời được xác định bởi giá trị lớn nhất trên từng hàng và từng cột, nên chiều cao của mỗi ô không thể vượt quá giá trị nhỏ hơn trong hai giá trị đó. Vì $n\le 50$, ta có thể duyệt lần đầu để tìm các giá trị lớn nhất và duyệt lần hai qua các ô.
>
> Ô $(i,j)$ có thể tăng chiều cao đến $\min(\textit{rowMax}[i],\textit{colMax}[j])$. Cộng phần chênh lệch so với chiều cao ban đầu của từng ô sẽ cho tổng mức tăng.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể tăng giá trị của mỗi ô $(i, j)$ đến giá trị nhỏ hơn giữa giá trị lớn nhất của hàng thứ $i$ và cột thứ $j$ mà không làm thay đổi đường chân trời. Vì vậy, phần chiều cao tăng thêm ở mỗi ô là $\min(\textit{rowMax}[i], \textit{colMax}[j]) - \textit{grid}[i][j]$.

Do đó, trước tiên ta duyệt ma trận một lần để tính giá trị lớn nhất của mỗi hàng và cột, lần lượt lưu vào các mảng $\textit{rowMax}$ và $\textit{colMax}$. Sau đó, ta duyệt ma trận lần nữa để tính đáp án.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài cạnh của ma trận $\textit{grid}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxIncreaseKeepingSkyline(self, grid: List[List[int]]) -> int:
        row_max = [max(row) for row in grid]
        col_max = [max(col) for col in zip(*grid)]
        return sum(
            min(row_max[i], col_max[j]) - x
            for i, row in enumerate(grid)
            for j, x in enumerate(row)
        )
```

#### Java

```java
class Solution {
    public int maxIncreaseKeepingSkyline(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] rowMax = new int[m];
        int[] colMax = new int[n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                rowMax[i] = Math.max(rowMax[i], grid[i][j]);
                colMax[j] = Math.max(colMax[j], grid[i][j]);
            }
        }
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans += Math.min(rowMax[i], colMax[j]) - grid[i][j];
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
    int maxIncreaseKeepingSkyline(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vector<int> rowMax(m);
        vector<int> colMax(n);
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                rowMax[i] = max(rowMax[i], grid[i][j]);
                colMax[j] = max(colMax[j], grid[i][j]);
            }
        }
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans += min(rowMax[i], colMax[j]) - grid[i][j];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxIncreaseKeepingSkyline(grid [][]int) (ans int) {
	rowMax := make([]int, len(grid))
	colMax := make([]int, len(grid[0]))
	for i, row := range grid {
		for j, x := range row {
			rowMax[i] = max(rowMax[i], x)
			colMax[j] = max(colMax[j], x)
		}
	}
	for i, row := range grid {
		for j, x := range row {
			ans += min(rowMax[i], colMax[j]) - x
		}
	}
	return
}
```

#### TypeScript

```ts
function maxIncreaseKeepingSkyline(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const rowMax = Array(m).fill(0);
    const colMax = Array(n).fill(0);
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            rowMax[i] = Math.max(rowMax[i], grid[i][j]);
            colMax[j] = Math.max(colMax[j], grid[i][j]);
        }
    }
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            ans += Math.min(rowMax[i], colMax[j]) - grid[i][j];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
