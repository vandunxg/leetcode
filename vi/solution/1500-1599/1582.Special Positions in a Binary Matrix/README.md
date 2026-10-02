---
comments: true
difficulty: Easy
rating: 1321
source: Weekly Contest 206 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [1582. Special Positions in a Binary Matrix](https://leetcode.com/problems/special-positions-in-a-binary-matrix)

[中文文档](/solution/1500-1599/1582.Special%20Positions%20in%20a%20Binary%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>m x n</code> <code>mat</code>, trả về <em>số vị trí đặc biệt trong </em><code>mat</code><em>.</em></p>

<p>Một vị trí <code>(i, j)</code> được gọi là <strong>đặc biệt</strong> nếu <code>mat[i][j] == 1</code> và mọi phần tử khác trong hàng <code>i</code> và cột <code>j</code> đều bằng <code>0</code> (hàng và cột được <strong>đánh số từ 0</strong>).</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1582.Special%20Positions%20in%20a%20Binary%20Matrix/images/special1.jpg" style="width: 244px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[1,0,0],[0,0,1],[1,0,0]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> (1, 2) là vị trí đặc biệt vì mat[1][2] == 1 và mọi phần tử khác trong hàng 1 và cột 2 đều bằng 0.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1582.Special%20Positions%20in%20a%20Binary%20Matrix/images/special-grid.jpg" style="width: 244px; height: 245px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[1,0,0],[0,1,0],[0,0,1]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> (0, 0), (1, 1) và (2, 2) là các vị trí đặc biệt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>mat[i][j]</code> is either <code>0</code> or <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các số 1 là số 1 duy nhất trong hàng và cột của chúng. Kiểm tra lại toàn bộ hàng và cột với mỗi số 1 sẽ tốn $O(mn(m+n))$.
>
> Trước tiên đếm số 1 theo từng hàng và cột, sau đó duyệt lại: một ô được tính khi nó bằng $1$ và cả hai số đếm đều bằng $1$.

<!-- thinking:end -->

Ta có thể dùng hai mảng, $\textit{rows}$ và $\textit{cols}$, để ghi lại số lượng $1$ trong mỗi hàng và mỗi cột.

Sau đó, ta duyệt ma trận. Với mỗi $1$, ta kiểm tra xem hàng và cột của nó có đúng một $1$ hay không. Nếu có, ta tăng đáp án lên một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m + n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận $\textit{mat}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSpecial(self, mat: List[List[int]]) -> int:
        rows = [0] * len(mat)
        cols = [0] * len(mat[0])
        for i, row in enumerate(mat):
            for j, x in enumerate(row):
                rows[i] += x
                cols[j] += x
        ans = 0
        for i, row in enumerate(mat):
            for j, x in enumerate(row):
                ans += x == 1 and rows[i] == 1 and cols[j] == 1
        return ans
```

#### Java

```java
class Solution {
    public int numSpecial(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int[] rows = new int[m];
        int[] cols = new int[n];

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                rows[i] += mat[i][j];
                cols[j] += mat[i][j];
            }
        }

        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (mat[i][j] == 1 && rows[i] == 1 && cols[j] == 1) {
                    ans++;
                }
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
    int numSpecial(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        vector<int> rows(m);
        vector<int> cols(n);

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                rows[i] += mat[i][j];
                cols[j] += mat[i][j];
            }
        }

        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (mat[i][j] == 1 && rows[i] == 1 && cols[j] == 1) {
                    ans++;
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func numSpecial(mat [][]int) (ans int) {
	rows := make([]int, len(mat))
	cols := make([]int, len(mat[0]))
	for i, row := range mat {
		for j, x := range row {
			rows[i] += x
			cols[j] += x
		}
	}
	for i, row := range mat {
		for j, x := range row {
			if x == 1 && rows[i] == 1 && cols[j] == 1 {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function numSpecial(mat: number[][]): number {
    const m = mat.length;
    const n = mat[0].length;
    const rows: number[] = Array(m).fill(0);
    const cols: number[] = Array(n).fill(0);
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            rows[i] += mat[i][j];
            cols[j] += mat[i][j];
        }
    }
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (mat[i][j] === 1 && rows[i] === 1 && cols[j] === 1) {
                ++ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_special(mat: Vec<Vec<i32>>) -> i32 {
        let m = mat.len();
        let n = mat[0].len();
        let mut rows = vec![0; m];
        let mut cols = vec![0; n];
        for i in 0..m {
            for j in 0..n {
                rows[i] += mat[i][j];
                cols[j] += mat[i][j];
            }
        }

        let mut ans = 0;
        for i in 0..m {
            for j in 0..n {
                if mat[i][j] == 1 && rows[i] == 1 && cols[j] == 1 {
                    ans += 1;
                }
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int NumSpecial(int[][] mat) {
        int m = mat.Length;
        int n = mat[0].Length;

        int[] rows = new int[m];
        int[] cols = new int[n];

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                rows[i] += mat[i][j];
                cols[j] += mat[i][j];
            }
        }

        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (mat[i][j] == 1 && rows[i] == 1 && cols[j] == 1) {
                    ans++;
                }
            }
        }

        return ans;
    }
}
```

#### C

```c
int numSpecial(int** mat, int matSize, int* matColSize) {
    int m = matSize, n = matColSize[0];
    int rows[m];
    int cols[n];
    memset(rows, 0, sizeof(rows));
    memset(cols, 0, sizeof(cols));

    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            rows[i] += mat[i][j];
            cols[j] += mat[i][j];
        }
    }

    int ans = 0;
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (mat[i][j] == 1 && rows[i] == 1 && cols[j] == 1) {
                ans++;
            }
        }
    }

    return ans;

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
