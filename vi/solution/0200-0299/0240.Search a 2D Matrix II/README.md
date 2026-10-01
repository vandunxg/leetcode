---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Divide and Conquer
    - Matrix
---

<!-- problem:start -->

# [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii)

[中文文档](/solution/0200-0299/0240.Search%20a%202D%20Matrix%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một thuật toán hiệu quả để tìm giá trị <code>target</code> trong ma trận số nguyên <code>matrix</code> kích thước <code>m x n</code>. Ma trận này có các tính chất sau:</p>

<ul>
	<li>Các số nguyên trong mỗi hàng được sắp xếp tăng dần từ trái sang phải.</li>
	<li>Các số nguyên trong mỗi cột được sắp xếp tăng dần từ trên xuống dưới.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0240.Search%20a%202D%20Matrix%20II/images/searchgrid2.jpg" style="width: 300px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0240.Search%20a%202D%20Matrix%20II/images/searchgrid.jpg" style="width: 300px; height: 300px;" />
<pre>
<strong>Đầu vào:</strong> matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= n, m &lt;= 300</code></li>
	<li><code>-10<sup>9</sup> &lt;= matrix[i][j] &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả các số nguyên trong mỗi hàng đều được <strong>sắp xếp</strong> theo thứ tự tăng dần.</li>
	<li>Tất cả các số nguyên trong mỗi cột đều được <strong>sắp xếp</strong> theo thứ tự tăng dần.</li>
	<li><code>-10<sup>9</sup> &lt;= target &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng (và mỗi cột) đều được sắp xếp, vì vậy chúng ta có thể tìm kiếm nhị phân $target$ trong từng hàng. Cách này có độ phức tạp $O(m\log n)$ và không cần thêm bộ nhớ.

<!-- thinking:end -->

Vì tất cả phần tử trong mỗi hàng đều được sắp xếp tăng dần, với mỗi hàng, chúng ta có thể dùng tìm kiếm nhị phân để tìm phần tử đầu tiên lớn hơn hoặc bằng $\textit{target}$, sau đó kiểm tra xem phần tử đó có bằng $\textit{target}$ hay không. Nếu bằng $\textit{target}$, nghĩa là đã tìm thấy giá trị đích và chúng ta trả về $\text{true}$. Nếu không bằng $\textit{target}$, nghĩa là tất cả phần tử trong hàng này đều nhỏ hơn $\textit{target}$, nên chúng ta tiếp tục tìm kiếm ở hàng tiếp theo.

Nếu đã tìm kiếm tất cả các hàng mà vẫn không tìm thấy giá trị đích, nghĩa là giá trị đích không tồn tại và chúng ta trả về $\text{false}$.

Độ phức tạp thời gian là $O(m \times \log n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        for row in matrix:
            j = bisect_left(row, target)
            if j < len(matrix[0]) and row[j] == target:
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        for (var row : matrix) {
            int j = Arrays.binarySearch(row, target);
            if (j >= 0) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        for (auto& row : matrix) {
            int j = lower_bound(row.begin(), row.end(), target) - row.begin();
            if (j < matrix[0].size() && row[j] == target) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func searchMatrix(matrix [][]int, target int) bool {
	for _, row := range matrix {
		j := sort.SearchInts(row, target)
		if j < len(matrix[0]) && row[j] == target {
			return true
		}
	}
	return false
}
```

#### TypeScript

```ts
function searchMatrix(matrix: number[][], target: number): boolean {
    const n = matrix[0].length;
    for (const row of matrix) {
        const j = _.sortedIndex(row, target);
        if (j < n && row[j] === target) {
            return true;
        }
    }
    return false;
}
```

#### Rust

```rust
use std::cmp::Ordering;

impl Solution {
    pub fn search_matrix(matrix: Vec<Vec<i32>>, target: i32) -> bool {
        let m = matrix.len();
        let n = matrix[0].len();
        let mut i = 0;
        let mut j = n;
        while i < m && j > 0 {
            match target.cmp(&matrix[i][j - 1]) {
                Ordering::Less => {
                    j -= 1;
                }
                Ordering::Greater => {
                    i += 1;
                }
                Ordering::Equal => {
                    return true;
                }
            }
        }
        false
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var searchMatrix = function (matrix, target) {
    const n = matrix[0].length;
    for (const row of matrix) {
        const j = _.sortedIndex(row, target);
        if (j < n && row[j] == target) {
            return true;
        }
    }
    return false;
};
```

#### C#

```cs
public class Solution {
    public bool SearchMatrix(int[][] matrix, int target) {
        foreach (int[] row in matrix) {
            int j = Array.BinarySearch(row, target);
            if (j >= 0) {
                return true;
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tìm kiếm từ góc dưới bên trái hoặc góc trên bên phải

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm nhị phân theo từng hàng bỏ qua thứ tự của các cột. Từ góc dưới bên trái, nếu giá trị quá lớn thì di chuyển lên (loại bỏ phần phía trên của cột đó), còn nếu giá trị quá nhỏ thì di chuyển sang phải (loại bỏ phần bên phải của hàng đó).
>
> Mỗi bước loại bỏ một hàng hoặc một cột, nên thời gian là $O(m+n)$.

<!-- thinking:end -->

Bắt đầu tìm kiếm từ góc dưới bên trái hoặc góc trên bên phải, rồi di chuyển dần về phía góc trên bên phải hoặc góc dưới bên trái. So sánh phần tử hiện tại $\textit{matrix}[i][j]$ với $\textit{target}$:

- Nếu $\textit{matrix}[i][j] = \textit{target}$, nghĩa là đã tìm thấy giá trị đích và chúng ta trả về $\text{true}$.
- Nếu $\textit{matrix}[i][j] > \textit{target}$, nghĩa là tất cả phần tử trong cột này từ vị trí hiện tại trở lên đều lớn hơn $\textit{target}$, nên chúng ta di chuyển con trỏ $i$ lên trên, tức là $i \leftarrow i - 1$.
- Nếu $\textit{matrix}[i][j] < \textit{target}$, nghĩa là tất cả phần tử trong hàng này từ vị trí hiện tại sang phải đều nhỏ hơn $\textit{target}$, nên chúng ta di chuyển con trỏ $j$ sang phải, tức là $j \leftarrow j + 1$.

Nếu kết thúc tìm kiếm mà không tìm thấy $\textit{target}$, trả về $\text{false}$.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        i, j = m - 1, 0
        while i >= 0 and j < n:
            if matrix[i][j] == target:
                return True
            if matrix[i][j] > target:
                i -= 1
            else:
                j += 1
        return False
```

#### Java

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int i = m - 1, j = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] == target) {
                return true;
            }
            if (matrix[i][j] > target) {
                --i;
            } else {
                ++j;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int i = m - 1, j = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] == target) {
                return true;
            }
            if (matrix[i][j] > target) {
                --i;
            } else {
                ++j;
            }
        }
        return false;
    }
};
```

#### Go

```go
func searchMatrix(matrix [][]int, target int) bool {
	m, n := len(matrix), len(matrix[0])
	i, j := m-1, 0
	for i >= 0 && j < n {
		if matrix[i][j] == target {
			return true
		}
		if matrix[i][j] > target {
			i--
		} else {
			j++
		}
	}
	return false
}
```

#### TypeScript

```ts
function searchMatrix(matrix: number[][], target: number): boolean {
    const [m, n] = [matrix.length, matrix[0].length];
    let [i, j] = [m - 1, 0];
    while (i >= 0 && j < n) {
        if (matrix[i][j] === target) {
            return true;
        }
        if (matrix[i][j] > target) {
            --i;
        } else {
            ++j;
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn search_matrix(matrix: Vec<Vec<i32>>, target: i32) -> bool {
        let m = matrix.len();
        let n = matrix[0].len();
        let mut i = m - 1;
        let mut j = 0;
        while i >= 0 && j < n {
            if matrix[i][j] == target {
                return true;
            }
            if matrix[i][j] > target {
                if i == 0 {
                    break;
                }
                i -= 1;
            } else {
                j += 1;
            }
        }
        false
    }
}
```

#### C#

```cs
public class Solution {
    public bool SearchMatrix(int[][] matrix, int target) {
        int m = matrix.Length, n = matrix[0].Length;
        int i = m - 1, j = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] == target) {
                return true;
            }
            if (matrix[i][j] > target) {
                --i;
            } else {
                ++j;
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
