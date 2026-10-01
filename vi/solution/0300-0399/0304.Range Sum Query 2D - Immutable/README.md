---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [304. Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable)

[中文文档](/solution/0300-0399/0304.Range%20Sum%20Query%202D%20-%20Immutable/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận hai chiều <code>matrix</code>, hãy xử lý nhiều truy vấn thuộc dạng sau:</p>

<ul>
	<li>Tính <strong>tổng</strong> các phần tử của <code>matrix</code> nằm trong hình chữ nhật được xác định bởi <strong>góc trên bên trái</strong> <code>(row1, col1)</code> và <strong>góc dưới bên phải</strong> <code>(row2, col2)</code>.</li>
</ul>

<p>Hãy cài đặt class <code>NumMatrix</code>:</p>

<ul>
	<li><code>NumMatrix(int[][] matrix)</code> Khởi tạo object với ma trận số nguyên <code>matrix</code>.</li>
	<li><code>int sumRegion(int row1, int col1, int row2, int col2)</code> Trả về <strong>tổng</strong> các phần tử của <code>matrix</code> nằm trong hình chữ nhật được xác định bởi <strong>góc trên bên trái</strong> <code>(row1, col1)</code> và <strong>góc dưới bên phải</strong> <code>(row2, col2)</code>.</li>
</ul>

<p>Hãy thiết kế thuật toán để <code>sumRegion</code> chạy với độ phức tạp thời gian <code>O(1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0304.Range%20Sum%20Query%202D%20-%20Immutable/images/sum-grid.jpg" style="width: 415px; height: 415px;" />
<pre>
<strong>Đầu vào</strong>
[&quot;NumMatrix&quot;, &quot;sumRegion&quot;, &quot;sumRegion&quot;, &quot;sumRegion&quot;]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [1, 1, 2, 2], [1, 2, 2, 4]]
<strong>Đầu ra</strong>
[null, 8, 11, 12]

<strong>Giải thích</strong>
NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e sum of the red rectangle)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (i.e sum of the green rectangle)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (i.e sum of the blue rectangle)
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>-10<sup>4</sup> &lt;= matrix[i][j] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= row1 &lt;= row2 &lt; m</code></li>
	<li><code>0 &lt;= col1 &lt;= col2 &lt; n</code></li>
	<li>Có tối đa <code>10<sup>4</sup></code> lần gọi <code>sumRegion</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố hai chiều

<!-- thinking:start -->

> **Tư duy**
>
> Ma trận không thay đổi và có nhiều truy vấn tính tổng hình chữ nhật con. Cộng từng ô cho mỗi truy vấn sẽ không hiệu quả khi số truy vấn lớn.
>
> Mảng tổng tiền tố hai chiều $s[i+1][j+1]$ lưu tổng các phần tử trong hình chữ nhật từ gốc tọa độ đến $(i,j)$. Dùng nguyên lý bù trừ, ta tính tổng của vùng bất kỳ bằng bốn lần tra cứu: cộng tổng tiền tố ở góc dưới bên phải, trừ dải phía trên và bên trái, rồi cộng lại phần góc bị trừ hai lần. Sau khi tiền xử lý trong $O(mn)$, mỗi truy vấn chỉ mất $O(1)$.

<!-- thinking:end -->

Ta dùng $s[i + 1][j + 1]$ để biểu diễn tổng các phần tử trong vùng góc trên bên trái đến hàng thứ $i$ và cột thứ $j$, với chỉ số $i$ và $j$ đều bắt đầu từ $0$. Công thức tổng tiền tố là:

$$
s[i + 1][j + 1] = s[i + 1][j] + s[i][j + 1] - s[i][j] + nums[i][j]
$$

Khi đó, tổng các phần tử trong hình chữ nhật có $(x_1, y_1)$ và $(x_2, y_2)$ lần lượt là góc trên bên trái và góc dưới bên phải được tính như sau:

$$
s[x_2 + 1][y_2 + 1] - s[x_2 + 1][y_1] - s[x_1][y_2 + 1] + s[x_1][y_1]
$$

