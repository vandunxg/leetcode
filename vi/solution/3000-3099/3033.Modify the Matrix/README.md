---
comments: true
difficulty: Easy
rating: 1180
source: Weekly Contest 384 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [3033. Modify the Matrix](https://leetcode.com/problems/modify-the-matrix)

[中文文档](/solution/3000-3099/3033.Modify%20the%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <strong>0-indexed</strong> có kích thước <code>m x n</code> là <code>matrix</code>, hãy tạo một ma trận <strong>0-indexed</strong> mới có tên <code>answer</code>. Đặt <code>answer</code> bằng <code>matrix</code>, sau đó thay mỗi phần tử có giá trị <code>-1</code> bằng phần tử <strong>lớn nhất</strong> trong cột tương ứng.</p>

<p>Hãy trả về <em>ma trận</em> <code>answer</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3033.Modify%20the%20Matrix/images/matrix1.png" style="width: 491px; height: 161px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,2,-1],[4,-1,6],[7,8,9]]
<strong>Đầu ra:</strong> [[1,2,9],[4,8,6],[7,8,9]]
<strong>Giải thích:</strong> Hình minh họa phía trên cho thấy các phần tử được thay đổi (màu xanh dương).
- Ta thay giá trị trong ô [1][1] bằng giá trị lớn nhất trong cột 1, tức là 8.
- Ta thay giá trị trong ô [0][2] bằng giá trị lớn nhất trong cột 2, tức là 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3033.Modify%20the%20Matrix/images/matrix2.png" style="width: 411px; height: 111px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[3,-1],[5,2]]
<strong>Đầu ra:</strong> [[3,2],[5,2]]
<strong>Giải thích:</strong> Hình minh họa phía trên cho thấy các phần tử được thay đổi (màu xanh dương).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>2 &lt;= m, n &lt;= 50</code></li>
	<li><code>-1 &lt;= matrix[i][j] &lt;= 100</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho mỗi cột chứa ít nhất một số nguyên không âm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giá trị $-1$ được thay bằng giá trị lớn nhất trong cột tương ứng. Kích thước ma trận tối đa là $50$, nên chỉ cần duyệt mỗi cột hai lần.
>
> Giá trị lớn nhất của một cột không phụ thuộc vào các lần thay thế, vì vậy trước tiên ta tính giá trị đó, sau đó ghi nó vào mọi vị trí có giá trị $-1$.

<!-- thinking:end -->

Theo mô tả đề bài, ta có thể duyệt từng cột, tìm giá trị lớn nhất của mỗi cột, sau đó lại duyệt từng cột lần nữa và thay các phần tử có giá trị -1 bằng giá trị lớn nhất của cột đó.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def modifiedMatrix(self, matrix: List[List[int]]) -> List[List[int]]:
        m, n = len(matrix), len(matrix[0])
        for j in range(n):
            mx = max(matrix[i][j] for i in range(m))
            for i in range(m):
                if matrix[i][j] == -1:
                    matrix[i][j] = mx
        return matrix
```

#### Java

```java
class Solution {
    public int[][] modifiedMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        for (int j = 0; j < n; ++j) {
            int mx = -1;
            for (int i = 0; i < m; ++i) {
                mx = Math.max(mx, matrix[i][j]);
            }
            for (int i = 0; i < m; ++i) {
                if (matrix[i][j] == -1) {
                    matrix[i][j] = mx;
                }
            }
        }
        return matrix;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> modifiedMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        for (int j = 0; j < n; ++j) {
            int mx = -1;
            for (int i = 0; i < m; ++i) {
                mx = max(mx, matrix[i][j]);
            }
            for (int i = 0; i < m; ++i) {
                if (matrix[i][j] == -1) {
                    matrix[i][j] = mx;
                }
            }
        }
        return matrix;
    }
};
```

#### Go

```go
func modifiedMatrix(matrix [][]int) [][]int {
	m, n := len(matrix), len(matrix[0])
	for j := 0; j < n; j++ {
		mx := -1
		for i := 0; i < m; i++ {
			mx = max(mx, matrix[i][j])
		}
		for i := 0; i < m; i++ {
			if matrix[i][j] == -1 {
				matrix[i][j] = mx
			}
		}
	}
	return matrix
}
```

#### TypeScript

```ts
function modifiedMatrix(matrix: number[][]): number[][] {
    const [m, n] = [matrix.length, matrix[0].length];
    for (let j = 0; j < n; ++j) {
        let mx = -1;
        for (let i = 0; i < m; ++i) {
            mx = Math.max(mx, matrix[i][j]);
        }
        for (let i = 0; i < m; ++i) {
            if (matrix[i][j] === -1) {
                matrix[i][j] = mx;
            }
        }
    }
    return matrix;
}
```

#### C#

```cs
public class Solution {
    public int[][] ModifiedMatrix(int[][] matrix) {
        int m = matrix.Length, n = matrix[0].Length;
        for (int j = 0; j < n; ++j) {
            int mx = -1;
            for (int i = 0; i < m; ++i) {
                mx = Math.Max(mx, matrix[i][j]);
            }
            for (int i = 0; i < m; ++i) {
                if (matrix[i][j] == -1) {
                    matrix[i][j] = mx;
                }
            }
        }
        return matrix;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
