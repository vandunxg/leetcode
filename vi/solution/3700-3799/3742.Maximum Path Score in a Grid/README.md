---
comments: true
difficulty: Medium
rating: 1804
source: Weekly Contest 475 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3742. Maximum Path Score in a Grid](https://leetcode.com/problems/maximum-path-score-in-a-grid)

[中文文档](/solution/3700-3799/3742.Maximum%20Path%20Score%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một lưới <code>m x n</code>, trong đó mỗi ô chứa một trong các giá trị 0, 1 hoặc 2. Bạn cũng được cho một số nguyên <code>k</code>.</p>

<p>Bạn bắt đầu từ góc trên bên trái <code>(0, 0)</code> và muốn đi đến góc dưới bên phải <code>(m - 1, n - 1)</code> bằng cách chỉ di chuyển <strong>sang phải</strong> hoặc <strong>xuống dưới</strong>.</p>

<p>Mỗi ô đóng góp một điểm số và phát sinh một chi phí tương ứng với giá trị của ô:</p>

<ul>
	<li>0: cộng 0 điểm và tốn 0 chi phí.</li>
	<li>1: cộng 1 điểm và tốn 1 chi phí.</li>
	<li>2: cộng 2 điểm và tốn 1 chi phí.</li>
</ul>

<p>Trả về điểm số <strong>lớn nhất</strong> có thể đạt được mà không vượt quá tổng chi phí <code>k</code>, hoặc -1 nếu không tồn tại đường đi hợp lệ.</p>

<p><strong>Lưu ý:</strong> Nếu bạn đến ô cuối cùng nhưng tổng chi phí vượt quá <code>k</code>, đường đi đó không hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0, 1],[2, 0]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi tối ưu là:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Ô</th>
			<th style="border: 1px solid black;">grid[i][j]</th>
			<th style="border: 1px solid black;">Điểm</th>
			<th style="border: 1px solid black;">Tổng<br />
			Điểm</th>
			<th style="border: 1px solid black;">Chi phí</th>
			<th style="border: 1px solid black;">Tổng<br />
			Chi phí</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">(0, 0)</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">(1, 0)</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">(1, 1)</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, điểm số lớn nhất có thể đạt được là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[0, 1],[1, 2]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có đường đi nào đến được ô <code>(1, 1)</code> mà không vượt quá chi phí k. Vì vậy, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>3</sup>​​​​​​​</code></li>
	<li><code><sup>​​​​​​​</sup>grid[0][0] == 0</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm với Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ có thể di chuyển sang phải hoặc xuống dưới, ngân sách là $k$, và các giá trị trong ô đều rất nhỏ. Khi tìm ngược từ đích về $(0,0)$, một ô khác 0 sẽ tốn $1$ chi phí và cộng giá trị của ô đó vào điểm số. Trạng thái $(i,j,k)$ phù hợp để memoization; đi ra ngoài biên hoặc hết ngân sách đều đồng nghĩa với việc không thể đến đích.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs}(i, j, k)$ biểu diễn điểm số lớn nhất có thể đạt được khi bắt đầu từ vị trí $(i, j)$ và đi đến điểm cuối $(0, 0)$ với chi phí còn lại không vượt quá $k$. Ta sử dụng memoization để tránh các phép tính trùng lặp.

Cụ thể, các bước triển khai hàm $\textit{dfs}(i, j, k)$ như sau:

1. Nếu tọa độ hiện tại $(i, j)$ nằm ngoài biên hoặc chi phí còn lại $k$ nhỏ hơn $0$, trả về âm vô cùng để biểu thị rằng không thể đến điểm cuối.
2. Nếu tọa độ hiện tại là điểm bắt đầu $(0, 0)$, trả về $0$, biểu thị đã đến điểm cuối (đề bài đảm bảo ô bắt đầu có giá trị $0$).
3. Tính phần điểm số $\textit{res}$ mà ô hiện tại đóng góp. Nếu giá trị của ô hiện tại khác $0$, giảm chi phí còn lại $k$ đi $1$.
4. Đệ quy tính điểm số lớn nhất có thể đạt được từ ô phía trên $(i-1, j)$ và ô bên trái $(i, j-1)$ khi đi đến điểm cuối với chi phí còn lại không vượt quá $k$, lần lượt ký hiệu là $\textit{a}$ và $\textit{b}$.
5. Cộng phần điểm số $\textit{res}$ của ô hiện tại vào $\max(\textit{a}, \textit{b})$ để nhận được điểm số lớn nhất có thể đạt được từ ô hiện tại, rồi trả về giá trị này.

