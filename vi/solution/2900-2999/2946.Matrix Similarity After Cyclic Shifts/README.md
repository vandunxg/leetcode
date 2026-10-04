---
comments: true
difficulty: Easy
rating: 1405
source: Weekly Contest 373 Q1
tags:
    - Array
    - Math
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [2946. Matrix Similarity After Cyclic Shifts](https://leetcode.com/problems/matrix-similarity-after-cyclic-shifts)

[中文文档](/solution/2900-2999/2946.Matrix%20Similarity%20After%20Cyclic%20Shifts/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>m x n</code> <code>mat</code> và một số nguyên <code>k</code>. Các hàng của ma trận được đánh chỉ số từ 0.</p>

<p>Quy trình sau được thực hiện <code>k</code> lần:</p>

<ul>
	<li>Các hàng có chỉ số <strong>chẵn</strong> (0, 2, 4, ...) được dịch vòng sang trái.</li>
</ul>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2946.Matrix%20Similarity%20After%20Cyclic%20Shifts/images/lshift.jpg" style="width: 283px; height: 90px;" /></p>

<ul>
	<li>Các hàng có chỉ số <strong>lẻ</strong> (1, 3, 5, ...) được dịch vòng sang phải.</li>
</ul>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2946.Matrix%20Similarity%20After%20Cyclic%20Shifts/images/rshift-stlone.jpg" style="width: 283px; height: 90px;" /></p>

<p>Trả về <code>true</code> nếu ma trận sau khi biến đổi qua <code>k</code> bước giống hệt ma trận ban đầu, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mat = [[1,2,3],[4,5,6],[7,8,9]], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ở mỗi bước, các hàng 0 và 2 (chỉ số chẵn) được dịch sang trái, còn hàng 1 (chỉ số lẻ) được dịch sang phải.</p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2946.Matrix%20Similarity%20After%20Cyclic%20Shifts/images/t1-2.jpg" style="width: 857px; height: 150px;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mat = [[1,2,1,2],[5,5,5,5],[6,3,6,3]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2946.Matrix%20Similarity%20After%20Cyclic%20Shifts/images/t1-3.jpg" style="width: 632px; height: 150px;" /></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">mat = [[2,2],[2,2]], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì mọi giá trị trong ma trận đều bằng nhau, ma trận vẫn không thay đổi ngay cả sau khi thực hiện các phép dịch vòng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= mat.length &lt;= 25</code></li>
	<li><code>1 &lt;= mat[i].length &lt;= 25</code></li>
	<li><code>1 &lt;= mat[i][j] &lt;= 25</code></li>
	<li><code>1 &lt;= k &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các hàng lẻ được dịch sang phải $k$ vị trí và các hàng chẵn được dịch sang trái $k$ vị trí; ma trận phải giữ nguyên. Có thể rút gọn $k$ theo modulo số cột, nhưng việc so sánh ô sẽ chuyển đến $(i,j)$ giúp tránh phải xoay ma trận.
>
> Với hàng lẻ, kiểm tra $mat[i][(j+k)\bmod n]$; với hàng chẵn, kiểm tra $mat[i][(j-k+n)\bmod n]$. Chỉ cần một điểm không khớp là kết quả không thỏa mãn. Ma trận có kích thước tối đa $25 \times 25$.

<!-- thinking:end -->

Ta duyệt qua từng phần tử của ma trận và kiểm tra xem vị trí của phần tử sau phép dịch vòng có giữ nguyên như vị trí ban đầu hay không.

Với các hàng có chỉ số lẻ, ta dịch các phần tử sang phải $k$ vị trí, nên phần tử $(i, j)$ sẽ chuyển đến vị trí $(i, (j + k) \bmod n)$ sau phép dịch vòng, trong đó $n$ là số cột.

Với các hàng có chỉ số chẵn, ta dịch các phần tử sang trái $k$ vị trí, nên phần tử $(i, j)$ sẽ chuyển đến vị trí $(i, (j - k + n) \bmod n)$ sau phép dịch vòng.

Nếu tại bất kỳ thời điểm nào trong quá trình duyệt, ta phát hiện vị trí của một phần tử sau phép dịch vòng khác với vị trí ban đầu, ta trả về $\text{false}$. Nếu mọi phần tử vẫn giữ nguyên sau khi duyệt hết, ta trả về $\text{true}$.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def areSimilar(self, mat: List[List[int]], k: int) -> bool:
        n = len(mat[0])
        for i, row in enumerate(mat):
            for j, x in enumerate(row):
                if i % 2 == 1 and x != mat[i][(j + k) % n]:
                    return False
                if i % 2 == 0 and x != mat[i][(j - k + n) % n]:
                    return False
        return True
```

#### Java

```java
class Solution {
    public boolean areSimilar(int[][] mat, int k) {
        int m = mat.length, n = mat[0].length;
        k %= n;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i % 2 == 1 && mat[i][j] != mat[i][(j + k) % n]) {
                    return false;
                }
                if (i % 2 == 0 && mat[i][j] != mat[i][(j - k + n) % n]) {
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
    bool areSimilar(vector<vector<int>>& mat, int k) {
        int m = mat.size(), n = mat[0].size();
        k %= n;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i % 2 == 1 && mat[i][j] != mat[i][(j + k) % n]) {
                    return false;
                }
                if (i % 2 == 0 && mat[i][j] != mat[i][(j - k + n) % n]) {
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
func areSimilar(mat [][]int, k int) bool {
	n := len(mat[0])
	k %= n
	for i, row := range mat {
		for j, x := range row {
			if i%2 == 1 && x != mat[i][(j+k)%n] {
				return false
			}
			if i%2 == 0 && x != mat[i][(j-k+n)%n] {
				return false
			}
		}
	}
	return true
}
```

#### TypeScript

```ts
function areSimilar(mat: number[][], k: number): boolean {
    const m = mat.length;
    const n = mat[0].length;
    k %= n;
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (i % 2 === 1 && mat[i][j] !== mat[i][(j + k) % n]) {
                return false;
            }
            if (i % 2 === 0 && mat[i][j] !== mat[i][(j - k + n) % n]) {
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
    pub fn are_similar(mat: Vec<Vec<i32>>, k: i32) -> bool {
        let m = mat.len();
        let n = mat[0].len();
        let k = (k as usize) % n;

        for i in 0..m {
            for j in 0..n {
                if i % 2 == 1 && mat[i][j] != mat[i][(j + k) % n] {
                    return false;
                }
                if i % 2 == 0 && mat[i][j] != mat[i][(j + n - k) % n] {
                    return false;
                }
            }
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
