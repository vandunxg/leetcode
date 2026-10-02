---
comments: true
difficulty: Easy
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [766. Toeplitz Matrix](https://leetcode.com/problems/toeplitz-matrix)

[中文文档](/solution/0700-0799/0766.Toeplitz%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>matrix</code> kích thước <code>m x n</code>, hãy trả về&nbsp;<em><code>true</code>&nbsp;nếu đây là ma trận Toeplitz. Nếu không, trả về <code>false</code>.</em></p>

<p>Một ma trận được gọi là <strong>Toeplitz</strong> nếu mọi đường chéo từ trên trái xuống dưới phải đều gồm các phần tử giống nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0766.Toeplitz%20Matrix/images/ex1.jpg" style="width: 322px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,2,3,4],[5,1,2,3],[9,5,1,2]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Trong ma trận trên, các đường chéo là:
&quot;[9]&quot;, &quot;[5, 5]&quot;, &quot;[1, 1, 1]&quot;, &quot;[2, 2, 2]&quot;, &quot;[3, 3]&quot;, &quot;[4]&quot;.
Mọi phần tử trên từng đường chéo đều giống nhau, vì vậy đáp án là true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0766.Toeplitz%20Matrix/images/ex2.jpg" style="width: 162px; height: 162px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,2],[2,2]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Đường chéo &quot;[1, 2]&quot; có các phần tử khác nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 20</code></li>
	<li><code>0 &lt;= matrix[i][j] &lt;= 99</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Nếu <code>matrix</code> được lưu trên đĩa và bộ nhớ bị giới hạn đến mức mỗi lần chỉ có thể tải tối đa một hàng của ma trận vào bộ nhớ thì sao?</li>
	<li>Nếu <code>matrix</code> quá lớn đến mức mỗi lần chỉ có thể tải một phần của một hàng vào bộ nhớ thì sao?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Trong ma trận Toeplitz, các đường chéo có giá trị bằng nhau, tức mỗi ô bằng ô ở phía trên bên trái nó. Ma trận có kích thước tối đa $20\times 20$.
>
> Duyệt từ $(1,1)$; nếu $matrix[i][j]\ne matrix[i-1][j-1]$ thì trả về false. Hàng đầu tiên và cột đầu tiên không có ô đứng trước.

<!-- thinking:end -->

Theo mô tả bài toán, đặc điểm của ma trận Toeplitz là mỗi phần tử bằng phần tử ở góc trên bên trái của nó. Vì vậy, ta chỉ cần duyệt qua từng phần tử trong ma trận và kiểm tra xem nó có bằng phần tử ở góc trên bên trái hay không.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isToeplitzMatrix(self, matrix: List[List[int]]) -> bool:
        m, n = len(matrix), len(matrix[0])
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][j] != matrix[i - 1][j - 1]:
                    return False
        return True
```

#### Java

```java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][j] != matrix[i - 1][j - 1]) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isToeplitzMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][j] != matrix[i - 1][j - 1]) {
                    return false;
                }
            }
        }
        return true;
    }
};
```

#### Go

```go
func isToeplitzMatrix(matrix [][]int) bool {
	m, n := len(matrix), len(matrix[0])
	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			if matrix[i][j] != matrix[i-1][j-1] {
				return false
			}
		}
	}
	return true
}
```

#### TypeScript

```ts
function isToeplitzMatrix(matrix: number[][]): boolean {
    const [m, n] = [matrix.length, matrix[0].length];
    for (let i = 1; i < m; ++i) {
        for (let j = 1; j < n; ++j) {
            if (matrix[i][j] !== matrix[i - 1][j - 1]) {
                return false;
            }
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_toeplitz_matrix(matrix: Vec<Vec<i32>>) -> bool {
        let (m, n) = (matrix.len(), matrix[0].len());
        for i in 1..m {
            for j in 1..n {
                if matrix[i][j] != matrix[i - 1][j - 1] {
                    return false;
                }
            }
        }
        true
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matrix
 * @return {boolean}
 */
var isToeplitzMatrix = function (matrix) {
    const [m, n] = [matrix.length, matrix[0].length];
    for (let i = 1; i < m; ++i) {
        for (let j = 1; j < n; ++j) {
            if (matrix[i][j] !== matrix[i - 1][j - 1]) {
                return false;
            }
        }
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
