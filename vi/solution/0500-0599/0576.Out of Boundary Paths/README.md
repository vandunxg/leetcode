---
comments: true
difficulty: Medium
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [576. Out of Boundary Paths](https://leetcode.com/problems/out-of-boundary-paths)

[中文文档](/solution/0500-0599/0576.Out%20of%20Boundary%20Paths/README.md)

## Mô tả

<!-- description:start -->

<p>Cho lưới kích thước <code>m x n</code> có một quả bóng, ban đầu nằm ở vị trí <code>[startRow, startColumn]</code>. Bạn có thể di chuyển bóng sang một trong bốn ô kề (có thể đi ra ngoài lưới qua biên). Bạn được thực hiện <strong>tối đa</strong> <code>maxMove</code> lượt di chuyển.</p>

<p>Cho năm số nguyên <code>m</code>, <code>n</code>, <code>maxMove</code>, <code>startRow</code>, <code>startColumn</code>, hãy trả về số cách đưa bóng ra khỏi biên lưới. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0576.Out%20of%20Boundary%20Paths/images/out_of_boundary_paths_1.png" style="width: 500px; height: 296px;" />
<pre>
<strong>Đầu vào:</strong> m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0
<strong>Đầu ra:</strong> 6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0576.Out%20of%20Boundary%20Paths/images/out_of_boundary_paths_2.png" style="width: 500px; height: 293px;" />
<pre>
<strong>Đầu vào:</strong> m = 1, n = 3, maxMove = 3, startRow = 0, startColumn = 1
<strong>Đầu ra:</strong> 12
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>0 &lt;= maxMove &lt;= 50</code></li>
	<li><code>0 &lt;= startRow &lt; m</code></li>
	<li><code>0 &lt;= startColumn &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Từ mỗi ô, ta có thể đi theo bốn hướng tối đa $k$ bước và cần đếm số đường đi ra khỏi lưới. Nếu không dùng memoization, quá trình tìm kiếm sẽ tính lặp lại cùng một ô với cùng số bước còn lại.
>
> $dfs(i,j,k)$ là số đường đi ra khỏi biên bắt đầu từ $(i,j)$ khi còn $k$ bước. Nếu vị trí đã ra ngoài lưới và $k \ge 0$, kết quả là $1$; nếu hết bước thì kết quả là $0$. Chuyển trạng thái theo bốn hướng và lấy modulo $10^9+7$. Mỗi bộ ba trạng thái chỉ được tính một lần.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs}(i, j, k)$ là số đường đi ra khỏi biên khi bắt đầu tại tọa độ $(i, j)$ và còn $k$ bước.

Trong hàm $\textit{dfs}(i, j, k)$, trước tiên xử lý các trường hợp biên. Nếu tọa độ hiện tại $(i, j)$ nằm ngoài lưới, trả về $1$ nếu $k \geq 0$, ngược lại trả về $0$. Nếu $k \leq 0$, nghĩa là bóng vẫn ở trong lưới nhưng đã hết lượt di chuyển, nên trả về $0$. Tiếp theo, lần lượt thử bốn hướng, di chuyển đến tọa độ mới $(x, y)$, gọi đệ quy $\textit{dfs}(x, y, k - 1)$ rồi cộng kết quả vào đáp án.

Trong hàm chính, gọi $\textit{dfs}(startRow, startColumn, maxMove)$ để tính số đường đi ra khỏi biên từ tọa độ ban đầu $(\textit{startRow}, \textit{startColumn})$ khi còn $\textit{maxMove}$ bước.

Để tránh tính toán lặp, ta dùng memoization.

Độ phức tạp thời gian là $O(m \times n \times k)$ và độ phức tạp không gian là $O(m \times n \times k)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của lưới, còn $k$ là số bước tối đa được di chuyển, với $k = \textit{maxMove} \leq 50$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPaths(
        self, m: int, n: int, maxMove: int, startRow: int, startColumn: int
    ) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if not 0 <= i < m or not 0 <= j < n:
                return int(k >= 0)
            if k <= 0:
                return 0
            ans = 0
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                ans = (ans + dfs(x, y, k - 1)) % mod
            return ans

        mod = 10**9 + 7
        dirs = (-1, 0, 1, 0, -1)
        return dfs(startRow, startColumn, maxMove)
