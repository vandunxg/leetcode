---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [01.08. Zero Matrix](https://leetcode.cn/problems/zero-matrix-lcci)

[中文文档](/lcci/01.08.Zero%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một thuật toán sao cho nếu một phần tử trong ma trận MxN là 0, toàn bộ hàng và cột chứa phần tử đó được đặt thành 0.</p>

<p>&nbsp;</p>

<p><strong>Ví dụ 1: </strong></p>

<pre>

<strong>Đầu vào: </strong>

[

  [1,1,1],

  [1,0,1],

  [1,1,1]

]

<strong>Đầu ra: </strong>

[

  [1,0,1],

  [0,0,0],

  [1,0,1]

]

</pre>

<p><strong>Ví dụ 2: </strong></p>

<pre>

<strong>Đầu vào: </strong>

[

  [0,1,2,0],

  [3,4,5,2],

  [1,3,1,5]

]

<strong>Đầu ra: </strong>

[

  [0,0,0,0],

  [0,4,5,0],

  [0,3,1,0]

]

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đánh dấu bằng mảng

<!-- thinking:start -->

> **Tư duy**
>
> Một số 0 buộc toàn bộ hàng và cột chứa nó về 0. Nếu xóa ngay trong lúc duyệt, các số 0 vừa ghi sẽ bị xem như số 0 ban đầu và làm lan truyền sai.
>
> Phải ghi lại đầy đủ các hàng và cột cần xóa trước khi ghi bất kỳ giá trị nào.
>
> Các mảng $rows$ và $cols$ được điền ở lượt duyệt đầu tiên và áp dụng ở lượt thứ hai. Hai lượt duyệt tuyến tính cùng $O(m+n)$ không gian bổ sung giúp tránh nhầm lẫn khi đọc/ghi.

<!-- thinking:end -->

Chúng ta dùng các mảng `rows` và `cols` để đánh dấu các hàng và cột cần đặt về 0.

Sau đó, chúng ta duyệt lại ma trận, đặt về 0 các phần tử tương ứng với những hàng và cột đã được đánh dấu trong `rows` và `cols`.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m + n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        m, n = len(matrix), len(matrix[0])
        rows = [0] * m
        cols = [0] * n
        for i, row in enumerate(matrix):
            for j, v in enumerate(row):
                if v == 0:
                    rows[i] = cols[j] = 1
        for i in range(m):
            for j in range(n):
                if rows[i] or cols[j]:
                    matrix[i][j] = 0
```

#### Java

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean[] rows = new boolean[m];
        boolean[] cols = new boolean[n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    rows[i] = true;
                    cols[j] = true;
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (rows[i] || cols[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        vector<bool> rows(m);
        vector<bool> cols(n);
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (!matrix[i][j]) {
                    rows[i] = 1;
                    cols[j] = 1;
                }
            }
        }
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (rows[i] || cols[j]) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
};
```

#### Go

```go
func setZeroes(matrix [][]int) {
	m, n := len(matrix), len(matrix[0])
	rows := make([]bool, m)
	cols := make([]bool, n)
	for i, row := range matrix {
		for j, v := range row {
			if v == 0 {
				rows[i] = true
				cols[j] = true
			}
		}
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if rows[i] || cols[j] {
				matrix[i][j] = 0
			}
		}
	}
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify matrix in-place instead.
 */
function setZeroes(matrix: number[][]): void {
    const m = matrix.length;
    const n = matrix[0].length;
    const rows = new Array(m).fill(false);
    const cols = new Array(n).fill(false);
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (matrix[i][j] === 0) {
                rows[i] = true;
                cols[j] = true;
            }
        }
    }
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (rows[i] || cols[j]) {
                matrix[i][j] = 0;
            }
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn set_zeroes(matrix: &mut Vec<Vec<i32>>) {
        let m = matrix.len();
        let n = matrix[0].len();
        let mut rows = vec![false; m];
        let mut cols = vec![false; n];
        for i in 0..m {
            for j in 0..n {
                if matrix[i][j] == 0 {
                    rows[i] = true;
                    cols[j] = true;
                }
            }
        }
        for i in 0..m {
            for j in 0..n {
                if rows[i] || cols[j] {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
var setZeroes = function (matrix) {
    const m = matrix.length;
    const n = matrix[0].length;
    const rows = new Array(m).fill(false);
    const cols = new Array(n).fill(false);
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (matrix[i][j] == 0) {
                rows[i] = true;
                cols[j] = true;
            }
        }
    }
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (rows[i] || cols[j]) {
                matrix[i][j] = 0;
            }
        }
    }
};
```

#### C

```c
void setZeroes(int** matrix, int matrixSize, int* matrixColSize) {
    int m = matrixSize;
    int n = matrixColSize[0];
    int* rows = (int*) malloc(sizeof(int) * m);
    int* cols = (int*) malloc(sizeof(int) * n);
    memset(rows, 0, sizeof(int) * m);
    memset(cols, 0, sizeof(int) * n);
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; ++j) {
            if (matrix[i][j] == 0) {
                rows[i] = 1;
                cols[j] = 1;
            }
        }
    }
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; ++j) {
            if (rows[i] || cols[j]) {
                matrix[i][j] = 0;
            }
        }
    }
    free(rows);
    free(cols);
}
```

#### Swift

```swift
class Solution {
    func setZeroes(_ matrix: inout [[Int]]) {
        let m = matrix.count
        guard m > 0 else { return }
        let n = matrix[0].count
        var rows = Array(repeating: false, count: m)
        var cols = Array(repeating: false, count: n)

        for i in 0..<m {
            for j in 0..<n {
                if matrix[i][j] == 0 {
                    rows[i] = true
                    cols[j] = true
                }
            }
        }

        for i in 0..<m {
            for j in 0..<n {
                if rows[i] || cols[j] {
                    matrix[i][j] = 0
                }
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Đánh dấu tại chỗ

<!-- thinking:start -->

> **Tư duy**
>
> Có thể thay các mảng bổ sung trong Lời giải 1 bằng hàng đầu tiên và cột đầu tiên của ma trận để làm marker.
>
> Hai đường này vừa là marker vừa là dữ liệu, nên $i0$ và $j0$ ghi nhớ xem ban đầu chúng có chứa số 0 hay không. Các ô khác vẫn cần hai lượt: ghi marker, áp dụng marker, rồi xử lý hàng đầu tiên và cột đầu tiên theo $i0$ và $j0$. Không gian bổ sung giảm xuống còn $O(1)$.

<!-- thinking:end -->

Trong Lời giải 1, chúng ta dùng các mảng bổ sung để đánh dấu các hàng và cột cần đặt về 0. Thực ra, chúng ta có thể trực tiếp dùng hàng đầu tiên và cột đầu tiên của ma trận để đánh dấu mà không cần cấp phát thêm không gian cho mảng.

Vì hàng đầu tiên và cột đầu tiên được dùng để đánh dấu, các giá trị của chúng có thể thay đổi do thao tác đánh dấu. Do đó, chúng ta cần thêm các biến $i0$ và $j0$ để đánh dấu xem hàng đầu tiên và cột đầu tiên có cần được đặt về 0 hay không.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        m, n = len(matrix), len(matrix[0])
        i0 = any(v == 0 for v in matrix[0])
        j0 = any(matrix[i][0] == 0 for i in range(m))
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][j] == 0:
                    matrix[i][0] = matrix[0][j] = 0
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0
        if i0:
            for j in range(n):
                matrix[0][j] = 0
        if j0:
            for i in range(m):
                matrix[i][0] = 0
```

#### Java

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean i0 = false, j0 = false;
        for (int j = 0; j < n; ++j) {
            if (matrix[0][j] == 0) {
                i0 = true;
                break;
            }
        }
        for (int i = 0; i < m; ++i) {
            if (matrix[i][0] == 0) {
                j0 = true;
                break;
            }
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (i0) {
            for (int j = 0; j < n; ++j) {
                matrix[0][j] = 0;
            }
        }
        if (j0) {
            for (int i = 0; i < m; ++i) {
                matrix[i][0] = 0;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        bool i0 = false, j0 = false;
        for (int j = 0; j < n; ++j) {
            if (matrix[0][j] == 0) {
                i0 = true;
                break;
            }
        }
        for (int i = 0; i < m; ++i) {
            if (matrix[i][0] == 0) {
                j0 = true;
                break;
            }
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (i0) {
            for (int j = 0; j < n; ++j) {
                matrix[0][j] = 0;
            }
        }
        if (j0) {
            for (int i = 0; i < m; ++i) {
                matrix[i][0] = 0;
            }
        }
    }
};
```

#### Go

```go
func setZeroes(matrix [][]int) {
	m, n := len(matrix), len(matrix[0])
	i0, j0 := false, false
	for j := 0; j < n; j++ {
		if matrix[0][j] == 0 {
			i0 = true
			break
		}
	}
	for i := 0; i < m; i++ {
		if matrix[i][0] == 0 {
			j0 = true
			break
		}
	}
	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			if matrix[i][j] == 0 {
				matrix[i][0], matrix[0][j] = 0, 0
			}
		}
	}
	for i := 1; i < m; i++ {
		for j := 1; j < n; j++ {
			if matrix[i][0] == 0 || matrix[0][j] == 0 {
				matrix[i][j] = 0
			}
		}
	}
	if i0 {
		for j := 0; j < n; j++ {
			matrix[0][j] = 0
		}
	}
	if j0 {
		for i := 0; i < m; i++ {
			matrix[i][0] = 0
		}
	}
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify matrix in-place instead.
 */
function setZeroes(matrix: number[][]): void {
    const m = matrix.length;
    const n = matrix[0].length;
    let l0 = false;
    let r0 = false;
    for (let i = 0; i < m; i++) {
        if (matrix[i][0] === 0) {
            l0 = true;
            break;
        }
    }
    for (let j = 0; j < n; j++) {
        if (matrix[0][j] === 0) {
            r0 = true;
            break;
        }
    }
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (matrix[i][j] === 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (matrix[i][0] === 0 || matrix[0][j] === 0) {
                matrix[i][j] = 0;
            }
        }
    }
    if (l0) {
        for (let i = 0; i < m; i++) {
            matrix[i][0] = 0;
        }
    }
    if (r0) {
        for (let j = 0; j < n; j++) {
            matrix[0][j] = 0;
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn set_zeroes(matrix: &mut Vec<Vec<i32>>) {
        let m = matrix.len();
        let n = matrix[0].len();
        let l0 = {
            let mut res = false;
            for j in 0..n {
                if matrix[0][j] == 0 {
                    res = true;
                    break;
                }
            }
            res
        };
        let r0 = {
            let mut res = false;
            for i in 0..m {
                if matrix[i][0] == 0 {
                    res = true;
                    break;
                }
            }
            res
        };
        for i in 0..m {
            for j in 0..n {
                if matrix[i][j] == 0 {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for i in 1..m {
            for j in 1..n {
                if matrix[i][0] == 0 || matrix[0][j] == 0 {
                    matrix[i][j] = 0;
                }
            }
        }
        if l0 {
            for j in 0..n {
                matrix[0][j] = 0;
            }
        }
        if r0 {
            for i in 0..m {
                matrix[i][0] = 0;
            }
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
var setZeroes = function (matrix) {
    const m = matrix.length;
    const n = matrix[0].length;
    let i0 = matrix[0].some(v => v == 0);
    let j0 = false;
    for (let i = 0; i < m; ++i) {
        if (matrix[i][0] == 0) {
            j0 = true;
            break;
        }
    }
    for (let i = 1; i < m; ++i) {
        for (let j = 1; j < n; ++j) {
            if (matrix[i][j] == 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    for (let i = 1; i < m; ++i) {
        for (let j = 1; j < n; ++j) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                matrix[i][j] = 0;
            }
        }
    }
    if (i0) {
        for (let j = 0; j < n; ++j) {
            matrix[0][j] = 0;
        }
    }
    if (j0) {
        for (let i = 0; i < m; ++i) {
            matrix[i][0] = 0;
        }
    }
};
```

#### C

```c
void setZeroes(int** matrix, int matrixSize, int* matrixColSize) {
    int m = matrixSize;
    int n = matrixColSize[0];
    int l0 = 0;
    int r0 = 0;
    for (int i = 0; i < m; i++) {
        if (matrix[i][0] == 0) {
            l0 = 1;
            break;
        }
    }
    for (int j = 0; j < n; j++) {
        if (matrix[0][j] == 0) {
            r0 = 1;
            break;
        }
    }
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (matrix[i][j] == 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                matrix[i][j] = 0;
            }
        }
    }
    if (l0) {
        for (int i = 0; i < m; i++) {
            matrix[i][0] = 0;
        }
    }
    if (r0) {
        for (int j = 0; j < n; j++) {
            matrix[0][j] = 0;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
