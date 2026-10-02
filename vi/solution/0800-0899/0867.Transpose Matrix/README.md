---
comments: true
difficulty: Easy
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [867. Transpose Matrix](https://leetcode.com/problems/transpose-matrix)

[中文文档](/solution/0800-0899/0867.Transpose%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên hai chiều <code>matrix</code>, hãy trả về <em>ma trận <strong>chuyển vị</strong> của</em> <code>matrix</code>.</p>

<p><strong>Chuyển vị</strong> một ma trận là lật ma trận qua đường chéo chính, hoán đổi chỉ số hàng và cột.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0867.Transpose%20Matrix/images/hint_transpose.png" style="width: 600px; height: 197px;" /></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[1,2,3],[4,5,6],[7,8,9]]
<strong>Đầu ra:</strong> [[1,4,7],[2,5,8],[3,6,9]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[1,2,3],[4,5,6]]
<strong>Đầu ra:</strong> [[1,4],[2,5],[3,6]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 1000</code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= matrix[i][j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chuyển vị hoán đổi hàng và cột. Ma trận đủ nhỏ để tạo ma trận mới, không cần đổi chỗ tại chỗ.
>
> Dùng $\textit{zip}$ trên ma trận sẽ biến mỗi cột thành một hàng mới.

<!-- thinking:end -->

Gọi $m$ là số hàng và $n$ là số cột của ma trận $\textit{matrix}$. Theo định nghĩa phép chuyển vị, ma trận chuyển vị $\textit{ans}$ sẽ có $n$ hàng và $m$ cột.

Mỗi vị trí $(i, j)$ trong $\textit{ans}$ tương ứng với vị trí $(j, i)$ trong ma trận $\textit{matrix}$. Vì vậy, ta duyệt từng phần tử trong ma trận $\textit{matrix}$ và chuyển vị nó đến vị trí tương ứng trong $\textit{ans}$.

Sau khi duyệt xong, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận $\textit{matrix}$. Không tính phần bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def transpose(self, matrix: List[List[int]]) -> List[List[int]]:
        return [list(row) for row in zip(*matrix)]
```

#### Java

```java
class Solution {
    public int[][] transpose(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int[][] ans = new int[n][m];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                ans[i][j] = matrix[j][i];
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
    vector<vector<int>> transpose(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> ans(n, vector<int>(m));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                ans[i][j] = matrix[j][i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func transpose(matrix [][]int) [][]int {
	m, n := len(matrix), len(matrix[0])
	ans := make([][]int, n)
	for i := range ans {
		ans[i] = make([]int, m)
		for j := range ans[i] {
			ans[i][j] = matrix[j][i]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function transpose(matrix: number[][]): number[][] {
    const [m, n] = [matrix.length, matrix[0].length];
    const ans: number[][] = Array.from({ length: n }, () => Array(m).fill(0));
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < m; ++j) {
            ans[i][j] = matrix[j][i];
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matrix
 * @return {number[][]}
 */
var transpose = function (matrix) {
    const [m, n] = [matrix.length, matrix[0].length];
    const ans = Array.from({ length: n }, () => Array(m).fill(0));
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < m; ++j) {
            ans[i][j] = matrix[j][i];
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