```

#### Java

```java
class Solution {
    private int m, n;
    private Integer[][][] f;
    private final int mod = (int) 1e9 + 7;

    public int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        this.m = m;
        this.n = n;
        f = new Integer[m][n][maxMove + 1];
        return dfs(startRow, startColumn, maxMove);
    }

    private int dfs(int i, int j, int k) {
        if (i < 0 || i >= m || j < 0 || j >= n) {
            return k >= 0 ? 1 : 0;
        }
        if (k <= 0) {
            return 0;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        int ans = 0;
        final int[] dirs = {-1, 0, 1, 0, -1};
        for (int d = 0; d < 4; ++d) {
            int x = i + dirs[d], y = j + dirs[d + 1];
            ans = (ans + dfs(x, y, k - 1)) % mod;
        }
        return f[i][j][k] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        int f[m][n][maxMove + 1];
        memset(f, -1, sizeof(f));
        const int mod = 1e9 + 7;
        const int dirs[5] = {-1, 0, 1, 0, -1};
        auto dfs = [&](this auto&& dfs, int i, int j, int k) -> int {
            if (i < 0 || i >= m || j < 0 || j >= n) {
                return k >= 0;
            }
            if (k <= 0) {
                return 0;
            }
            if (f[i][j][k] != -1) {
                return f[i][j][k];
            }
            int ans = 0;
            for (int d = 0; d < 4; ++d) {
                int x = i + dirs[d], y = j + dirs[d + 1];
                ans = (ans + dfs(x, y, k - 1)) % mod;
            }
            return f[i][j][k] = ans;
        };
        return dfs(startRow, startColumn, maxMove);
    }
};
```

#### Go

```go
func findPaths(m int, n int, maxMove int, startRow int, startColumn int) int {
	f := make([][][]int, m)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, maxMove+1)
			for k := range f[i][j] {
				f[i][j][k] = -1
			}
		}
	}
	const mod int = 1e9 + 7
	var dfs func(int, int, int) int
	dirs := [5]int{-1, 0, 1, 0, -1}
	dfs = func(i, j, k int) int {
		if i < 0 || i >= m || j < 0 || j >= n {
			if k >= 0 {
				return 1
			}
			return 0
		}
		if k <= 0 {
			return 0
		}
		if f[i][j][k] != -1 {
			return f[i][j][k]
		}
		ans := 0
		for d := 0; d < 4; d++ {
			x, y := i+dirs[d], j+dirs[d+1]
			ans = (ans + dfs(x, y, k-1)) % mod
		}
		f[i][j][k] = ans
		return ans
	}
	return dfs(startRow, startColumn, maxMove)
}
```

#### TypeScript

```ts
function findPaths(
    m: number,
    n: number,
    maxMove: number,
    startRow: number,
    startColumn: number,
): number {
    const f = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Array(maxMove + 1).fill(-1)),
    );
    const mod = 1000000007;
    const dirs = [-1, 0, 1, 0, -1];
    const dfs = (i: number, j: number, k: number): number => {
        if (i < 0 || i >= m || j < 0 || j >= n) {
            return k >= 0 ? 1 : 0;
        }
        if (k <= 0) {
            return 0;
        }
        if (f[i][j][k] !== -1) {
            return f[i][j][k];
        }
        let ans = 0;
        for (let d = 0; d < 4; ++d) {
            const [x, y] = [i + dirs[d], j + dirs[d + 1]];
            ans = (ans + dfs(x, y, k - 1)) % mod;
        }
        return (f[i][j][k] = ans);
    };
    return dfs(startRow, startColumn, maxMove);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