Trong hàm khởi tạo, ta tiền xử lý mảng tổng tiền tố $s$; trong hàm truy vấn, chỉ cần trả về kết quả theo công thức trên.

Độ phức tạp thời gian khởi tạo là $O(m \times n)$, độ phức tạp thời gian cho mỗi truy vấn là $O(1)$, còn độ phức tạp không gian là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class NumMatrix:
    def __init__(self, matrix: List[List[int]]):
        m, n = len(matrix), len(matrix[0])
        self.s = [[0] * (n + 1) for _ in range(m + 1)]
        for i, row in enumerate(matrix):
            for j, v in enumerate(row):
                self.s[i + 1][j + 1] = (
                    self.s[i][j + 1] + self.s[i + 1][j] - self.s[i][j] + v
                )

    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        return (
            self.s[row2 + 1][col2 + 1]
            - self.s[row2 + 1][col1]
            - self.s[row1][col2 + 1]
            + self.s[row1][col1]
        )


# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
```

#### Java

```java
class NumMatrix {
    private int[][] s;

    public NumMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        s = new int[m + 1][n + 1];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                s[i + 1][j + 1] = s[i + 1][j] + s[i][j + 1] - s[i][j] + matrix[i][j];
            }
        }
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return s[row2 + 1][col2 + 1] - s[row2 + 1][col1] - s[row1][col2 + 1] + s[row1][col1];
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```

#### C++

```cpp
class NumMatrix {
public:
    vector<vector<int>> s;

    NumMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        s.resize(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                s[i + 1][j + 1] = s[i + 1][j] + s[i][j + 1] - s[i][j] + matrix[i][j];
            }
        }
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        return s[row2 + 1][col2 + 1] - s[row2 + 1][col1] - s[row1][col2 + 1] + s[row1][col1];
    }
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * int param_1 = obj->sumRegion(row1,col1,row2,col2);
 */
```

#### Go

```go
type NumMatrix struct {
	s [][]int
}

func Constructor(matrix [][]int) NumMatrix {
	m, n := len(matrix), len(matrix[0])
	s := make([][]int, m+1)
	for i := range s {
		s[i] = make([]int, n+1)
	}
	for i, row := range matrix {
		for j, v := range row {
			s[i+1][j+1] = s[i+1][j] + s[i][j+1] - s[i][j] + v
		}
	}
	return NumMatrix{s}
}

