---
comments: true
difficulty: Hard
rating: 2402
source: Weekly Contest 413 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
    - Matrix
---

<!-- problem:start -->

# [3276. Select Cells in Grid With Maximum Score](https://leetcode.com/problems/select-cells-in-grid-with-maximum-score)

[中文文档](/solution/3200-3299/3276.Select%20Cells%20in%20Grid%20With%20Maximum%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận 2D <code>grid</code> gồm các số nguyên dương.</p>

<p>Hãy chọn <em>một hoặc nhiều</em> ô trong ma trận sao cho thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Không có hai ô được chọn nào nằm trên <strong>cùng</strong> một hàng của ma trận.</li>
	<li>Các giá trị trong tập hợp những ô được chọn là <strong>duy nhất</strong>.</li>
</ul>

<p>Điểm số là <strong>tổng</strong> các giá trị của những ô được chọn.</p>

<p>Hãy trả về điểm số <strong>lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2,3],[4,3,2],[1,1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3276.Select%20Cells%20in%20Grid%20With%20Maximum%20Score/images/grid1drawio.png" /></p>

<p>Ta có thể chọn các ô có giá trị 1, 3 và 4 được tô màu ở trên.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[8,7,6],[8,3,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3276.Select%20Cells%20in%20Grid%20With%20Maximum%20Score/images/grid8_8drawio.png" style="width: 170px; height: 114px;" /></p>

<p>Ta có thể chọn các ô có giá trị 7 và 8 được tô màu ở trên.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= grid.length, grid[i].length &lt;= 10</code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Chọn nhiều nhất một ô trên mỗi hàng với các giá trị khác nhau, đồng thời tối đa hóa tổng. Có nhiều nhất $10$ hàng và các giá trị $\le 100$; nếu chọn theo hàng thì việc quyết định “các giá trị còn lại” sẽ ràng buộc lẫn nhau. Vì vậy, ta xét các giá trị từ nhỏ đến lớn và dùng bitmask để biểu diễn các hàng đã được sử dụng.
>
> $f[i][S]$ là điểm số lớn nhất khi sử dụng các giá trị $\le i$ và tập hàng là $S$. Bỏ qua $i$ thì sao chép $f[i-1][S]$; nếu chọn $i$, ta xét một hàng $k\in S$ chứa nó. Trước tiên, ta xây dựng một map từ giá trị đến các hàng.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là điểm số lớn nhất khi chọn các số trong $[1,..i]$ và trạng thái của các hàng tương ứng với những số đã chọn là $j$. Ban đầu, $f[i][j] = 0$, và đáp án là $f[\textit{mx}][2^m - 1]$, trong đó $\textit{mx}$ là giá trị lớn nhất trong ma trận, còn $m$ là số hàng của ma trận.

Đầu tiên, ta tiền xử lý ma trận bằng một hash table $g$ để ghi lại tập hợp các hàng tương ứng với mỗi số. Sau đó, ta có thể dùng quy hoạch động nén trạng thái để giải bài toán.

Với trạng thái $f[i][j]$, ta có thể không chọn số $i$, khi đó $f[i][j] = f[i-1][j]$. Ngoài ra, ta có thể chọn số $i$. Khi đó, ta cần duyệt qua từng hàng $k$ trong tập hợp $g[i]$ tương ứng với số $i$. Nếu bit thứ $k$ của $j$ là $1$, điều đó có nghĩa là ta có thể chọn số $i$. Vì vậy, $f[i][j] = \max(f[i][j], f[i-1][j \oplus 2^k] + i)$.

Cuối cùng, ta trả về $f[\textit{mx}][2^m - 1]$.

Độ phức tạp thời gian là $O(m \times 2^m \times \textit{mx})$, còn độ phức tạp không gian là $O(\textit{mx} \times 2^m)$. Ở đây, $m$ là số hàng của ma trận và $\textit{mx}$ là giá trị lớn nhất trong ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, grid: List[List[int]]) -> int:
        g = defaultdict(set)
        mx = 0
        for i, row in enumerate(grid):
            for x in row:
                g[x].add(i)
                mx = max(mx, x)
        m = len(grid)
        f = [[0] * (1 << m) for _ in range(mx + 1)]
        for i in range(1, mx + 1):
            for j in range(1 << m):
                f[i][j] = f[i - 1][j]
                for k in g[i]:
                    if j >> k & 1:
                        f[i][j] = max(f[i][j], f[i - 1][j ^ 1 << k] + i)
        return f[-1][-1]
```

#### Java

```java
class Solution {
    public int maxScore(List<List<Integer>> grid) {
        int m = grid.size();
        int mx = 0;
        boolean[][] g = new boolean[101][m + 1];
        for (int i = 0; i < m; ++i) {
            for (int x : grid.get(i)) {
                g[x][i] = true;
                mx = Math.max(mx, x);
            }
        }
        int[][] f = new int[mx + 1][1 << m];
        for (int i = 1; i <= mx; ++i) {
            for (int j = 0; j < 1 << m; ++j) {
                f[i][j] = f[i - 1][j];
                for (int k = 0; k < m; ++k) {
                    if (g[i][k] && (j >> k & 1) == 1) {
                        f[i][j] = Math.max(f[i][j], f[i - 1][j ^ 1 << k] + i);
                    }
                }
            }
        }
        return f[mx][(1 << m) - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScore(vector<vector<int>>& grid) {
        int m = grid.size();
        int mx = 0;
        bool g[101][11]{};
        for (int i = 0; i < m; ++i) {
            for (int x : grid[i]) {
                g[x][i] = true;
                mx = max(mx, x);
            }
        }
        int f[mx + 1][1 << m];
        memset(f, 0, sizeof(f));
        for (int i = 1; i <= mx; ++i) {
            for (int j = 0; j < 1 << m; ++j) {
                f[i][j] = f[i - 1][j];
                for (int k = 0; k < m; ++k) {
                    if (g[i][k] && (j >> k & 1) == 1) {
                        f[i][j] = max(f[i][j], f[i - 1][j ^ 1 << k] + i);
                    }
                }
            }
        }
        return f[mx][(1 << m) - 1];
    }
};
```

#### Go

```go
func maxScore(grid [][]int) int {
	m := len(grid)
	mx := 0
	g := [101][11]bool{}
	for i, row := range grid {
		for _, x := range row {
			g[x][i] = true
			mx = max(mx, x)
		}
	}
	f := make([][]int, mx+1)
	for i := range f {
		f[i] = make([]int, 1<<m)
	}
	for i := 1; i <= mx; i++ {
		for j := 0; j < 1<<m; j++ {
			f[i][j] = f[i-1][j]
			for k := 0; k < m; k++ {
				if g[i][k] && (j>>k&1) == 1 {
					f[i][j] = max(f[i][j], f[i-1][j^1<<k]+i)
				}
			}
		}
	}
	return f[mx][1<<m-1]
}
```

#### TypeScript

```ts
function maxScore(grid: number[][]): number {
    const m = grid.length;
    let mx = 0;
    const g: boolean[][] = Array.from({ length: 101 }, () => Array(m + 1).fill(false));
    for (let i = 0; i < m; ++i) {
        for (const x of grid[i]) {
            g[x][i] = true;
            mx = Math.max(mx, x);
        }
    }
    const f: number[][] = Array.from({ length: mx + 1 }, () => Array(1 << m).fill(0));
    for (let i = 1; i <= mx; ++i) {
        for (let j = 0; j < 1 << m; ++j) {
            f[i][j] = f[i - 1][j];
            for (let k = 0; k < m; ++k) {
                if (g[i][k] && ((j >> k) & 1) === 1) {
                    f[i][j] = Math.max(f[i][j], f[i - 1][j ^ (1 << k)] + i);
                }
            }
        }
    }
    return f[mx][(1 << m) - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
