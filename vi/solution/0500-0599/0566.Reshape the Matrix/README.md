---
comments: true
difficulty: Easy
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [566. Reshape the Matrix](https://leetcode.com/problems/reshape-the-matrix)

[中文文档](/solution/0500-0599/0566.Reshape%20the%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Trong MATLAB có hàm tiện dụng <code>reshape</code>, cho phép đổi ma trận kích thước <code>m x n</code> thành ma trận mới kích thước <code>r x c</code> mà vẫn giữ nguyên dữ liệu.</p>

<p>Cho ma trận <code>mat</code> kích thước <code>m x n</code> và hai số nguyên <code>r</code>, <code>c</code> lần lượt là số hàng và số cột của ma trận mới.</p>

<p>Ma trận mới phải chứa toàn bộ phần tử của ma trận ban đầu theo đúng thứ tự duyệt từng hàng như ban đầu.</p>

<p>Nếu có thể thực hiện phép <code>reshape</code> với các tham số đã cho, hãy trả về ma trận mới; nếu không, trả về ma trận ban đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0566.Reshape%20the%20Matrix/images/reshape1-grid.jpg" style="width: 613px; height: 173px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[1,2],[3,4]], r = 1, c = 4
<strong>Đầu ra:</strong> [[1,2,3,4]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0566.Reshape%20the%20Matrix/images/reshape2-grid.jpg" style="width: 453px; height: 173px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[1,2],[3,4]], r = 2, c = 4
<strong>Đầu ra:</strong> [[1,2],[3,4]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>-1000 &lt;= mat[i][j] &lt;= 1000</code></li>
	<li><code>1 &lt;= r, c &lt;= 300</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Reshape sắp xếp lại dữ liệu theo thứ tự row-major vào ma trận $r \times c$. Nếu tích hai kích thước khác nhau, trả về ma trận ban đầu.
>
> Chỉ số tuyến tính $i$ đọc phần tử `mat[i // n][i % n]` và ghi vào `ans[i // c][i % c]`. Chỉ cần một lượt duyệt để sao chép mọi phần tử.

<!-- thinking:end -->

Trước tiên, ta lấy số hàng và số cột của ma trận ban đầu, lần lượt ký hiệu là $m$ và $n$. Nếu $m \times n \neq r \times c$, không thể đổi kích thước ma trận nên ta trả về ma trận ban đầu.

Nếu không, ta tạo ma trận mới có $r$ hàng và $c$ cột. Bắt đầu từ phần tử đầu tiên của ma trận ban đầu, ta duyệt tất cả phần tử theo thứ tự row-major rồi lần lượt đặt chúng vào ma trận mới.

Sau khi duyệt hết các phần tử của ma trận ban đầu, ta thu được đáp án.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận ban đầu. Không tính phần bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def matrixReshape(self, mat: List[List[int]], r: int, c: int) -> List[List[int]]:
        m, n = len(mat), len(mat[0])
        if m * n != r * c:
            return mat
        ans = [[0] * c for _ in range(r)]
        for i in range(m * n):
            ans[i // c][i % c] = mat[i // n][i % n]
        return ans
```

#### Java

```java
class Solution {
    public int[][] matrixReshape(int[][] mat, int r, int c) {
        int m = mat.length, n = mat[0].length;
        if (m * n != r * c) {
            return mat;
        }
        int[][] ans = new int[r][c];
        for (int i = 0; i < m * n; ++i) {
            ans[i / c][i % c] = mat[i / n][i % n];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& mat, int r, int c) {
        int m = mat.size(), n = mat[0].size();
        if (m * n != r * c) {
            return mat;
        }
        vector<vector<int>> ans(r, vector<int>(c));
        for (int i = 0; i < m * n; ++i) {
            ans[i / c][i % c] = mat[i / n][i % n];
        }
        return ans;
    }
};
```

#### Go

```go
func matrixReshape(mat [][]int, r int, c int) [][]int {
	m, n := len(mat), len(mat[0])
	if m*n != r*c {
		return mat
	}
	ans := make([][]int, r)
	for i := range ans {
		ans[i] = make([]int, c)
	}
	for i := 0; i < m*n; i++ {
		ans[i/c][i%c] = mat[i/n][i%n]
	}
	return ans
}
```

#### TypeScript

```ts
function matrixReshape(mat: number[][], r: number, c: number): number[][] {
    let m = mat.length,
        n = mat[0].length;
    if (m * n != r * c) return mat;
    let ans = Array.from({ length: r }, v => new Array(c).fill(0));
    let k = 0;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            ans[Math.floor(k / c)][k % c] = mat[i][j];
            ++k;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn matrix_reshape(mat: Vec<Vec<i32>>, r: i32, c: i32) -> Vec<Vec<i32>> {
        let r = r as usize;
        let c = c as usize;
        let m = mat.len();
        let n = mat[0].len();
        if m * n != r * c {
            return mat;
        }
        let mut i = 0;
        let mut j = 0;
        (0..r)
            .into_iter()
            .map(|_| {
                (0..c)
                    .into_iter()
                    .map(|_| {
                        let res = mat[i][j];
                        j += 1;
                        if j == n {
                            j = 0;
                            i += 1;
                        }
                        res
                    })
                    .collect()
            })
            .collect()
    }
}
```

#### C

```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *returnColumnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** matrixReshape(int** mat, int matSize, int* matColSize, int r, int c, int* returnSize, int** returnColumnSizes) {
    if (matSize * matColSize[0] != r * c) {
        *returnSize = matSize;
        *returnColumnSizes = matColSize;
        return mat;
    }
    *returnSize = r;
    *returnColumnSizes = malloc(sizeof(int) * r);
    int** ans = malloc(sizeof(int*) * r);
    for (int i = 0; i < r; i++) {
        (*returnColumnSizes)[i] = c;
        ans[i] = malloc(sizeof(int) * c);
    }
    for (int i = 0; i < r * c; i++) {
        ans[i / c][i % c] = mat[i / matColSize[0]][i % matColSize[0]];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã ánh xạ các phần tử thông qua chỉ số tuyến tính. Lời giải 2 dùng cùng công thức khi duyệt ma trận đích, nên chỉ thay đổi thứ tự vòng lặp.
>
> Cách ánh xạ vẫn giống nhau, nên không cải thiện độ phức tạp tiệm cận.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function matrixReshape(mat: number[][], r: number, c: number): number[][] {
    const m = mat.length;
    const n = mat[0].length;
    if (m * n !== r * c) {
        return mat;
    }
    const ans = Array.from({ length: r }, () => new Array(c).fill(0));
    for (let i = 0; i < r * c; i++) {
        ans[Math.floor(i / c)][i % c] = mat[Math.floor(i / n)][i % n];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
