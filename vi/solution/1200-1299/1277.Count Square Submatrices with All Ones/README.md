---
comments: true
difficulty: Medium
rating: 1613
source: Weekly Contest 165 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [1277. Count Square Submatrices with All Ones](https://leetcode.com/problems/count-square-submatrices-with-all-ones)

[中文文档](/solution/1200-1299/1277.Count%20Square%20Submatrices%20with%20All%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m * n</code> gồm các số 0 và 1, hãy trả về số lượng ma trận con <strong>vuông</strong> chỉ chứa số 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix =
[
&nbsp; [0,1,1,1],
&nbsp; [1,1,1,1],
&nbsp; [0,1,1,1]
]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> 
Có <strong>10</strong> hình vuông cạnh 1.
Có <strong>4</strong> hình vuông cạnh 2.
Có <strong>1</strong> hình vuông cạnh 3.
Tổng số hình vuông = 10 + 4 + 1 = <strong>15</strong>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> 
Có <b>6</b> hình vuông cạnh 1.  
Có <strong>1</strong> hình vuông cạnh 2. 
Tổng số hình vuông = 6 + 1 = <b>7</b>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length&nbsp;&lt;= 300</code></li>
	<li><code>1 &lt;= arr[0].length&nbsp;&lt;= 300</code></li>
	<li><code>0 &lt;= arr[i][j] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta đếm các hình vuông chỉ gồm số $1$. Vì $m,n \le 300$, duyệt mọi hình vuông sẽ tốn $O(n^3)$. Kích thước hình vuông lớn nhất có góc dưới bên phải tại $(i,j)$ bị giới hạn bởi các hình vuông ở ô phía trên, bên trái và trên-trái.
>
> $f[i][j]$ là độ dài cạnh đó: nếu ô hiện tại là $1$, lấy giá trị nhỏ nhất trong ba ô kia rồi cộng $1$. Mỗi ô như vậy đóng góp $f[i][j]$ hình vuông (có cạnh từ $1\ldots f$). DP giúp đếm bằng một lượt điền bảng.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là độ dài cạnh của ma trận con vuông có góc dưới bên phải tại $(i,j)$. Ban đầu $f[i][j] = 0$, và đáp án là $\sum_{i,j} f[i][j]$.

Xét cách chuyển state cho $f[i][j]$:

- Nếu $\text{matrix}[i][j] = 0$, ta có $f[i][j] = 0$.
- Nếu $\text{matrix}[i][j] = 1$, giá trị state $f[i][j]$ phụ thuộc vào ba vị trí phía trên, bên trái và trên-trái:
    - Nếu $i = 0$ hoặc $j = 0$, thì $f[i][j] = 1$.
    - Ngược lại, $f[i][j] = \min(f[i-1][j-1], f[i-1][j], f[i][j-1]) + 1$.

Đáp án là $\sum_{i,j} f[i][j]$.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        m, n = len(matrix), len(matrix[0])
        f = [[0] * n for _ in range(m)]
        ans = 0
        for i, row in enumerate(matrix):
            for j, v in enumerate(row):
                if v == 0:
                    continue
                if i == 0 or j == 0:
                    f[i][j] = 1
                else:
                    f[i][j] = min(f[i - 1][j - 1], f[i - 1][j], f[i][j - 1]) + 1
                ans += f[i][j]
        return ans
```

#### Java

```java
class Solution {
    public int countSquares(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        int[][] f = new int[m][n];
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    continue;
                }
                if (i == 0 || j == 0) {
                    f[i][j] = 1;
                } else {
                    f[i][j] = Math.min(f[i - 1][j - 1], Math.min(f[i - 1][j], f[i][j - 1])) + 1;
                }
                ans += f[i][j];
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
    int countSquares(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        int ans = 0;
        vector<vector<int>> f(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    continue;
                }
                if (i == 0 || j == 0) {
                    f[i][j] = 1;
                } else {
                    f[i][j] = min(f[i - 1][j - 1], min(f[i - 1][j], f[i][j - 1])) + 1;
                }
                ans += f[i][j];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSquares(matrix [][]int) int {
	m, n, ans := len(matrix), len(matrix[0]), 0
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, n)
	}
	for i, row := range matrix {
		for j, v := range row {
			if v == 0 {
				continue
			}
			if i == 0 || j == 0 {
				f[i][j] = 1
			} else {
				f[i][j] = min(f[i-1][j-1], min(f[i-1][j], f[i][j-1])) + 1
			}
			ans += f[i][j]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countSquares(matrix: number[][]): number {
    const m = matrix.length;
    const n = matrix[0].length;
    const f: number[][] = Array.from({ length: m }, () => Array(n).fill(0));
    let ans = 0;

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (matrix[i][j] === 0) {
                continue;
            }
            if (i === 0 || j === 0) {
                f[i][j] = 1;
            } else {
                f[i][j] = Math.min(f[i - 1][j - 1], Math.min(f[i - 1][j], f[i][j - 1])) + 1;
            }
            ans += f[i][j];
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_squares(matrix: Vec<Vec<i32>>) -> i32 {
        let m = matrix.len();
        let n = matrix[0].len();
        let mut f = vec![vec![0; n]; m];
        let mut ans = 0;

        for i in 0..m {
            for j in 0..n {
                if matrix[i][j] == 0 {
                    continue;
                }
                if i == 0 || j == 0 {
                    f[i][j] = 1;
                } else {
                    f[i][j] = std::cmp::min(f[i - 1][j - 1], std::cmp::min(f[i - 1][j], f[i][j - 1])) + 1;
                }
                ans += f[i][j];
            }
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matrix
 * @return {number}
 */
var countSquares = function (matrix) {
    const m = matrix.length;
    const n = matrix[0].length;
    const f = Array.from({ length: m }, () => Array(n).fill(0));
    let ans = 0;

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (matrix[i][j] === 0) {
                continue;
            }
            if (i === 0 || j === 0) {
                f[i][j] = 1;
            } else {
                f[i][j] = Math.min(f[i - 1][j - 1], Math.min(f[i - 1][j], f[i][j - 1])) + 1;
            }
            ans += f[i][j];
        }
    }

    return ans;
};
```

#### C#

```cs
public class Solution {
    public int CountSquares(int[][] matrix) {
        int m = matrix.Length;
        int n = matrix[0].Length;
        int[,] f = new int[m, n];
        int ans = 0;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    continue;
                }
                if (i == 0 || j == 0) {
                    f[i, j] = 1;
                } else {
                    f[i, j] = Math.Min(f[i - 1, j - 1], Math.Min(f[i - 1, j], f[i, j - 1])) + 1;
                }
                ans += f[i, j];
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
