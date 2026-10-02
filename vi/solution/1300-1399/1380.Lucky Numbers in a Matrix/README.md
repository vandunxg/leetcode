---
comments: true
difficulty: Easy
rating: 1207
source: Weekly Contest 180 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [1380. Lucky Numbers in a Matrix](https://leetcode.com/problems/lucky-numbers-in-a-matrix)

[中文文档](/solution/1300-1399/1380.Lucky%20Numbers%20in%20a%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> gồm các số <strong>đôi một khác nhau</strong>, hãy trả về <em>tất cả <strong>số may mắn</strong> trong ma trận theo <strong>bất kỳ thứ tự nào</strong></em>.</p>

<p><strong>Số may mắn</strong> là phần tử nhỏ nhất trong hàng của nó và đồng thời là phần tử lớn nhất trong cột của nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[3,7,8],[9,11,13],[15,16,17]]
<strong>Đầu ra:</strong> [15]
<strong>Giải thích:</strong> 15 là số may mắn duy nhất vì nó nhỏ nhất trong hàng và lớn nhất trong cột.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[1,10,4,2],[9,3,8,7],[15,16,17,12]]
<strong>Đầu ra:</strong> [12]
<strong>Giải thích:</strong> 12 là số may mắn duy nhất vì nó nhỏ nhất trong hàng và lớn nhất trong cột.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[7,8],[1,2]]
<strong>Đầu ra:</strong> [7]
<strong>Giải thích:</strong> 7 là số may mắn duy nhất vì nó nhỏ nhất trong hàng và lớn nhất trong cột.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= n, m &lt;= 50</code></li>
	<li><code>1 &lt;= matrix[i][j] &lt;= 10<sup>5</sup></code>.</li>
	<li>Tất cả phần tử trong ma trận đôi một khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lưu giá trị nhỏ nhất theo hàng và lớn nhất theo cột

<!-- thinking:start -->

> **Tư duy**
>
> Số may mắn là giá trị nhỏ nhất trong hàng và lớn nhất trong cột của nó. Vì mọi phần tử đều khác nhau, giao giữa tập các giá trị nhỏ nhất theo hàng và tập các giá trị lớn nhất theo cột chính là đáp án.

<!-- thinking:end -->

Ta có thể dùng hai mảng $rows$ và $cols$ để lưu giá trị nhỏ nhất của mỗi hàng và giá trị lớn nhất của mỗi cột trong ma trận. Sau đó, duyệt từng phần tử và kiểm tra xem nó có phải giá trị nhỏ nhất trong hàng đồng thời là giá trị lớn nhất trong cột hay không. Nếu đúng, phần tử đó là số may mắn và được thêm vào mảng kết quả.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def luckyNumbers(self, matrix: List[List[int]]) -> List[int]:
        rows = {min(row) for row in matrix}
        cols = {max(col) for col in zip(*matrix)}
        return list(rows & cols)
```

#### Java

```java
class Solution {
    public List<Integer> luckyNumbers(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int[] rows = new int[m];
        int[] cols = new int[n];
        Arrays.fill(rows, 1 << 30);
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                rows[i] = Math.min(rows[i], matrix[i][j]);
                cols[j] = Math.max(cols[j], matrix[i][j]);
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (rows[i] == cols[j]) {
                    ans.add(rows[i]);
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
    vector<int> luckyNumbers(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        int rows[m];
        int cols[n];
        memset(rows, 0x3f, sizeof(rows));
        memset(cols, 0, sizeof(cols));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                rows[i] = min(rows[i], matrix[i][j]);
                cols[j] = max(cols[j], matrix[i][j]);
            }
        }
        vector<int> ans;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (rows[i] == cols[j]) {
                    ans.push_back(rows[i]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func luckyNumbers(matrix [][]int) (ans []int) {
	m, n := len(matrix), len(matrix[0])
	rows, cols := make([]int, m), make([]int, n)
	for i := range rows {
		rows[i] = 1 << 30
	}
	for i, row := range matrix {
		for j, x := range row {
			rows[i] = min(rows[i], x)
			cols[j] = max(cols[j], x)
		}
	}
	for i, row := range matrix {
		for j, x := range row {
			if rows[i] == cols[j] {
				ans = append(ans, x)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function luckyNumbers(matrix: number[][]): number[] {
    const m = matrix.length;
    const n = matrix[0].length;
    const rows: number[] = new Array(m).fill(1 << 30);
    const cols: number[] = new Array(n).fill(0);
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; j++) {
            rows[i] = Math.min(rows[i], matrix[i][j]);
            cols[j] = Math.max(cols[j], matrix[i][j]);
        }
    }
    const ans: number[] = [];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; j++) {
            if (rows[i] === cols[j]) {
                ans.push(rows[i]);
            }
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
var luckyNumbers = function (matrix) {
    const m = matrix.length;
    const n = matrix[0].length;
    const rows = new Array(m).fill(1 << 30);
    const cols = new Array(n).fill(0);
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; j++) {
            rows[i] = Math.min(rows[i], matrix[i][j]);
            cols[j] = Math.max(cols[j], matrix[i][j]);
        }
    }
    const ans = [];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; j++) {
            if (rows[i] === cols[j]) {
                ans.push(rows[i]);
            }
        }
    }
    return ans;
};
```

#### Rust

```rust
impl Solution {
    pub fn lucky_numbers(matrix: Vec<Vec<i32>>) -> Vec<i32> {
        let m = matrix.len();
        let n = matrix[0].len();
        let mut res = vec![];
        let mut col = vec![0; n];
        for j in 0..n {
            for i in 0..m {
                col[j] = col[j].max(matrix[i][j]);
            }
        }
        for x in 0..m {
            let mut i = 0;
            for y in 1..n {
                if matrix[x][y] < matrix[x][i] {
                    i = y;
                }
            }
            if matrix[x][i] == col[i] {
                res.push(col[i]);
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
