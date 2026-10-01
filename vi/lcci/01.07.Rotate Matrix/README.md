---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [01.07. Rotate Matrix](https://leetcode.cn/problems/rotate-matrix-lcci)

[中文文档](/lcci/01.07.Rotate%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hình ảnh được biểu diễn bằng ma trận N x N, trong đó mỗi pixel trong hình ảnh chiếm 4 byte, hãy viết một phương thức để xoay hình ảnh 90 độ. Bạn có thể thực hiện thao tác này in-place không?</p>

<p>&nbsp;</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

Cho <strong>ma trận</strong> =

[

  [1,2,3],

  [4,5,6],

  [7,8,9]

],



Xoay ma trận <strong>in-place. </strong>Kết quả là:

[

  [7,4,1],

  [8,5,2],

  [9,6,3]

]

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

Cho <strong>ma trận</strong> =

[

  [ 5, 1, 9,11],

  [ 2, 4, 8,10],

  [13, 3, 6, 7],

  [15,14,12,16]

],



Xoay ma trận <strong>in-place. </strong>Kết quả là:

[

  [15,13, 2, 5],

  [14, 3, 4, 1],

  [12, 6, 8, 9],

  [16, 7,10,11]

]

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xoay in-place

<!-- thinking:start -->

> **Tư duy**
>
> Phép xoay theo chiều kim đồng hồ $90^\circ$ ánh xạ $(i,j)$ thành $(j, n-i-1)$. Việc ghi vào một ma trận mới là đúng, nhưng sử dụng thêm $O(n^2)$ không gian, trái với yêu cầu in-place.
>
> Phép ánh xạ này có thể phân tích thành hai phép đối xứng: lật theo chiều dọc, sau đó chuyển vị qua đường chéo chính. Sau cả hai lần hoán đổi, $(i,j)$ sẽ nằm ở vị trí đích.
>
> Đoạn code hoán đổi hàng $i$ với hàng $n-i-1$, sau đó hoán đổi nửa tam giác dưới với điều kiện $i>j$ để chuyển vị, chỉ sử dụng một vài biến tạm.

<!-- thinking:end -->

Theo yêu cầu của đề bài, chúng ta cần xoay $\text{matrix}[i][j]$ thành $\text{matrix}[j][n - i - 1]$.

Trước tiên, chúng ta có thể lật ma trận theo chiều dọc, tức là hoán đổi $\text{matrix}[i][j]$ với $\text{matrix}[n - i - 1][j]$, sau đó lật ma trận theo đường chéo chính, tức là hoán đổi $\text{matrix}[i][j]$ với $\text{matrix}[j][i]$. Khi đó, $\text{matrix}[i][j]$ sẽ được xoay thành $\text{matrix}[j][n - i - 1]$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        n = len(matrix)
        for i in range(n >> 1):
            for j in range(n):
                matrix[i][j], matrix[n - i - 1][j] = matrix[n - i - 1][j], matrix[i][j]
        for i in range(n):
            for j in range(i):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```

#### Java

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n >> 1; ++i) {
            for (int j = 0; j < n; ++j) {
                int t = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = t;
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int t = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = t;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for (int i = 0; i < n >> 1; ++i) {
            for (int j = 0; j < n; ++j) {
                swap(matrix[i][j], matrix[n - i - 1][j]);
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }
    }
};
```

#### Go

```go
func rotate(matrix [][]int) {
	n := len(matrix)
	for i := 0; i < n>>1; i++ {
		for j := 0; j < n; j++ {
			matrix[i][j], matrix[n-i-1][j] = matrix[n-i-1][j], matrix[i][j]
		}
	}
	for i := 0; i < n; i++ {
		for j := 0; j < i; j++ {
			matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
		}
	}
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify matrix in-place instead.
 */
function rotate(matrix: number[][]): void {
    matrix.reverse();
    for (let i = 0; i < matrix.length; ++i) {
        for (let j = 0; j < i; ++j) {
            const t = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = t;
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn rotate(matrix: &mut Vec<Vec<i32>>) {
        let n = matrix.len();
        for i in 0..n / 2 {
            for j in 0..n {
                let t = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = t;
            }
        }
        for i in 0..n {
            for j in 0..i {
                let t = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = t;
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
var rotate = function (matrix) {
    matrix.reverse();
    for (let i = 0; i < matrix.length; ++i) {
        for (let j = 0; j < i; ++j) {
            [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
        }
    }
};
```

#### C#

```cs
public class Solution {
    public void Rotate(int[][] matrix) {
        int n = matrix.Length;
        for (int i = 0; i < n >> 1; ++i) {
            for (int j = 0; j < n; ++j) {
                int t = matrix[i][j];
                matrix[i][j] = matrix[n - i - 1][j];
                matrix[n - i - 1][j] = t;
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int t = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = t;
            }
        }
    }
}
```

#### Swift

```swift
class Solution {
    func rotate(_ matrix: inout [[Int]]) {
        let n = matrix.count

        for i in 0..<(n >> 1) {
            for j in 0..<n {
                let t = matrix[i][j]
                matrix[i][j] = matrix[n - i - 1][j]
                matrix[n - i - 1][j] = t
            }
        }

        for i in 0..<n {
            for j in 0..<i {
                let t = matrix[i][j]
                matrix[i][j] = matrix[j][i]
                matrix[j][i] = t
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
