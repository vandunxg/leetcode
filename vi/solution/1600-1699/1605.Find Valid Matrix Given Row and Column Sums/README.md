---
comments: true
difficulty: Medium
rating: 1867
source: Biweekly Contest 36 Q3
tags:
    - Greedy
    - Array
    - Matrix
    - Network Flow
---

<!-- problem:start -->

# [1605. Find Valid Matrix Given Row and Column Sums](https://leetcode.com/problems/find-valid-matrix-given-row-and-column-sums)

[中文文档](/solution/1600-1699/1605.Find%20Valid%20Matrix%20Given%20Row%20and%20Column%20Sums/README.md)

## Mô tả

<!-- description:start -->

<p>You are given two arrays <code>rowSum</code> and <code>colSum</code> of non-negative integers where <code>rowSum[i]</code> is the sum of the elements in the <code>i<sup>th</sup></code> row and <code>colSum[j]</code> is the sum of the elements of the <code>j<sup>th</sup></code> column of a 2D matrix. In other words, you do not know the elements of the matrix, but you do know the sums of each row and column.</p>

<p>Find any matrix of <strong>non-negative</strong> integers of size <code>rowSum.length x colSum.length</code> that satisfies the <code>rowSum</code> and <code>colSum</code> requirements.</p>

<p>Return <em>a 2D array representing <strong>any</strong> matrix that fulfills the requirements</em>. It&#39;s guaranteed that <strong>at least one </strong>matrix that fulfills the requirements exists.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> rowSum = [3,8], colSum = [4,7]
<strong>Output:</strong> [[3,0],
         [1,7]]
<strong>Explanation:</strong> 
0<sup>th</sup> row: 3 + 0 = 3 == rowSum[0]
1<sup>st</sup> row: 1 + 7 = 8 == rowSum[1]
0<sup>th</sup> column: 3 + 1 = 4 == colSum[0]
1<sup>st</sup> column: 0 + 7 = 7 == colSum[1]
The row and column sums match, and all matrix elements are non-negative.
Another possible matrix is: [[1,2],
                             [3,5]]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> rowSum = [5,7,10], colSum = [8,6,8]
<strong>Output:</strong> [[0,5,0],
         [6,1,0],
         [2,0,8]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= rowSum.length, colSum.length &lt;= 500</code></li>
	<li><code>0 &lt;= rowSum[i], colSum[i] &lt;= 10<sup>8</sup></code></li>
	<li><code>sum(rowSum) == sum(colSum)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + xây dựng

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải điền một ma trận không âm có tổng hàng và tổng cột khớp với hai mảng đã cho, với $\sum \textit{rowSum} = \sum \textit{colSum}$. Tìm kiếm từng ô là không khả thi khi $m,n \le 500$.
>
> Đặt $x=\min(\textit{rowSum}[i],\textit{colSum}[j])$ tại $(i,j)$ rồi trừ khỏi hai phần còn lại sẽ tạo ra bài toán nhỏ hơn nhưng vẫn nhất quán, vì vậy lựa chọn tham lam là an toàn.
>
> Chỉ cần quét ma trận theo thứ tự hàng trước cột là có thể xây dựng ma trận hợp lệ mà không cần quay lui.

<!-- thinking:end -->

Trước hết, ta khởi tạo ma trận đáp án $ans$ kích thước $m$ x $n$.

Tiếp theo, ta duyệt từng vị trí $(i, j)$, đặt phần tử tại đó bằng $x = \min(rowSum[i], colSum[j])$, rồi lần lượt trừ $x$ khỏi $rowSum[i]$ và $colSum[j]$. Sau khi duyệt hết, ta thu được ma trận $ans$ thỏa mãn yêu cầu.

Tính đúng đắn của chiến lược trên được giải thích như sau:

Theo đề bài, tổng $rowSum$ và $colSum$ bằng nhau, nên $rowSum[0]$ không lớn hơn $\sum_{j = 0}^{n - 1} colSum[j]$. Vì vậy, sau $n$ thao tác, chắc chắn có thể đưa $rowSum[0]$ về $0$, đồng thời luôn đảm bảo $colSum[j] \geq 0$ với mọi $j \in [0, n - 1]$.

Do đó, ta thu gọn bài toán ban đầu thành bài toán con có $m-1$ hàng và $n$ cột, tiếp tục các thao tác trên cho đến khi mọi phần tử trong $rowSum$ và $colSum$ đều bằng $0$; khi đó thu được ma trận $ans$ thỏa mãn yêu cầu.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$, $n$ lần lượt là độ dài của $rowSum$ và $colSum$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def restoreMatrix(self, rowSum: List[int], colSum: List[int]) -> List[List[int]]:
        m, n = len(rowSum), len(colSum)
        ans = [[0] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                x = min(rowSum[i], colSum[j])
                ans[i][j] = x
                rowSum[i] -= x
                colSum[j] -= x
        return ans
```

#### Java

```java
class Solution {
    public int[][] restoreMatrix(int[] rowSum, int[] colSum) {
        int m = rowSum.length;
        int n = colSum.length;
        int[][] ans = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int x = Math.min(rowSum[i], colSum[j]);
                ans[i][j] = x;
                rowSum[i] -= x;
                colSum[j] -= x;
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
    vector<vector<int>> restoreMatrix(vector<int>& rowSum, vector<int>& colSum) {
        int m = rowSum.size(), n = colSum.size();
        vector<vector<int>> ans(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int x = min(rowSum[i], colSum[j]);
                ans[i][j] = x;
                rowSum[i] -= x;
                colSum[j] -= x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func restoreMatrix(rowSum []int, colSum []int) [][]int {
	m, n := len(rowSum), len(colSum)
	ans := make([][]int, m)
	for i := range ans {
		ans[i] = make([]int, n)
	}
	for i := range rowSum {
		for j := range colSum {
			x := min(rowSum[i], colSum[j])
			ans[i][j] = x
			rowSum[i] -= x
			colSum[j] -= x
		}
	}
	return ans
}
```

#### TypeScript

```ts
function restoreMatrix(rowSum: number[], colSum: number[]): number[][] {
    const m = rowSum.length;
    const n = colSum.length;
    const ans = Array.from(new Array(m), () => new Array(n).fill(0));
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            const x = Math.min(rowSum[i], colSum[j]);
            ans[i][j] = x;
            rowSum[i] -= x;
            colSum[j] -= x;
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} rowSum
 * @param {number[]} colSum
 * @return {number[][]}
 */
var restoreMatrix = function (rowSum, colSum) {
    const m = rowSum.length;
    const n = colSum.length;
    const ans = Array.from(new Array(m), () => new Array(n).fill(0));
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            const x = Math.min(rowSum[i], colSum[j]);
            ans[i][j] = x;
            rowSum[i] -= x;
            colSum[j] -= x;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