Cuối cùng, ta gọi $\textit{dfs}(m-1, n-1, k)$ để tính điểm số lớn nhất có thể đạt được khi bắt đầu từ góc dưới bên phải và đi đến góc trên bên trái với chi phí còn lại không vượt quá $k$. Nếu kết quả nhỏ hơn $0$, trả về $-1$ để biểu thị không tồn tại đường đi hợp lệ; nếu không, trả về kết quả đó.

Độ phức tạp thời gian là $O(m \times n \times k)$, và độ phức tạp không gian là $O(m \times n \times k)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của lưới, còn $k$ là chi phí tối đa được phép.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPathScore(self, grid: List[List[int]], k: int) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if i < 0 or j < 0 or k < 0:
                return -inf
            if i == 0 and j == 0:
                return 0
            res = grid[i][j]
            if grid[i][j]:
                k -= 1
            a = dfs(i - 1, j, k)
            b = dfs(i, j - 1, k)
            res += max(a, b)
            return res

        ans = dfs(len(grid) - 1, len(grid[0]) - 1, k)
        dfs.cache_clear()
        return -1 if ans < 0 else ans
```

#### Java

```java
class Solution {
    private int[][] grid;
    private Integer[][][] f;
    private final int inf = 1 << 30;

    public int maxPathScore(int[][] grid, int k) {
        this.grid = grid;
        int m = grid.length;
        int n = grid[0].length;
        f = new Integer[m][n][k + 1];
        int ans = dfs(m - 1, n - 1, k);
        return ans < 0 ? -1 : ans;
    }

    private int dfs(int i, int j, int k) {
        if (i < 0 || j < 0 || k < 0) {
            return -inf;
        }
        if (i == 0 && j == 0) {
            return 0;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        int res = grid[i][j];
        int nk = k;
        if (grid[i][j] > 0) {
            --nk;
        }
        int a = dfs(i - 1, j, nk);
        int b = dfs(i, j - 1, nk);
        res += Math.max(a, b);
        f[i][j][k] = res;
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxPathScore(vector<vector<int>>& grid, int k) {
        int m = grid.size();
        int n = grid[0].size();
        int inf = 1 << 30;
        vector f(m, vector(n, vector<int>(k + 1, -1)));

        auto dfs = [&](this auto&& dfs, int i, int j, int k) -> int {
            if (i < 0 || j < 0 || k < 0) {
                return -inf;
            }
            if (i == 0 && j == 0) {
                return 0;
            }
            if (f[i][j][k] != -1) {
                return f[i][j][k];
            }

            int res = grid[i][j];
            int nk = k;
            if (grid[i][j] > 0) {
                --nk;
            }

            int a = dfs(i - 1, j, nk);
            int b = dfs(i, j - 1, nk);
            res += max(a, b);

            return f[i][j][k] = res;
        };

        int ans = dfs(m - 1, n - 1, k);
        return ans < 0 ? -1 : ans;
    }
};
```

#### Go

```go
func maxPathScore(grid [][]int, k int) int {
	m := len(grid)
	n := len(grid[0])
	inf := 1 << 30

	f := make([][][]int, m)
	for i := 0; i < m; i++ {
		f[i] = make([][]int, n)
		for j := 0; j < n; j++ {
			f[i][j] = make([]int, k+1)
			for t := 0; t <= k; t++ {
				f[i][j][t] = -1
			}
		}
	}

	var dfs func(i, j, k int) int
	dfs = func(i, j, k int) int {
		if i < 0 || j < 0 || k < 0 {
			return -inf
		}
		if i == 0 && j == 0 {
			return 0
		}
		if f[i][j][k] != -1 {
			return f[i][j][k]
		}

		res := grid[i][j]
		nk := k
		if grid[i][j] != 0 {
			nk--
		}

		a := dfs(i-1, j, nk)
		b := dfs(i, j-1, nk)
		res += max(a, b)

		f[i][j][k] = res
		return res
	}

	ans := dfs(m-1, n-1, k)
	if ans < 0 {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function maxPathScore(grid: number[][], k: number): number {
    const m = grid.length;
    const n = grid[0].length;
    const inf = 1 << 30;

    const f: number[][][] = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Array(k + 1).fill(-1)),
    );

    const dfs = (i: number, j: number, k: number): number => {
        if (i < 0 || j < 0 || k < 0) {
            return -inf;
        }
        if (i === 0 && j === 0) {
            return 0;
        }
        if (f[i][j][k] !== -1) {
            return f[i][j][k];
        }

        let res = grid[i][j];
        let nk = k;
        if (grid[i][j] !== 0) {
            --nk;
        }

        const a = dfs(i - 1, j, nk);
        const b = dfs(i, j - 1, nk);
        res += Math.max(a, b);

        f[i][j][k] = res;
        return res;
    };

    const ans = dfs(m - 1, n - 1, k);
    return ans < 0 ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
