---
comments: true
difficulty: Medium
rating: 2036
source: Weekly Contest 289 Q3
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [2245. Maximum Trailing Zeros in a Cornered Path](https://leetcode.com/problems/maximum-trailing-zeros-in-a-cornered-path)

[中文文档](/solution/2200-2299/2245.Maximum%20Trailing%20Zeros%20in%20a%20Cornered%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>grid</code> có kích thước <code>m x n</code>, trong đó mỗi ô chứa một số nguyên dương.</p>

<p><strong>Đường đi có góc</strong> được định nghĩa là một tập hợp các ô kề nhau với <strong>nhiều nhất</strong> một lần rẽ. Cụ thể hơn, trước điểm rẽ (nếu có), đường đi chỉ được di chuyển theo <strong>chiều ngang</strong> hoặc <strong>chiều dọc</strong>, không quay lại ô đã đi qua. Sau khi rẽ, đường đi chỉ được di chuyển theo hướng <strong>còn lại</strong>: di chuyển theo chiều dọc nếu trước đó đã di chuyển theo chiều ngang, và ngược lại; đồng thời cũng không được quay lại ô đã đi qua.</p>

<p><strong>Tích</strong> của một đường đi được định nghĩa là tích của tất cả các giá trị trên đường đi.</p>

<p>Trả về <em>số lượng <strong>số 0 ở cuối</strong> <strong>lớn nhất</strong> trong tích của một đường đi có góc được tìm thấy trong </em><code>grid</code>.</p>

<p>Lưu ý:</p>

<ul>
	<li>Di chuyển theo <strong>chiều ngang</strong> nghĩa là di chuyển sang trái hoặc sang phải.</li>
	<li>Di chuyển theo <strong>chiều dọc</strong> nghĩa là di chuyển lên trên hoặc xuống dưới.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2245.Maximum%20Trailing%20Zeros%20in%20a%20Cornered%20Path/images/ex1new2.jpg" style="width: 577px; height: 190px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[23,17,15,3,20],[8,1,20,27,11],[9,4,6,2,21],[40,9,1,10,6],[22,7,4,5,3]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Lưới ở bên trái thể hiện một đường đi có góc hợp lệ.
Tích của đường đi là 15 * 20 * 6 * 1 * 10 = 18000, có 3 số 0 ở cuối.
Có thể chứng minh rằng đây là số lượng số 0 ở cuối lớn nhất trong tích của một đường đi có góc.

Lưới ở giữa không phải là một đường đi có góc vì có nhiều hơn một lần rẽ.
Lưới ở bên phải không phải là một đường đi có góc vì cần quay lại một ô đã đi qua.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2245.Maximum%20Trailing%20Zeros%20in%20a%20Cornered%20Path/images/ex2.jpg" style="width: 150px; height: 157px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[4,3,2],[7,6,1],[8,8,8]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Lưới được hiển thị trong hình phía trên.
Không có đường đi có góc nào trong lưới tạo ra tích có số 0 ở cuối.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= grid[i][j] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Liệt kê điểm rẽ

<!-- thinking:start -->

> **Tư duy**
>
> Một đường đi di chuyển theo chiều ngang rồi theo chiều dọc (hoặc ngược lại). Số 0 ở cuối bằng $\min(\#2,\#5)$ trên đường đi. Vì $mn \le 10^5$, việc duyệt qua mọi đường đi là quá chậm. Khi đã cố định góc, đường đi là một tiền tố hoặc hậu tố của hàng đó cộng với một tiền tố hoặc hậu tố của cột đó.
>
> Tính tổng tiền tố số $2$ và số $5$ trên mọi hàng và cột. Liệt kê góc $(i,j)$ và ghép bốn tổ hợp trái/phải với lên/xuống, chỉ tính ô góc một lần, sau đó lấy giá trị lớn nhất của $\min(\#2,\#5)$.

<!-- thinking:end -->

Trước hết, ta cần hiểu rằng với một tích, số lượng số 0 ở cuối phụ thuộc vào số lượng nhỏ hơn giữa $2$ và $5$ trong các thừa số. Ngoài ra, mỗi đường đi có góc cần đi qua nhiều số nhất có thể, nên nó phải bắt đầu từ một biên, đi đến một điểm rẽ, rồi đi đến một biên khác.

Do đó, ta có thể tạo bốn mảng hai chiều $r2$, $c2$, $r5$, $c5$ để ghi lại số lượng $2$ và $5$ trong mỗi hàng và cột. Cụ thể:

- `r2[i][j]` biểu diễn số lượng $2$ từ cột đầu tiên đến cột thứ $j$ trong hàng thứ $i$;
- `c2[i][j]` biểu diễn số lượng $2$ từ hàng đầu tiên đến hàng thứ $i$ trong cột thứ $j$;
- `r5[i][j]` biểu diễn số lượng $5$ từ cột đầu tiên đến cột thứ $j$ trong hàng thứ $i$;
- `c5[i][j]` biểu diễn số lượng $5$ từ hàng đầu tiên đến hàng thứ $i$ trong cột thứ $j$.

Tiếp theo, ta duyệt mảng hai chiều `grid`. Với mỗi số, ta tính số lượng $2$ và $5$ của nó, rồi cập nhật bốn mảng hai chiều.

Sau đó, ta liệt kê điểm rẽ $(i, j)$. Với mỗi điểm rẽ, ta tính bốn giá trị:

- `a` biểu diễn số lượng nhỏ hơn giữa $2$ và $5$ trong đường đi bắt đầu từ $(i, 1)$, di chuyển sang phải đến $(i, j)$, rồi rẽ và di chuyển lên đến $(1, j)$;
- `b` biểu diễn số lượng nhỏ hơn giữa $2$ và $5$ trong đường đi bắt đầu từ $(i, 1)$, di chuyển sang phải đến $(i, j)$, rồi rẽ và di chuyển xuống đến $(m, j)$;
- `c` biểu diễn số lượng nhỏ hơn giữa $2$ và $5$ trong đường đi bắt đầu từ $(i, n)$, di chuyển sang trái đến $(i, j)$, rồi rẽ và di chuyển lên đến $(1, j)$;
- `d` biểu diễn số lượng nhỏ hơn giữa $2$ và $5$ trong đường đi bắt đầu từ $(i, n)$, di chuyển sang trái đến $(i, j)$, rồi rẽ và di chuyển xuống đến $(m, j)$.

Mỗi lần liệt kê, ta lấy giá trị lớn nhất trong bốn giá trị này, rồi cập nhật đáp án.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của mảng `grid`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTrailingZeros(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        r2 = [[0] * (n + 1) for _ in range(m + 1)]
        c2 = [[0] * (n + 1) for _ in range(m + 1)]
        r5 = [[0] * (n + 1) for _ in range(m + 1)]
        c5 = [[0] * (n + 1) for _ in range(m + 1)]
        for i, row in enumerate(grid, 1):
            for j, x in enumerate(row, 1):
                s2 = s5 = 0
                while x % 2 == 0:
                    x //= 2
                    s2 += 1
                while x % 5 == 0:
                    x //= 5
                    s5 += 1
                r2[i][j] = r2[i][j - 1] + s2
                c2[i][j] = c2[i - 1][j] + s2
                r5[i][j] = r5[i][j - 1] + s5
                c5[i][j] = c5[i - 1][j] + s5
        ans = 0
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                a = min(r2[i][j] + c2[i - 1][j], r5[i][j] + c5[i - 1][j])
                b = min(r2[i][j] + c2[m][j] - c2[i][j], r5[i][j] + c5[m][j] - c5[i][j])
                c = min(r2[i][n] - r2[i][j] + c2[i][j], r5[i][n] - r5[i][j] + c5[i][j])
                d = min(
                    r2[i][n] - r2[i][j - 1] + c2[m][j] - c2[i][j],
                    r5[i][n] - r5[i][j - 1] + c5[m][j] - c5[i][j],
                )
                ans = max(ans, a, b, c, d)
        return ans
```

#### Java

```java
class Solution {
    public int maxTrailingZeros(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] r2 = new int[m + 1][n + 1];
        int[][] c2 = new int[m + 1][n + 1];
        int[][] r5 = new int[m + 1][n + 1];
        int[][] c5 = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int x = grid[i - 1][j - 1];
                int s2 = 0, s5 = 0;
                for (; x % 2 == 0; x /= 2) {
                    ++s2;
                }
                for (; x % 5 == 0; x /= 5) {
                    ++s5;
                }
                r2[i][j] = r2[i][j - 1] + s2;
                c2[i][j] = c2[i - 1][j] + s2;
                r5[i][j] = r5[i][j - 1] + s5;
                c5[i][j] = c5[i - 1][j] + s5;
            }
        }
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int a = Math.min(r2[i][j] + c2[i - 1][j], r5[i][j] + c5[i - 1][j]);
                int b = Math.min(r2[i][j] + c2[m][j] - c2[i][j], r5[i][j] + c5[m][j] - c5[i][j]);
                int c = Math.min(r2[i][n] - r2[i][j] + c2[i][j], r5[i][n] - r5[i][j] + c5[i][j]);
                int d = Math.min(r2[i][n] - r2[i][j - 1] + c2[m][j] - c2[i][j],
                    r5[i][n] - r5[i][j - 1] + c5[m][j] - c5[i][j]);
                ans = Math.max(ans, Math.max(a, Math.max(b, Math.max(c, d))));
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
    int maxTrailingZeros(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> r2(m + 1, vector<int>(n + 1));
        vector<vector<int>> c2(m + 1, vector<int>(n + 1));
        vector<vector<int>> r5(m + 1, vector<int>(n + 1));
        vector<vector<int>> c5(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int x = grid[i - 1][j - 1];
                int s2 = 0, s5 = 0;
                for (; x % 2 == 0; x /= 2) {
                    ++s2;
                }
                for (; x % 5 == 0; x /= 5) {
                    ++s5;
                }
                r2[i][j] = r2[i][j - 1] + s2;
                c2[i][j] = c2[i - 1][j] + s2;
                r5[i][j] = r5[i][j - 1] + s5;
                c5[i][j] = c5[i - 1][j] + s5;
            }
        }
        int ans = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                int a = min(r2[i][j] + c2[i - 1][j], r5[i][j] + c5[i - 1][j]);
                int b = min(r2[i][j] + c2[m][j] - c2[i][j], r5[i][j] + c5[m][j] - c5[i][j]);
                int c = min(r2[i][n] - r2[i][j] + c2[i][j], r5[i][n] - r5[i][j] + c5[i][j]);
                int d = min(r2[i][n] - r2[i][j - 1] + c2[m][j] - c2[i][j], r5[i][n] - r5[i][j - 1] + c5[m][j] - c5[i][j]);
                ans = max({ans, a, b, c, d});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxTrailingZeros(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	r2 := get(m+1, n+1)
	c2 := get(m+1, n+1)
	r5 := get(m+1, n+1)
	c5 := get(m+1, n+1)
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			x := grid[i-1][j-1]
			s2, s5 := 0, 0
			for ; x%2 == 0; x /= 2 {
				s2++
			}
			for ; x%5 == 0; x /= 5 {
				s5++
			}
			r2[i][j] = r2[i][j-1] + s2
			c2[i][j] = c2[i-1][j] + s2
			r5[i][j] = r5[i][j-1] + s5
			c5[i][j] = c5[i-1][j] + s5
		}
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			a := min(r2[i][j]+c2[i-1][j], r5[i][j]+c5[i-1][j])
			b := min(r2[i][j]+c2[m][j]-c2[i][j], r5[i][j]+c5[m][j]-c5[i][j])
			c := min(r2[i][n]-r2[i][j]+c2[i][j], r5[i][n]-r5[i][j]+c5[i][j])
			d := min(r2[i][n]-r2[i][j-1]+c2[m][j]-c2[i][j], r5[i][n]-r5[i][j-1]+c5[m][j]-c5[i][j])
			ans = max(ans, max(a, max(b, max(c, d))))
		}
	}
	return
}

func get(m, n int) [][]int {
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, n)
	}
	return f
}
```

#### TypeScript

```ts
function maxTrailingZeros(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const r2 = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));
    const c2 = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));
    const r5 = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));
    const c5 = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            let x = grid[i - 1][j - 1];
            let s2 = 0;
            let s5 = 0;
            for (; x % 2 == 0; x = Math.floor(x / 2)) {
                ++s2;
            }
            for (; x % 5 == 0; x = Math.floor(x / 5)) {
                ++s5;
            }
            r2[i][j] = r2[i][j - 1] + s2;
            c2[i][j] = c2[i - 1][j] + s2;
            r5[i][j] = r5[i][j - 1] + s5;
            c5[i][j] = c5[i - 1][j] + s5;
        }
    }
    let ans = 0;
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            const a = Math.min(r2[i][j] + c2[i - 1][j], r5[i][j] + c5[i - 1][j]);
            const b = Math.min(r2[i][j] + c2[m][j] - c2[i][j], r5[i][j] + c5[m][j] - c5[i][j]);
            const c = Math.min(r2[i][n] - r2[i][j] + c2[i][j], r5[i][n] - r5[i][j] + c5[i][j]);
            const d = Math.min(
                r2[i][n] - r2[i][j - 1] + c2[m][j] - c2[i][j],
                r5[i][n] - r5[i][j - 1] + c5[m][j] - c5[i][j],
            );
            ans = Math.max(ans, a, b, c, d);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
