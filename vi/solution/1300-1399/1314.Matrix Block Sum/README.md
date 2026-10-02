---
comments: true
difficulty: Medium
rating: 1483
source: Biweekly Contest 17 Q2
tags:
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [1314. Matrix Block Sum](https://leetcode.com/problems/matrix-block-sum)

[中文文档](/solution/1300-1399/1314.Matrix%20Block%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> <code>mat</code> và số nguyên <code>k</code>, hãy trả về <em>ma trận</em> <code>answer</code>, <em>trong đó mỗi phần tử</em> <code>answer[i][j]</code> <em>bằng tổng tất cả phần tử</em> <code>mat[r][c]</code> <em>thỏa mãn</em>:</p>

<ul>
	<li><code>i - k &lt;= r &lt;= i + k,</code></li>
	<li><code>j - k &lt;= c &lt;= j + k</code>, và</li>
	<li><code>(r, c)</code> là vị trí hợp lệ trong ma trận.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[1,2,3],[4,5,6],[7,8,9]], k = 1
<strong>Đầu ra:</strong> [[12,21,16],[27,45,33],[24,39,28]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[1,2,3],[4,5,6],[7,8,9]], k = 2
<strong>Đầu ra:</strong> [[45,45,45],[45,45,45],[45,45,45]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m ==&nbsp;mat.length</code></li>
	<li><code>n ==&nbsp;mat[i].length</code></li>
	<li><code>1 &lt;= m, n, k &lt;= 100</code></li>
	<li><code>1 &lt;= mat[i][j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố hai chiều

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi ô, cần tính tổng của vùng lân cận hình vuông. Nếu $k$ lớn, tính tổng trực tiếp mỗi lần sẽ tốn $O(mnk^2)$. Nhiều ô cần tính các vùng chồng lấp; mảng tổng tiền tố 2D cho phép tính tổng một hình chữ nhật bằng bốn giá trị ở các góc.
>
> Xây dựng $s[i][j]$ là tổng của vùng $i \times j$ ở góc trên bên trái, giới hạn mỗi cửa sổ trong phạm vi ma trận, rồi tính $s[x_2+1][y_2+1]-s[x_1][y_2+1]-s[x_2+1][y_1]+s[x_1][y_1]$.

<!-- thinking:end -->

Bài này là dạng mẫu của kỹ thuật tổng tiền tố hai chiều.

Ta định nghĩa $s[i][j]$ là tổng các phần tử trong $i$ hàng đầu tiên và $j$ cột đầu tiên của ma trận $mat$. Công thức tính $s[i][j]$ là:

$$
s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + mat[i-1][j-1]
$$

Nhờ đó, ta có thể nhanh chóng tính tổng các phần tử trong bất kỳ vùng hình chữ nhật nào bằng mảng $s$.

Với vùng hình chữ nhật có tọa độ góc trên bên trái là $(x_1, y_1)$ và góc dưới bên phải là $(x_2, y_2)$, ta có thể tính tổng các phần tử trong vùng đó bằng mảng $s$:

$$
s[x_2+1][y_2+1] - s[x_1][y_2+1] - s[x_2+1][y_1] + s[x_1][y_1]
$$

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def matrixBlockSum(self, mat: List[List[int]], k: int) -> List[List[int]]:
        m, n = len(mat), len(mat[0])
        s = [[0] * (n + 1) for _ in range(m + 1)]
        for i, row in enumerate(mat, 1):
            for j, x in enumerate(row, 1):
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + x
        ans = [[0] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                x1, y1 = max(i - k, 0), max(j - k, 0)
                x2, y2 = min(m - 1, i + k), min(n - 1, j + k)
                ans[i][j] = (
                    s[x2 + 1][y2 + 1] - s[x1][y2 + 1] - s[x2 + 1][y1] + s[x1][y1]
                )
        return ans
```

#### Java

```java
class Solution {
    public int[][] matrixBlockSum(int[][] mat, int k) {
        int m = mat.length;
        int n = mat[0].length;
        int[][] s = new int[m + 1][n + 1];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                s[i + 1][j + 1] = s[i][j + 1] + s[i + 1][j] - s[i][j] + mat[i][j];
            }
        }

        int[][] ans = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int x1 = Math.max(i - k, 0);
                int y1 = Math.max(j - k, 0);
                int x2 = Math.min(m - 1, i + k);
                int y2 = Math.min(n - 1, j + k);
                ans[i][j] = s[x2 + 1][y2 + 1] - s[x1][y2 + 1] - s[x2 + 1][y1] + s[x1][y1];
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
    vector<vector<int>> matrixBlockSum(vector<vector<int>>& mat, int k) {
        int m = mat.size();
        int n = mat[0].size();

        vector<vector<int>> s(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                s[i + 1][j + 1] = s[i][j + 1] + s[i + 1][j] - s[i][j] + mat[i][j];
            }
        }

        vector<vector<int>> ans(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int x1 = max(i - k, 0);
                int y1 = max(j - k, 0);
                int x2 = min(m - 1, i + k);
                int y2 = min(n - 1, j + k);
                ans[i][j] = s[x2 + 1][y2 + 1] - s[x1][y2 + 1] - s[x2 + 1][y1] + s[x1][y1];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func matrixBlockSum(mat [][]int, k int) [][]int {
	m, n := len(mat), len(mat[0])
	s := make([][]int, m+1)
	for i := range s {
		s[i] = make([]int, n+1)
	}
	for i, row := range mat {
		for j, x := range row {
			s[i+1][j+1] = s[i][j+1] + s[i+1][j] - s[i][j] + x
		}
	}

	ans := make([][]int, m)
	for i := range ans {
		ans[i] = make([]int, n)
	}

	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			x1 := max(i-k, 0)
			y1 := max(j-k, 0)
			x2 := min(m-1, i+k)
			y2 := min(n-1, j+k)
			ans[i][j] = s[x2+1][y2+1] - s[x1][y2+1] - s[x2+1][y1] + s[x1][y1]
		}
	}

	return ans
}
```

#### TypeScript

```ts
function matrixBlockSum(mat: number[][], k: number): number[][] {
    const m: number = mat.length;
    const n: number = mat[0].length;

    const s: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            s[i + 1][j + 1] = s[i][j + 1] + s[i + 1][j] - s[i][j] + mat[i][j];
        }
    }

    const ans: number[][] = Array.from({ length: m }, () => Array(n).fill(0));
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            const x1: number = Math.max(i - k, 0);
            const y1: number = Math.max(j - k, 0);
            const x2: number = Math.min(m - 1, i + k);
            const y2: number = Math.min(n - 1, j + k);
            ans[i][j] = s[x2 + 1][y2 + 1] - s[x1][y2 + 1] - s[x2 + 1][y1] + s[x1][y1];
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
