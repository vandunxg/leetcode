---
comments: true
difficulty: Medium
rating: 1583
source: Weekly Contest 328 Q2
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [2536. Increment Submatrices by One](https://leetcode.com/problems/increment-submatrices-by-one)

[Tài liệu tiếng Trung](/solution/2500-2599/2536.Increment%20Submatrices%20by%20One/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>, ban đầu có một ma trận số nguyên <code>mat</code> kích thước <code>n x n</code>, đánh chỉ số từ <strong>0</strong> và được điền toàn số 0.</p>

<p>Bạn cũng được cho một mảng số nguyên 2D <code>query</code>. Với mỗi <code>query[i] = [row1<sub>i</sub>, col1<sub>i</sub>, row2<sub>i</sub>, col2<sub>i</sub>]</code>, bạn cần thực hiện thao tác sau:</p>

<ul>
	<li>Cộng <code>1</code> vào <strong>mọi phần tử</strong> trong ma trận con có góc <strong>trên bên trái</strong> là <code>(row1<sub>i</sub>, col1<sub>i</sub>)</code> và góc <strong>dưới bên phải</strong> là <code>(row2<sub>i</sub>, col2<sub>i</sub>)</code>. Cụ thể, cộng <code>1</code> vào <code>mat[x][y]</code> với mọi <code>row1<sub>i</sub> &lt;= x &lt;= row2<sub>i</sub></code> và <code>col1<sub>i</sub> &lt;= y &lt;= col2<sub>i</sub></code>.</li>
</ul>

<p>Trả về <em>ma trận</em> <code>mat</code><em> sau khi thực hiện tất cả các truy vấn.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2536.Increment%20Submatrices%20by%20One/images/p2example11.png" style="width: 531px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, queries = [[1,1,2,2],[0,0,1,1]]
<strong>Đầu ra:</strong> [[1,1,0],[1,2,1],[0,1,1]]
<strong>Giải thích:</strong> Hình trên minh họa ma trận ban đầu, ma trận sau truy vấn thứ nhất và ma trận sau truy vấn thứ hai.
- Trong truy vấn thứ nhất, ta cộng 1 vào mọi phần tử trong ma trận con có góc trên bên trái (1, 1) và góc dưới bên phải (2, 2).
- Trong truy vấn thứ hai, ta cộng 1 vào mọi phần tử trong ma trận con có góc trên bên trái (0, 0) và góc dưới bên phải (1, 1).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2536.Increment%20Submatrices%20by%20One/images/p2example22.png" style="width: 261px; height: 82px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, queries = [[0,0,1,1]]
<strong>Đầu ra:</strong> [[1,1],[1,1]]
<strong>Giải thích:</strong> Hình trên minh họa ma trận ban đầu và ma trận sau truy vấn thứ nhất.
- Trong truy vấn thứ nhất, ta cộng 1 vào mọi phần tử trong ma trận.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= row1<sub>i</sub> &lt;= row2<sub>i</sub> &lt; n</code></li>
	<li><code>0 &lt;= col1<sub>i</sub> &lt;= col2<sub>i</sub> &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu 2D

<!-- thinking:start -->

> **Tư duy**
>
> Khi cần tăng nhiều ma trận con lên một đơn vị, việc duyệt qua mọi ô của từng truy vấn sẽ tốn nhiều thời gian khi cả $q$ và $n$ đều có thể đạt tới $500$.
>
> Mảng hiệu 2D biến thao tác tăng một hình chữ nhật thành bốn lần cập nhật tại các góc với độ phức tạp $O(1)$: cộng $+1$ ở góc trên bên trái, cộng $+1$ ngay sau góc dưới bên phải, và trừ $-1$ ở hai góc còn lại. Sau khi xử lý tất cả truy vấn, ta khôi phục ma trận bằng tổng tiền tố 2D.

<!-- thinking:end -->

Mảng hiệu 2D là một kỹ thuật dùng để xử lý hiệu quả các cập nhật trên một mảng 2D. Ta có thể cập nhật nhanh các ma trận con bằng cách duy trì một ma trận hiệu có cùng kích thước với ma trận ban đầu.

Giả sử ta có một ma trận hiệu 2D $\textit{diff}$, ban đầu mọi phần tử đều bằng $0$. Với mỗi truy vấn $[\textit{row1}, \textit{col1}, \textit{row2}, \textit{col2}]$, ta cập nhật ma trận hiệu theo các bước sau:

1. Tăng vị trí $(\textit{row1}, \textit{col1})$ lên $1$.
2. Giảm vị trí $(\textit{row2} + 1, \textit{col1})$ đi $1$, nếu $\textit{row2} + 1 < n$.
3. Giảm vị trí $(\textit{row1}, \textit{col2} + 1)$ đi $1$, nếu $\textit{col2} + 1 < n$.
4. Tăng vị trí $(\textit{row2} + 1, \textit{col2} + 1)$ lên $1$, nếu $\textit{row2} + 1 < n$ và $\textit{col2} + 1 < n$.

Sau khi hoàn tất tất cả truy vấn, ta cần chuyển ma trận hiệu trở lại ma trận ban đầu bằng các tổng tiền tố. Cụ thể, với mỗi vị trí $(i, j)$, ta tính:

$$
\textit{mat}[i][j] = \textit{diff}[i][j] + (\textit{mat}[i-1][j] \text{ if } i > 0 \text{ else } 0) + (\textit{mat}[i][j-1] \text{ if } j > 0 \text{ else } 0) - (\textit{mat}[i-1][j-1] \text{ if } i > 0 \text{ and } j > 0 \text{ else } 0)
$$

Độ phức tạp thời gian là $O(m + n^2)$, trong đó $m$ và $n$ lần lượt là độ dài của $\textit{queries}$ và giá trị $n$ đã cho. Không tính phần không gian được dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rangeAddQueries(self, n: int, queries: List[List[int]]) -> List[List[int]]:
        mat = [[0] * n for _ in range(n)]
        for x1, y1, x2, y2 in queries:
            mat[x1][y1] += 1
            if x2 + 1 < n:
                mat[x2 + 1][y1] -= 1
            if y2 + 1 < n:
                mat[x1][y2 + 1] -= 1
            if x2 + 1 < n and y2 + 1 < n:
                mat[x2 + 1][y2 + 1] += 1

        for i in range(n):
            for j in range(n):
                if i:
                    mat[i][j] += mat[i - 1][j]
                if j:
                    mat[i][j] += mat[i][j - 1]
                if i and j:
                    mat[i][j] -= mat[i - 1][j - 1]
        return mat
```

#### Java

```java
class Solution {
    public int[][] rangeAddQueries(int n, int[][] queries) {
        int[][] mat = new int[n][n];
        for (var q : queries) {
            int x1 = q[0], y1 = q[1], x2 = q[2], y2 = q[3];
            mat[x1][y1]++;
            if (x2 + 1 < n) {
                mat[x2 + 1][y1]--;
            }
            if (y2 + 1 < n) {
                mat[x1][y2 + 1]--;
            }
            if (x2 + 1 < n && y2 + 1 < n) {
                mat[x2 + 1][y2 + 1]++;
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i > 0) {
                    mat[i][j] += mat[i - 1][j];
                }
                if (j > 0) {
                    mat[i][j] += mat[i][j - 1];
                }
                if (i > 0 && j > 0) {
                    mat[i][j] -= mat[i - 1][j - 1];
                }
            }
        }
        return mat;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> rangeAddQueries(int n, vector<vector<int>>& queries) {
        vector<vector<int>> mat(n, vector<int>(n));
        for (auto& q : queries) {
            int x1 = q[0], y1 = q[1], x2 = q[2], y2 = q[3];
            mat[x1][y1]++;
            if (x2 + 1 < n) {
                mat[x2 + 1][y1]--;
            }
            if (y2 + 1 < n) {
                mat[x1][y2 + 1]--;
            }
            if (x2 + 1 < n && y2 + 1 < n) {
                mat[x2 + 1][y2 + 1]++;
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i > 0) {
                    mat[i][j] += mat[i - 1][j];
                }
                if (j > 0) {
                    mat[i][j] += mat[i][j - 1];
                }
                if (i > 0 && j > 0) {
                    mat[i][j] -= mat[i - 1][j - 1];
                }
            }
        }
        return mat;
    }
};
```

#### Go

```go
func rangeAddQueries(n int, queries [][]int) [][]int {
	mat := make([][]int, n)
	for i := range mat {
		mat[i] = make([]int, n)
	}
	for _, q := range queries {
		x1, y1, x2, y2 := q[0], q[1], q[2], q[3]
		mat[x1][y1]++
		if x2+1 < n {
			mat[x2+1][y1]--
		}
		if y2+1 < n {
			mat[x1][y2+1]--
		}
		if x2+1 < n && y2+1 < n {
			mat[x2+1][y2+1]++
		}
	}
	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if i > 0 {
				mat[i][j] += mat[i-1][j]
			}
			if j > 0 {
				mat[i][j] += mat[i][j-1]
			}
			if i > 0 && j > 0 {
				mat[i][j] -= mat[i-1][j-1]
			}
		}
	}
	return mat
}
```

#### TypeScript

```ts
function rangeAddQueries(n: number, queries: number[][]): number[][] {
    const mat: number[][] = Array.from({ length: n }, () => Array(n).fill(0));

    for (const [x1, y1, x2, y2] of queries) {
        mat[x1][y1] += 1;
        if (x2 + 1 < n) mat[x2 + 1][y1] -= 1;
        if (y2 + 1 < n) mat[x1][y2 + 1] -= 1;
        if (x2 + 1 < n && y2 + 1 < n) mat[x2 + 1][y2 + 1] += 1;
    }

    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (i > 0) mat[i][j] += mat[i - 1][j];
            if (j > 0) mat[i][j] += mat[i][j - 1];
            if (i > 0 && j > 0) mat[i][j] -= mat[i - 1][j - 1];
        }
    }

    return mat;
}
```

#### Rust

```rust
impl Solution {
    pub fn range_add_queries(n: i32, queries: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let n = n as usize;
        let mut mat = vec![vec![0; n]; n];

        for q in queries {
            let (x1, y1, x2, y2) = (q[0] as usize, q[1] as usize, q[2] as usize, q[3] as usize);
            mat[x1][y1] += 1;
            if x2 + 1 < n {
                mat[x2 + 1][y1] -= 1;
            }
            if y2 + 1 < n {
                mat[x1][y2 + 1] -= 1;
            }
            if x2 + 1 < n && y2 + 1 < n {
                mat[x2 + 1][y2 + 1] += 1;
            }
        }

        for i in 0..n {
            for j in 0..n {
                if i > 0 {
                    mat[i][j] += mat[i - 1][j];
                }
                if j > 0 {
                    mat[i][j] += mat[i][j - 1];
                }
                if i > 0 && j > 0 {
                    mat[i][j] -= mat[i - 1][j - 1];
                }
            }
        }

        mat
    }
}
```

#### C#

```cs
public class Solution {
    public int[][] RangeAddQueries(int n, int[][] queries) {
        int[][] mat = new int[n][];
        for (int i = 0; i < n; i++) {
            mat[i] = new int[n];
        }

        foreach (var q in queries) {
            int x1 = q[0], y1 = q[1], x2 = q[2], y2 = q[3];

            mat[x1][y1]++;

            if (x2 + 1 < n) {
                mat[x2 + 1][y1]--;
            }
            if (y2 + 1 < n) {
                mat[x1][y2 + 1]--;
            }
            if (x2 + 1 < n && y2 + 1 < n) {
                mat[x2 + 1][y2 + 1]++;
            }
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i > 0) {
                    mat[i][j] += mat[i - 1][j];
                }
                if (j > 0) {
                    mat[i][j] += mat[i][j - 1];
                }
                if (i > 0 && j > 0) {
                    mat[i][j] -= mat[i - 1][j - 1];
                }
            }
        }

        return mat;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
