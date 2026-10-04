---
comments: true
difficulty: Medium
rating: 1819
source: Weekly Contest 397 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3148. Maximum Difference Score in a Grid](https://leetcode.com/problems/maximum-difference-score-in-a-grid)

[中文文档](/solution/3100-3199/3148.Maximum%20Difference%20Score%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận <code>m x n</code> <code>grid</code> gồm các số nguyên <strong>dương</strong>. Bạn có thể di chuyển từ một ô trong ma trận đến <strong>bất kỳ</strong> ô nào khác nằm bên dưới hoặc bên phải (không nhất thiết phải kề nhau). Điểm số của một bước di chuyển từ ô có giá trị <code>c1</code> đến ô có giá trị <code>c2</code> là <code>c2 - c1</code>.<!-- notionvc: 8819ca04-8606-4ecf-815b-fb77bc63b851 --></p>

<p>Bạn có thể bắt đầu tại <strong>bất kỳ</strong> ô nào và phải thực hiện <strong>ít nhất</strong> một bước di chuyển.</p>

<p>Trả về tổng điểm <strong>lớn nhất</strong> mà bạn có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3148.Maximum%20Difference%20Score%20in%20a%20Grid/images/grid1.png" style="width: 240px; height: 240px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[9,5,7,3],[8,9,6,1],[6,7,14,3],[2,5,3,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong> Ta bắt đầu tại ô <code>(0, 1)</code> và thực hiện các bước di chuyển sau:<br />
- Di chuyển từ ô <code>(0, 1)</code> đến <code>(2, 1)</code> với điểm số là <code>7 - 5 = 2</code>.<br />
- Di chuyển từ ô <code>(2, 1)</code> đến <code>(2, 2)</code> với điểm số là <code>14 - 7 = 7</code>.<br />
Tổng điểm là <code>2 + 7 = 9</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3148.Maximum%20Difference%20Score%20in%20a%20Grid/images/moregridsdrawio-1.png" style="width: 180px; height: 116px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[4,3,2],[3,2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong> Ta bắt đầu tại ô <code>(0, 0)</code> và thực hiện một bước di chuyển: từ <code>(0, 0)</code> đến <code>(0, 1)</code>. Điểm số là <code>3 - 4 = -1</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 1000</code></li>
	<li><code>4 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các bước di chuyển chỉ đi sang phải hoặc xuống dưới, và tổng điểm triệt tiêu thành giá trị của ô cuối trừ giá trị của ô đầu. Việc thử mọi cặp ô sẽ tốn $O(m^2n^2)$.
>
> Với một ô đích cố định, điểm bắt đầu tốt nhất là ô có giá trị nhỏ nhất trong vùng phía trên bên trái, bao gồm cả ô đích nhưng không tính chính ô đó. Giá trị nhỏ nhất này được truy hồi từ ô phía trên và ô bên trái.
>
> Gọi $f[i][j]$ là giá trị nhỏ nhất xuất hiện trên một đường đi có thể đến $(i,j)$. Đáp án là giá trị lớn nhất của $grid[i][j]-\min(f[i-1][j],f[i][j-1])$.

<!-- thinking:end -->

Theo mô tả bài toán, nếu giá trị của các ô ta đi qua là $c_1, c_2, \cdots, c_k$ thì điểm số của ta là $c_2 - c_1 + c_3 - c_2 + \cdots + c_k - c_{k-1} = c_k - c_1$. Do đó, bài toán được chuyển thành: với mỗi ô $(i, j)$ của ma trận, nếu chọn ô đó làm điểm kết thúc thì giá trị nhỏ nhất của điểm bắt đầu là bao nhiêu.

Ta có thể dùng quy hoạch động để giải bài toán này. Gọi $f[i][j]$ là giá trị nhỏ nhất trên đường đi có $(i, j)$ làm điểm kết thúc. Khi đó, ta có phương trình chuyển trạng thái:

$$
f[i][j] = \min(f[i-1][j], f[i][j-1], grid[i][j])
$$

Vì vậy, đáp án là giá trị lớn nhất của $\textit{grid}[i][j] - \min(f[i-1][j], f[i][j-1])$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, grid: List[List[int]]) -> int:
        f = [[0] * len(grid[0]) for _ in range(len(grid))]
        ans = -inf
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                mi = inf
                if i:
                    mi = min(mi, f[i - 1][j])
                if j:
                    mi = min(mi, f[i][j - 1])
                ans = max(ans, x - mi)
                f[i][j] = min(x, mi)
        return ans
```

#### Java

```java
class Solution {
    public int maxScore(List<List<Integer>> grid) {
        int m = grid.size(), n = grid.get(0).size();
        final int inf = 1 << 30;
        int ans = -inf;
        int[][] f = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int mi = inf;
                if (i > 0) {
                    mi = Math.min(mi, f[i - 1][j]);
                }
                if (j > 0) {
                    mi = Math.min(mi, f[i][j - 1]);
                }
                ans = Math.max(ans, grid.get(i).get(j) - mi);
                f[i][j] = Math.min(grid.get(i).get(j), mi);
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
    int maxScore(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        const int inf = 1 << 30;
        int ans = -inf;
        int f[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int mi = inf;
                if (i) {
                    mi = min(mi, f[i - 1][j]);
                }
                if (j) {
                    mi = min(mi, f[i][j - 1]);
                }
                ans = max(ans, grid[i][j] - mi);
                f[i][j] = min(grid[i][j], mi);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxScore(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, n)
	}
	const inf int = 1 << 30
	ans := -inf
	for i, row := range grid {
		for j, x := range row {
			mi := inf
			if i > 0 {
				mi = min(mi, f[i-1][j])
			}
			if j > 0 {
				mi = min(mi, f[i][j-1])
			}
			ans = max(ans, x-mi)
			f[i][j] = min(x, mi)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxScore(grid: number[][]): number {
    const [m, n] = [grid.length, grid[0].length];
    const f: number[][] = Array.from({ length: m }, () => Array.from({ length: n }, () => 0));
    let ans = -Infinity;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            let mi = Infinity;
            if (i) {
                mi = Math.min(mi, f[i - 1][j]);
            }
            if (j) {
                mi = Math.min(mi, f[i][j - 1]);
            }
            ans = Math.max(ans, grid[i][j] - mi);
            f[i][j] = Math.min(mi, grid[i][j]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
