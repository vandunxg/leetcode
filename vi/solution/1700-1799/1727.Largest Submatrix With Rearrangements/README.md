---
comments: true
difficulty: Medium
rating: 1926
source: Weekly Contest 224 Q3
tags:
    - Greedy
    - Array
    - Matrix
    - Sorting
---

<!-- problem:start -->

# [1727. Largest Submatrix With Rearrangements](https://leetcode.com/problems/largest-submatrix-with-rearrangements)

[中文文档](/solution/1700-1799/1727.Largest%20Submatrix%20With%20Rearrangements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận nhị phân <code>matrix</code> kích thước <code>m x n</code>, trong đó ta được phép sắp xếp lại các <strong>cột</strong> của <code>matrix</code> theo bất kỳ thứ tự nào.</p>

<p>Trả về <em>diện tích của ma trận con lớn nhất trong </em><code>matrix</code><em> mà <strong>mọi</strong> phần tử đều là </em><code>1</code><em> sau khi sắp xếp lại các cột một cách tối ưu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1727.Largest%20Submatrix%20With%20Rearrangements/images/screenshot-2020-12-30-at-40536-pm.png" style="width: 500px; height: 240px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[0,0,1],[1,1,1],[1,0,1]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có thể sắp xếp lại các cột như hình trên.
Ma trận con lớn nhất gồm các số 1, được in đậm, có diện tích 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1727.Largest%20Submatrix%20With%20Rearrangements/images/screenshot-2020-12-30-at-40852-pm.png" style="width: 500px; height: 62px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,0,1,0,1]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có thể sắp xếp lại các cột như hình trên.
Ma trận con lớn nhất gồm các số 1, được in đậm, có diện tích 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[1,1,0],[1,0,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Lưu ý rằng phải sắp xếp lại toàn bộ cột, vì vậy không có cách nào tạo ma trận con gồm các số 1 có diện tích lớn hơn 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>matrix[i][j]</code> is either <code>0</code> or <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các cột có thể được sắp xếp lại; ta cần ma trận con toàn số 1 lớn nhất. $m\cdot n\le 10^5$ khiến việc thử các hoán vị là không thể.
>
> Sắp xếp lại chỉ thay đổi thứ tự cột, không thay đổi đoạn liên tiếp các số 1 hướng lên trong một cột. Sau khi thay mỗi $1$ bằng chiều cao đó, mỗi hàng trở thành một histogram.
>
> Sắp xếp hàng theo thứ tự giảm dần. Chiều cao lớn thứ $k$ là $v$ tạo thành một khối toàn số 1 kích thước $v\times k$. Lấy giá trị lớn nhất trên tất cả các hàng.

<!-- thinking:end -->

Vì ma trận có thể được sắp xếp lại theo cột, trước tiên ta có thể tiền xử lý từng cột của ma trận.

Với mỗi phần tử có giá trị $1$, ta cập nhật giá trị của nó thành số lượng lớn nhất các số $1$ liên tiếp ở phía trên (bao gồm chính nó), tức là $\text{matrix}[i][j] = \text{matrix}[i-1][j] + 1$.

Tiếp theo, ta sắp xếp từng hàng của ma trận đã cập nhật. Sau đó, ta duyệt từng hàng và tính diện tích lớn nhất của ma trận con toàn số $1$ có hàng đó làm cạnh đáy. Cách tính cụ thể như sau:

Với một hàng, gọi phần tử lớn thứ $k$ là $\text{val}_k$, trong đó $k \geq 1$. Khi đó có ít nhất $k$ phần tử trong hàng không nhỏ hơn $\text{val}_k$, tạo thành ma trận con toàn số $1$ có diện tích $\text{val}_k \times k$. Ta duyệt các phần tử của hàng từ lớn đến nhỏ, lấy giá trị lớn nhất của $\text{val}_k \times k$ và cập nhật đáp án.

Độ phức tạp thời gian là $O(m \times n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestSubmatrix(self, matrix: List[List[int]]) -> int:
        for i in range(1, len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j]:
                    matrix[i][j] = matrix[i - 1][j] + 1
        ans = 0
        for row in matrix:
            row.sort(reverse=True)
            for j, v in enumerate(row, 1):
                ans = max(ans, j * v)
        return ans
```

#### Java

```java
class Solution {
    public int largestSubmatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        for (int i = 1; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j] == 1) {
                    matrix[i][j] = matrix[i - 1][j] + 1;
                }
            }
        }
        int ans = 0;
        for (var row : matrix) {
            Arrays.sort(row);
            for (int j = n - 1, k = 1; j >= 0 && row[j] > 0; --j, ++k) {
                int s = row[j] * k;
                ans = Math.max(ans, s);
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
    int largestSubmatrix(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        for (int i = 1; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j]) {
                    matrix[i][j] = matrix[i - 1][j] + 1;
                }
            }
        }
        int ans = 0;
        for (auto& row : matrix) {
            sort(row.rbegin(), row.rend());
            for (int j = 0; j < n; ++j) {
                ans = max(ans, (j + 1) * row[j]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestSubmatrix(matrix [][]int) int {
	m, n := len(matrix), len(matrix[0])
	for i := 1; i < m; i++ {
		for j := 0; j < n; j++ {
			if matrix[i][j] == 1 {
				matrix[i][j] = matrix[i-1][j] + 1
			}
		}
	}
	ans := 0
	for _, row := range matrix {
		sort.Ints(row)
		for j, k := n-1, 1; j >= 0 && row[j] > 0; j, k = j-1, k+1 {
			ans = max(ans, row[j]*k)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function largestSubmatrix(matrix: number[][]): number {
    const m: number = matrix.length;
    const n: number = matrix[0].length;

    for (let i: number = 1; i < m; ++i) {
        for (let j: number = 0; j < n; ++j) {
            if (matrix[i][j] !== 0) {
                matrix[i][j] = matrix[i - 1][j] + 1;
            }
        }
    }

    let ans: number = 0;

    for (const row of matrix) {
        row.sort((a, b) => b - a);
        for (let j: number = 0; j < n; ++j) {
            ans = Math.max(ans, (j + 1) * row[j]);
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn largest_submatrix(mut matrix: Vec<Vec<i32>>) -> i32 {
        let m: usize = matrix.len();
        let n: usize = matrix[0].len();

        for i in 1..m {
            for j in 0..n {
                if matrix[i][j] != 0 {
                    matrix[i][j] = matrix[i - 1][j] + 1;
                }
            }
        }

        let mut ans: i32 = 0;

        for row in matrix.iter_mut() {
            row.sort_unstable_by(|a, b| b.cmp(a));
            for j in 0..n {
                ans = ans.max((j as i32 + 1) * row[j]);
            }
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int LargestSubmatrix(int[][] matrix) {
        int m = matrix.Length;
        int n = matrix[0].Length;

        for (int i = 1; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j] != 0) {
                    matrix[i][j] = matrix[i - 1][j] + 1;
                }
            }
        }

        int ans = 0;

        foreach (var row in matrix) {
            Array.Sort(row);
            Array.Reverse(row);
            for (int j = 0; j < n; ++j) {
                ans = Math.Max(ans, (j + 1) * row[j]);
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
