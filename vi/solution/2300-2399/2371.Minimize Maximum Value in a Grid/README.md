---
comments: true
difficulty: Hard
tags:
    - Union Find
    - Graph
    - Topological Sort
    - Array
    - Matrix
    - Sorting
---

<!-- problem:start -->

# [2371. Minimize Maximum Value in a Grid 🔒](https://leetcode.com/problems/minimize-maximum-value-in-a-grid)

[中文文档](/solution/2300-2399/2371.Minimize%20Maximum%20Value%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận số nguyên <code>m x n</code> <code>grid</code> gồm các số nguyên dương <strong>phân biệt</strong>.</p>

<p>Bạn phải thay thế mỗi số nguyên trong ma trận bằng một số nguyên dương thỏa mãn các điều kiện sau:</p>

<ul>
	<li><strong>Thứ tự tương đối</strong> của mọi cặp phần tử nằm trên cùng một hàng hoặc cột phải được <strong>giữ nguyên</strong> sau khi thay thế.</li>
	<li>Số <strong>lớn nhất</strong> trong ma trận sau khi thay thế phải <strong>nhỏ nhất</strong> có thể.</li>
</ul>

<p>Thứ tự tương đối được giữ nguyên nếu với mọi cặp phần tử trong ma trận ban đầu sao cho <code>grid[r<sub>1</sub>][c<sub>1</sub>] &gt; grid[r<sub>2</sub>][c<sub>2</sub>]</code>, trong đó <code>r<sub>1</sub> == r<sub>2</sub></code> hoặc <code>c<sub>1</sub> == c<sub>2</sub></code>, thì sau khi thay thế cũng phải có <code>grid[r<sub>1</sub>][c<sub>1</sub>] &gt; grid[r<sub>2</sub>][c<sub>2</sub>]</code>.</p>

<p>Ví dụ, nếu <code>grid = [[2, 4, 5], [7, 3, 9]]</code> thì một phép thay thế hợp lệ có thể là <code>grid = [[1, 2, 3], [2, 1, 4]]</code> hoặc <code>grid = [[1, 2, 3], [3, 1, 4]]</code>.</p>

<p>Trả về <em>ma trận <strong>kết quả</strong></em>. Nếu có nhiều đáp án, trả về <strong>bất kỳ</strong> đáp án nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2371.Minimize%20Maximum%20Value%20in%20a%20Grid/images/grid2drawio.png" style="width: 371px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[3,1],[2,5]]
<strong>Đầu ra:</strong> [[2,1],[1,2]]
<strong>Giải thích:</strong> Sơ đồ trên minh họa một phép thay thế hợp lệ.
Số lớn nhất trong ma trận là 2. Có thể chứng minh rằng không thể đạt được giá trị nhỏ hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[10]]
<strong>Đầu ra:</strong> [[1]]
<strong>Giải thích:</strong> Ta thay số duy nhất trong ma trận bằng 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 1000</code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>9</sup></code></li>
	<li><code>grid</code> gồm các số nguyên phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Viết lại grid bằng các số nguyên dương, giữ nguyên thứ tự tương đối và tối thiểu hóa giá trị lớn nhất cuối cùng. Vì mọi giá trị ban đầu khác nhau, ta gán giá trị theo thứ tự đó.
>
> Sau khi sắp xếp, một ô phải lớn hơn các giá trị đã được ghi vào hàng và cột của nó, nên ta ghi $\max(row,col)+1$ và cập nhật các giá trị lớn nhất đó. Mỗi bước đều là nhỏ nhất trong phạm vi cục bộ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minScore(self, grid: List[List[int]]) -> List[List[int]]:
        m, n = len(grid), len(grid[0])
        nums = [(v, i, j) for i, row in enumerate(grid) for j, v in enumerate(row)]
        nums.sort()
        row_max = [0] * m
        col_max = [0] * n
        ans = [[0] * n for _ in range(m)]
        for _, i, j in nums:
            ans[i][j] = max(row_max[i], col_max[j]) + 1
            row_max[i] = col_max[j] = ans[i][j]
        return ans
```

#### Java

```java
class Solution {
    public int[][] minScore(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        List<int[]> nums = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                nums.add(new int[] {grid[i][j], i, j});
            }
        }
        Collections.sort(nums, (a, b) -> a[0] - b[0]);
        int[] rowMax = new int[m];
        int[] colMax = new int[n];
        int[][] ans = new int[m][n];
        for (int[] num : nums) {
            int i = num[1], j = num[2];
            ans[i][j] = Math.max(rowMax[i], colMax[j]) + 1;
            rowMax[i] = ans[i][j];
            colMax[j] = ans[i][j];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> minScore(vector<vector<int>>& grid) {
        vector<tuple<int, int, int>> nums;
        int m = grid.size(), n = grid[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                nums.push_back({grid[i][j], i, j});
            }
        }
        sort(nums.begin(), nums.end());
        vector<int> rowMax(m);
        vector<int> colMax(n);
        vector<vector<int>> ans(m, vector<int>(n));
        for (auto [_, i, j] : nums) {
            ans[i][j] = max(rowMax[i], colMax[j]) + 1;
            rowMax[i] = colMax[j] = ans[i][j];
        }
        return ans;
    }
};
```

#### Go

```go
func minScore(grid [][]int) [][]int {
	m, n := len(grid), len(grid[0])
	nums := [][]int{}
	for i, row := range grid {
		for j, v := range row {
			nums = append(nums, []int{v, i, j})
		}
	}
	sort.Slice(nums, func(i, j int) bool { return nums[i][0] < nums[j][0] })
	rowMax := make([]int, m)
	colMax := make([]int, n)
	ans := make([][]int, m)
	for i := range ans {
		ans[i] = make([]int, n)
	}
	for _, num := range nums {
		i, j := num[1], num[2]
		ans[i][j] = max(rowMax[i], colMax[j]) + 1
		rowMax[i] = ans[i][j]
		colMax[j] = ans[i][j]
	}
	return ans
}
```

#### TypeScript

```ts
function minScore(grid: number[][]): number[][] {
    const m = grid.length;
    const n = grid[0].length;
    const nums = [];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            nums.push([grid[i][j], i, j]);
        }
    }
    nums.sort((a, b) => a[0] - b[0]);
    const rowMax = new Array(m).fill(0);
    const colMax = new Array(n).fill(0);
    const ans = Array.from({ length: m }, _ => new Array(n));
    for (const [_, i, j] of nums) {
        ans[i][j] = Math.max(rowMax[i], colMax[j]) + 1;
        rowMax[i] = colMax[j] = ans[i][j];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
