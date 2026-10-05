---
comments: true
difficulty: Medium
rating: 1958
source: Weekly Contest 514 Q3
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [4016. Maximum Area of Two Non-Overlapping Square Submatrices](https://leetcode.com/problems/maximum-area-of-two-non-overlapping-square-submatrices)

[中文文档](/solution/4000-4099/4016.Maximum%20Area%20of%20Two%20Non-Overlapping%20Square%20Submatrices/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên hai chiều <code>mat</code> có kích thước <code>m &times; n</code>, trong đó:</p>

<ul>
	<li><code>mat[r][c] == 1</code> nghĩa là ô ở hàng <code>r</code> và cột <code>c</code> có thể sử dụng.</li>
	<li><code>mat[r][c] == 0</code> nghĩa là ô đó không thể sử dụng.</li>
</ul>

<p>Nhiệm vụ của bạn là tìm <strong>hai <span data-keyword="submatrix">ma trận con</span></strong> thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Cả hai ma trận con đều phải là hình vuông có cùng độ dài cạnh <code>k</code>.</li>
	<li>Hai ma trận con không được dùng chung bất kỳ ô nào.</li>
	<li>Mỗi ma trận con chỉ được phủ các ô có <code>mat[r][c] == 1</code>.</li>
</ul>

<p>Trả về <strong>diện tích lớn nhất có thể</strong> của mỗi trong hai hình vuông. Nếu không thể chọn được hai hình vuông như vậy, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4016.Maximum%20Area%20of%20Two%20Non-Overlapping%20Square%20Submatrices/images/image.png" style="width: 291px; height: 140px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mat = [[1,1,1,0],[1,1,1,1],[0,0,1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai hình vuông lớn nhất, có cùng kích thước và không chồng lấp, có độ dài cạnh <code>k = 2</code> và diện tích bằng 4.</p>

<ul>
	<li>Hình vuông thứ nhất bắt đầu tại góc trên bên trái <code>(0, 0)</code> và phủ các ô <code>(0, 0)</code>, <code>(0, 1)</code>, <code>(1, 0)</code> và <code>(1, 1)</code>.</li>
	<li>Hình vuông thứ hai bắt đầu tại góc trên bên trái <code>(1, 2)</code> và phủ các ô <code>(1, 2)</code>, <code>(1, 3)</code>, <code>(2, 2)</code> và <code>(2, 3)</code>.</li>
</ul>

<p>Do đó, đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4016.Maximum%20Area%20of%20Two%20Non-Overlapping%20Square%20Submatrices/images/screenshot-2026-06-13-at-83728pm.png" style="width: 155px; height: 130px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mat = [[0,1],[1,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai hình vuông lớn nhất, có cùng kích thước và không chồng lấp, có độ dài cạnh <code>k = 1</code> và diện tích bằng 1.</p>

<ul>
	<li>Hình vuông thứ nhất bắt đầu tại góc trên bên trái <code>(0, 1)</code> và phủ ô <code>(0, 1)</code>.</li>
	<li>Hình vuông thứ hai bắt đầu tại góc trên bên trái <code>(1, 0)</code> và phủ ô <code>(1, 0)</code>.</li>
</ul>

<p>Do đó, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/4000-4099/4016.Maximum%20Area%20of%20Two%20Non-Overlapping%20Square%20Submatrices/images/screenshot-2026-06-13-at-83751pm.png" style="width: 152px; height: 125px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mat = [[0,0],[0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có một ô có thể sử dụng, nên không thể chọn hai hình vuông không chồng lấp. Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>mat.length == m</code></li>
	<li><code>mat[i].length == n</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>mat[i][j]</code> bằng 0 hoặc 1.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Liệt kê các đường phân chia

<!-- thinking:start -->

> **Tư duy**
>
> Hai hình vuông có các cạnh song song với trục tọa độ và không chồng lấp có các khoảng hàng rời nhau hoặc các khoảng cột rời nhau, vì vậy luôn có một đường cắt ngang hoặc dọc tách chúng. Liệt kê đường cắt rồi duyệt vét cạn hình vuông toàn số 1 lớn nhất trong mỗi nửa là quá chậm.
>
> Hình vuông lớn nhất có góc tại một ô là bài toán quy hoạch động tiêu chuẩn, còn prefix/suffix maximum theo các hàng cho phép đánh giá mọi đường cắt ngang trong $O(mn)$. Diện tích ứng với một đường cắt là bình phương của độ dài cạnh nhỏ hơn trong hai phía.
>
> Các đường cắt dọc dùng lại cùng routine sau khi chuyển vị ma trận; đáp án là giá trị lớn hơn trong hai hướng.

<!-- thinking:end -->

Hai hình chữ nhật không chồng lấp có các cạnh song song với trục tọa độ luôn có thể được phân tách bằng một đường ngang hoặc một đường dọc (các khoảng hàng hoặc khoảng cột của chúng phải rời nhau). Vì vậy, ta chỉ cần xét trường hợp một hình vuông nằm hoàn toàn phía trên một đường phân chia ngang nào đó và hình vuông còn lại nằm phía dưới đường đó, cũng như trường hợp một hình vuông nằm hoàn toàn bên trái một đường phân chia dọc nào đó và hình vuông còn lại nằm bên phải đường đó. Trường hợp thứ hai có thể xử lý bằng cách chuyển vị ma trận rồi dùng lại logic của trường hợp thứ nhất.

Với trường hợp phân chia ngang, ta thiết kế hàm $\textit{calc}(\textit{mat})$:

- Quy hoạch động từ dưới lên: gọi $f[i][j]$ là độ dài cạnh lớn nhất của một hình vuông toàn số $1$ có góc trên bên trái tại $(i, j)$. Nếu $\textit{mat}[i][j] = 1$, thì $f[i][j] = \min(f[i+1][j], f[i][j+1], f[i+1][j+1]) + 1$. Ta dùng $g[i]$ để ghi nhận độ dài cạnh lớn nhất trên hàng $i$, sau đó tính suffix maximum $\textit{suf}[i] = \max(\textit{suf}[i+1], g[i])$, biểu thị độ dài cạnh lớn nhất của một hình vuông toàn số $1$ trong các hàng $[i, m)$.
- Quy hoạch động từ trên xuống: gọi $f[i][j]$ là độ dài cạnh lớn nhất của một hình vuông toàn số $1$ có góc dưới bên phải tại $(i-1, j-1)$. Nếu $\textit{mat}[i-1][j-1] = 1$, thì $f[i][j] = \min(f[i-1][j], f[i][j-1], f[i-1][j-1]) + 1$. Tương tự, ta tính prefix maximum $\textit{pre}[i] = \max(\textit{pre}[i-1], g[i])$, biểu thị độ dài cạnh lớn nhất của một hình vuông toàn số $1$ trong các hàng $[0, i)$.
- Liệt kê đường phân chia $i \in [1, m)$ giữa mọi cặp hàng liền kề. Độ dài cạnh lớn nhất của một hình vuông toàn số $1$ phía trên đường phân chia là $\textit{pre}[i]$, còn phía dưới là $\textit{suf}[i]$. Vì hai hình vuông phải có cùng độ dài cạnh, độ dài cạnh khả thi là $t = \min(\textit{pre}[i], \textit{suf}[i])$, rồi cập nhật đáp án bằng $t^2$.

Cuối cùng, trả về $\max(\textit{calc}(\textit{mat}), \textit{calc}(\textit{mat}^\top))$.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxArea(self, mat: list[list[int]]) -> int:
        def calc(mat: list[list[int]]) -> int:
            m, n = len(mat), len(mat[0])

            f = [[0] * (n + 1) for _ in range(m + 1)]
            g = [0] * (m + 1)
            suf = [0] * (m + 1)
            for i in range(m - 1, 0, -1):
                for j in range(n - 1, -1, -1):
                    if mat[i][j]:
                        f[i][j] = min(f[i + 1][j], f[i][j + 1], f[i + 1][j + 1]) + 1
                        g[i] = max(g[i], f[i][j])
                suf[i] = max(suf[i + 1], g[i])

            f = [[0] * (n + 1) for _ in range(m + 1)]
            g = [0] * (m + 1)
            pre = [0] * (m + 1)
            for i in range(1, m + 1):
                for j in range(1, n + 1):
                    if mat[i - 1][j - 1]:
                        f[i][j] = min(f[i - 1][j], f[i][j - 1], f[i - 1][j - 1]) + 1
                        g[i] = max(g[i], f[i][j])

                pre[i] = max(pre[i - 1], g[i])

            ans = 0
            for i in range(1, m):
                t = min(pre[i], suf[i])
                ans = max(ans, t * t)
            return ans

        def transpose(mat: list[list[int]]) -> list[list[int]]:
            m, n = len(mat), len(mat[0])
            ans = [[0] * m for _ in range(n)]
            for i in range(m):
                for j in range(n):
                    ans[j][i] = mat[i][j]
            return ans

        return max(calc(mat), calc(transpose(mat)))
```

#### Java

```java
class Solution {
    public int maxArea(int[][] mat) {
        return Math.max(calc(mat), calc(transpose(mat)));
    }

    private int calc(int[][] mat) {
        int m = mat.length, n = mat[0].length;

        int[][] f = new int[m + 1][n + 1];
        int[] g = new int[m + 1];
        int[] suf = new int[m + 1];

        for (int i = m - 1; i > 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (mat[i][j] != 0) {
                    f[i][j] = Math.min(Math.min(f[i + 1][j], f[i][j + 1]), f[i + 1][j + 1]) + 1;
                    g[i] = Math.max(g[i], f[i][j]);
                }
            }
            suf[i] = Math.max(suf[i + 1], g[i]);
        }

        f = new int[m + 1][n + 1];
        g = new int[m + 1];
        int[] pre = new int[m + 1];

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (mat[i - 1][j - 1] != 0) {
                    f[i][j] = Math.min(Math.min(f[i - 1][j], f[i][j - 1]), f[i - 1][j - 1]) + 1;
                    g[i] = Math.max(g[i], f[i][j]);
                }
            }
            pre[i] = Math.max(pre[i - 1], g[i]);
        }

        int ans = 0;
        for (int i = 1; i < m; i++) {
            int t = Math.min(pre[i], suf[i]);
            ans = Math.max(ans, t * t);
        }
        return ans;
    }

    private int[][] transpose(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int[][] ans = new int[n][m];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                ans[j][i] = mat[i][j];
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
    int maxArea(vector<vector<int>>& mat) {
        return max(calc(mat), calc(transpose(mat)));
    }

private:
    int calc(const vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();

        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        vector<int> g(m + 1), suf(m + 1);

        for (int i = m - 1; i > 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (mat[i][j]) {
                    f[i][j] = min({f[i + 1][j],
                                  f[i][j + 1],
                                  f[i + 1][j + 1]})
                        + 1;
                    g[i] = max(g[i], f[i][j]);
                }
            }
            suf[i] = max(suf[i + 1], g[i]);
        }

        f.assign(m + 1, vector<int>(n + 1));
        g.assign(m + 1, 0);
        vector<int> pre(m + 1);

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (mat[i - 1][j - 1]) {
                    f[i][j] = min({f[i - 1][j],
                                  f[i][j - 1],
                                  f[i - 1][j - 1]})
                        + 1;
                    g[i] = max(g[i], f[i][j]);
                }
            }
            pre[i] = max(pre[i - 1], g[i]);
        }

        int ans = 0;
        for (int i = 1; i < m; i++) {
            int t = min(pre[i], suf[i]);
            ans = max(ans, t * t);
        }

        return ans;
    }

    vector<vector<int>> transpose(const vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();

        vector<vector<int>> ans(n, vector<int>(m));

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                ans[j][i] = mat[i][j];
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxArea(mat [][]int) int {
	return max(calc(mat), calc(transpose(mat)))
}

func calc(mat [][]int) int {
	m, n := len(mat), len(mat[0])

	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	g := make([]int, m+1)
	suf := make([]int, m+1)

	for i := m - 1; i > 0; i-- {
		for j := n - 1; j >= 0; j-- {
			if mat[i][j] != 0 {
				f[i][j] = min(
					f[i+1][j],
					f[i][j+1],
					f[i+1][j+1],
				) + 1
				if f[i][j] > g[i] {
					g[i] = f[i][j]
				}
			}
		}
		suf[i] = max(suf[i+1], g[i])
	}

	f = make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	g = make([]int, m+1)
	pre := make([]int, m+1)

	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if mat[i-1][j-1] != 0 {
				f[i][j] = min(
					f[i-1][j],
					f[i][j-1],
					f[i-1][j-1],
				) + 1
				if f[i][j] > g[i] {
					g[i] = f[i][j]
				}
			}
		}
		pre[i] = max(pre[i-1], g[i])
	}

	ans := 0
	for i := 1; i < m; i++ {
		t := min(pre[i], suf[i])
		if t*t > ans {
			ans = t * t
		}
	}
	return ans
}

func transpose(mat [][]int) [][]int {
	m, n := len(mat), len(mat[0])
	ans := make([][]int, n)
	for i := range ans {
		ans[i] = make([]int, m)
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			ans[j][i] = mat[i][j]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxArea(mat: number[][]): number {
    return Math.max(calc(mat), calc(transpose(mat)));
}

function calc(mat: number[][]): number {
    const m = mat.length;
    const n = mat[0].length;

    let f = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    let g = Array(m + 1).fill(0);
    let suf = Array(m + 1).fill(0);

    for (let i = m - 1; i > 0; i--) {
        for (let j = n - 1; j >= 0; j--) {
            if (mat[i][j]) {
                f[i][j] = Math.min(f[i + 1][j], f[i][j + 1], f[i + 1][j + 1]) + 1;
                g[i] = Math.max(g[i], f[i][j]);
            }
        }
        suf[i] = Math.max(suf[i + 1], g[i]);
    }

    f = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    g = Array(m + 1).fill(0);
    const pre = Array(m + 1).fill(0);

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (mat[i - 1][j - 1]) {
                f[i][j] = Math.min(f[i - 1][j], f[i][j - 1], f[i - 1][j - 1]) + 1;
                g[i] = Math.max(g[i], f[i][j]);
            }
        }
        pre[i] = Math.max(pre[i - 1], g[i]);
    }

    let ans = 0;
    for (let i = 1; i < m; i++) {
        const t = Math.min(pre[i], suf[i]);
        ans = Math.max(ans, t * t);
    }
    return ans;
}

function transpose(mat: number[][]): number[][] {
    const m = mat.length;
    const n = mat[0].length;

    const ans = Array.from({ length: n }, () => Array(m).fill(0));

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            ans[j][i] = mat[i][j];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