func (this *NumMatrix) SumRegion(row1 int, col1 int, row2 int, col2 int) int {
	return this.s[row2+1][col2+1] - this.s[row2+1][col1] - this.s[row1][col2+1] + this.s[row1][col1]
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * obj := Constructor(matrix);
 * param_1 := obj.SumRegion(row1,col1,row2,col2);
 */
```

#### TypeScript

```ts
class NumMatrix {
    private s: number[][];

    constructor(matrix: number[][]) {
        const m = matrix.length;
        const n = matrix[0].length;
        this.s = new Array(m + 1).fill(0).map(() => new Array(n + 1).fill(0));
        for (let i = 0; i < m; ++i) {
            for (let j = 0; j < n; ++j) {
                this.s[i + 1][j + 1] =
                    this.s[i + 1][j] + this.s[i][j + 1] - this.s[i][j] + matrix[i][j];
            }
        }
    }

    sumRegion(row1: number, col1: number, row2: number, col2: number): number {
        return (
            this.s[row2 + 1][col2 + 1] -
            this.s[row2 + 1][col1] -
            this.s[row1][col2 + 1] +
            this.s[row1][col1]
        );
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * var obj = new NumMatrix(matrix)
 * var param_1 = obj.sumRegion(row1,col1,row2,col2)
 */
```

#### Rust

```rust
/**
 * Your NumMatrix object will be instantiated and called as such:
 * let obj = NumMatrix::new(matrix);
 * let ret_1: i32 = obj.sum_region(row1, col1, row2, col2);
 */

struct NumMatrix {
    // Of size (N + 1) * (M + 1)
    prefix_vec: Vec<Vec<i32>>,
    n: usize,
    m: usize,
    is_initialized: bool,
    ref_vec: Vec<Vec<i32>>,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl NumMatrix {
    fn new(matrix: Vec<Vec<i32>>) -> Self {
        NumMatrix {
            prefix_vec: vec![vec![0; matrix[0].len() + 1]; matrix.len() + 1],
            n: matrix.len(),
            m: matrix[0].len(),
            is_initialized: false,
            ref_vec: matrix,
        }
    }

    fn sum_region(&mut self, row1: i32, col1: i32, row2: i32, col2: i32) -> i32 {
        if !self.is_initialized {
            self.initialize_prefix_vec();
        }
        // Since i32 will let `rustc` complain, just make it happy
        let row1: usize = row1 as usize;
        let col1: usize = col1 as usize;
        let row2: usize = row2 as usize;
        let col2: usize = col2 as usize;
        // Return the value in O(1)
        self.prefix_vec[row2 + 1][col2 + 1]
            - self.prefix_vec[row2 + 1][col1]
            - self.prefix_vec[row1][col2 + 1]
            + self.prefix_vec[row1][col1]
    }

    fn initialize_prefix_vec(&mut self) {
        // Initialize the prefix sum vector
        for i in 0..self.n {
            for j in 0..self.m {
                self.prefix_vec[i + 1][j + 1] =
                    self.prefix_vec[i][j + 1] + self.prefix_vec[i + 1][j] - self.prefix_vec[i][j]
                        + self.ref_vec[i][j];
            }
        }
        self.is_initialized = true;
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matrix
 */
var NumMatrix = function (matrix) {
    const m = matrix.length;
    const n = matrix[0].length;
    this.s = new Array(m + 1).fill(0).map(() => new Array(n + 1).fill(0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            this.s[i + 1][j + 1] =
                this.s[i + 1][j] + this.s[i][j + 1] - this.s[i][j] + matrix[i][j];
        }
    }
};

/**
 * @param {number} row1
 * @param {number} col1
 * @param {number} row2
 * @param {number} col2
 * @return {number}
 */
NumMatrix.prototype.sumRegion = function (row1, col1, row2, col2) {
    return (
        this.s[row2 + 1][col2 + 1] -
        this.s[row2 + 1][col1] -
        this.s[row1][col2 + 1] +
        this.s[row1][col1]
    );
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * var obj = new NumMatrix(matrix)
 * var param_1 = obj.sumRegion(row1,col1,row2,col2)
 */
```

#### Kotlin

```kotlin
class NumMatrix(matrix: Array<IntArray>) {
    private val n: Int
    private val m: Int
    private val matrix: Array<IntArray>
    private val prefix_sums_matrix: Array<IntArray>
    private var initialized: Boolean

    init {
        this.n = matrix.size
        this.m = matrix[0].size
        this.matrix = matrix
        this.prefix_sums_matrix = Array(n + 1) { IntArray(m + 1) }
        this.initialized = false
    }

    fun sumRegion(row1: Int, col1: Int, row2: Int, col2: Int): Int {
        this.init()
        return this.prefix_sums_matrix[row2 + 1][col2 + 1] -
            this.prefix_sums_matrix[row2 + 1][col1] -
            this.prefix_sums_matrix[row1][col2 + 1] +
            this.prefix_sums_matrix[row1][col1]
    }

    private fun init(): Boolean {
        if (!this.initialized) {
            for (i in 0..<this.n) {
                for (j in 0..<this.m) {
                    this.prefix_sums_matrix[i + 1][j + 1] =
                        this.prefix_sums_matrix[i + 1][j] +
                        this.prefix_sums_matrix[i][j + 1] -
                        this.prefix_sums_matrix[i][j] +
                        this.matrix[i][j]
                }
            }
            this.initialized = true
            return true
        }
        return false
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such: var obj = NumMatrix(matrix) var
 * param_1 = obj.sumRegion(row1,col1,row2,col2)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
