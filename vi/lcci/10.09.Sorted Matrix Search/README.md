---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [10.09. Sorted Matrix Search](https://leetcode.cn/problems/sorted-matrix-search-lcci)

[中文文档](/lcci/10.09.Sorted%20Matrix%20Search/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận M x N trong đó mỗi hàng và mỗi cột đều được sắp xếp theo thứ tự tăng dần, hãy viết một phương thức để tìm một phần tử.</p>

<p><strong>Ví dụ:</strong></p>

<p>Cho ma trận:</p>

<pre>

[

  [1,   4,  7, 11, 15],

  [2,   5,  8, 12, 19],

  [3,   6,  9, 16, 22],

  [10, 13, 14, 17, 24],

  [18, 21, 23, 26, 30]

]

</pre>

<p>Với target&nbsp;=&nbsp;5,&nbsp;trả về&nbsp;<code>true.</code></p>

<p>Với target&nbsp;=&nbsp;20, trả về&nbsp;<code>false.</code></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Các hàng và cột tăng dần; tìm $target$. Duyệt toàn bộ là $O(mn)$. Mỗi hàng đã được sắp xếp nên có thể dùng tìm kiếm nhị phân.
>
> Dùng $bisect\_left$ trên mỗi hàng và trả về khi tìm thấy. Không tận dụng tính đơn điệu của các cột; chi phí là $O(m\log n)$.
>
> Cách triển khai duyệt qua các hàng và tìm kiếm, với không gian bổ sung $O(1)$, ngắn gọn khi $m$ không lớn.

<!-- thinking:end -->

Vì tất cả phần tử trong mỗi hàng đều được sắp xếp theo thứ tự tăng dần, chúng ta có thể sử dụng tìm kiếm nhị phân để tìm phần tử đầu tiên lớn hơn hoặc bằng `target` trong mỗi hàng, sau đó kiểm tra xem phần tử này có bằng `target` hay không. Nếu bằng `target`, nghĩa là đã tìm thấy giá trị mục tiêu, và chúng ta trả về `true` ngay lập tức. Nếu không bằng `target`, nghĩa là tất cả phần tử trong hàng này đều nhỏ hơn `target`, và chúng ta nên tiếp tục tìm kiếm ở hàng tiếp theo.

Nếu đã tìm kiếm tất cả các hàng mà vẫn chưa tìm thấy giá trị mục tiêu, nghĩa là giá trị mục tiêu không tồn tại, nên trả về `false`.

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
        let left = 0,
            right = n;
        while (left < right) {
            const mid = (left + right) >> 1;
            if (row[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        if (left != n && row[left] == target) {
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
        let left = 0,
            right = n;
        while (left < right) {
            const mid = (left + right) >> 1;
            if (row[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        if (left != n && row[left] == target) {
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

#### Swift

```swift
class Solution {
    func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {
        for row in matrix {
            if binarySearch(row, target) {
                return true
            }
        }
        return false
    }

    private func binarySearch(_ array: [Int], _ target: Int) -> Bool {
        var left = 0
        var right = array.count - 1

        while left <= right {
            let mid = left + (right - left) / 2
            if array[mid] == target {
                return true
            } else if array[mid] < target {
                left = mid + 1
            } else {
                right = mid - 1
            }
        }

        return false
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
> Lời giải 1 không tận dụng tính tăng dần của các cột và vẫn có thể phải kiểm tra mọi hàng.
>
> Từ góc dưới bên trái (hoặc góc trên bên phải): nếu giá trị hiện tại quá lớn thì di chuyển lên trên và loại bỏ phần lớn hơn của cột đó; nếu quá nhỏ thì di chuyển sang phải và loại bỏ phần nhỏ hơn của hàng đó. Mỗi bước loại bỏ một hàng hoặc một cột, với độ phức tạp $O(m+n)$.

<!-- thinking:end -->

Ở đây, chúng ta bắt đầu tìm kiếm từ góc dưới bên trái và di chuyển về phía trên bên phải, so sánh phần tử hiện tại `matrix[i][j]` với `target`:

- Nếu $\textit{matrix}[i][j] = \textit{target}$, nghĩa là đã tìm thấy giá trị mục tiêu, và chúng ta trả về `true` ngay lập tức.
- Nếu $\textit{matrix}[i][j] > \textit{target}$, nghĩa là tất cả phần tử trong cột này từ vị trí hiện tại trở lên đều lớn hơn `target`, vì vậy chúng ta nên di chuyển con trỏ $i$ lên trên, tức là $i \leftarrow i - 1$.
- Nếu $\textit{matrix}[i][j] < \textit{target}$, nghĩa là tất cả phần tử trong hàng này từ vị trí hiện tại sang phải đều nhỏ hơn `target`, vì vậy chúng ta nên di chuyển con trỏ $j$ sang phải, tức là $j \leftarrow j + 1$.

Nếu kết thúc tìm kiếm mà vẫn chưa tìm thấy `target`, hãy trả về `false`.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix:
            return False
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
        if (matrix == null || matrix.length == 0) {
            return false;
        }
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
        if (matrix.empty()) {
            return false;
        }
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
	if len(matrix) == 0 {
		return false
	}
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
    if (matrix.length === 0) {
        return false;
    }
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

#### C#

```cs
public class Solution {
    public bool SearchMatrix(int[][] matrix, int target) {
        if (matrix.Length == 0) {
            return false;
        }
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
