---
comments: true
difficulty: Easy
rating: 1407
source: Weekly Contest 244 Q1
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [1886. Determine Whether Matrix Can Be Obtained By Rotation](https://leetcode.com/problems/determine-whether-matrix-can-be-obtained-by-rotation)

[中文文档](/solution/1800-1899/1886.Determine%20Whether%20Matrix%20Can%20Be%20Obtained%20By%20Rotation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai ma trận nhị phân <code>n x n</code> là <code>mat</code> và <code>target</code>, trả về <code>true</code><em> nếu có thể biến </em><code>mat</code><em> thành </em><code>target</code><em> bằng cách <strong>xoay</strong> </em><code>mat</code><em> theo các góc tăng dần từng <strong>90 độ</strong>, hoặc </em><code>false</code><em> nếu không.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1886.Determine%20Whether%20Matrix%20Can%20Be%20Obtained%20By%20Rotation/images/grid3.png" style="width: 301px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[0,1],[1,0]], target = [[1,0],[0,1]]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Ta có thể xoay mat 90 độ theo chiều kim đồng hồ để mat bằng target.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1886.Determine%20Whether%20Matrix%20Can%20Be%20Obtained%20By%20Rotation/images/grid4.png" style="width: 301px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[0,1],[1,1]], target = [[1,0],[0,1]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể biến mat thành target bằng cách xoay mat.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1886.Determine%20Whether%20Matrix%20Can%20Be%20Obtained%20By%20Rotation/images/grid4.png" style="width: 661px; height: 184px;" />
<pre>
<strong>Đầu vào:</strong> mat = [[0,0,0],[0,1,0],[1,1,1]], target = [[1,1,1],[0,1,0],[0,0,0]]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Ta có thể xoay mat 90 độ theo chiều kim đồng hồ hai lần để mat bằng target.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == mat.length == target.length</code></li>
	<li><code>n == mat[i].length == target[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>mat[i][j]</code> và <code>target[i][j]</code> đều là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So sánh tại chỗ

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần quyết định liệu $mat$ sau khi xoay $0/90/180/270$ độ có bằng $target$ hay không. Tạo bốn bản sao phù hợp với $n\le 10$, nhưng ta có thể so sánh tại chỗ.
>
> Bốn phép biến đổi ánh xạ $(i,j)$ lần lượt thành $(i,j)$, $(j,n-1-i)$, $(n-1-i,n-1-j)$ và $(n-1-j,i)$. Một mask bốn bit theo dõi các phép xoay còn khớp; khi có sai khác, ta xóa bit tương ứng và dừng nếu mask trở thành $0$.

<!-- thinking:end -->

Ta quan sát quy luật xoay ma trận và nhận thấy với một phần tử $\text{mat}[i][j]$, sau khi xoay 90 độ, nó xuất hiện ở vị trí $\text{mat}[j][n-1-i]$; sau khi xoay 180 độ, nó xuất hiện ở vị trí $\text{mat}[n-1-i][n-1-j]$; và sau khi xoay 270 độ, nó xuất hiện ở vị trí $\text{mat}[n-1-j][i]$.

Do đó, ta có thể dùng một số nguyên $\textit{ok}$ để ghi nhận các trạng thái xoay hiện tại, khởi tạo bằng $0b1111$, biểu thị rằng cả bốn trạng thái xoay đều có thể xảy ra. Với mỗi phần tử trong ma trận, ta kiểm tra xem vị trí của nó ở các trạng thái xoay khác nhau có khớp với phần tử tương ứng trong ma trận đích hay không. Nếu không khớp, ta loại trạng thái xoay đó khỏi $\textit{ok}$. Cuối cùng, nếu $\textit{ok}$ khác 0, nghĩa là có ít nhất một trạng thái xoay khiến ma trận khớp với ma trận đích, ta trả về $\textit{true}$; ngược lại, trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là kích thước ma trận. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findRotation(self, mat: List[List[int]], target: List[List[int]]) -> bool:
        n = len(mat)
        ok = 0b1111
        for i in range(n):
            for j in range(n):
                if mat[i][j] != target[i][j]:
                    ok &= ~0b0001
                if mat[j][n - 1 - i] != target[i][j]:
                    ok &= ~0b0010
                if mat[n - 1 - i][n - 1 - j] != target[i][j]:
                    ok &= ~0b0100
                if mat[n - 1 - j][i] != target[i][j]:
                    ok &= ~0b1000
                if ok == 0:
                    return False
        return ok != 0
```

#### Java

```java
class Solution {
    public boolean findRotation(int[][] mat, int[][] target) {
        int n = mat.length;
        int ok = 0b1111;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] != target[i][j]) {
                    ok &= ~0b0001;
                }
                if (mat[j][n - 1 - i] != target[i][j]) {
                    ok &= ~0b0010;
                }
                if (mat[n - 1 - i][n - 1 - j] != target[i][j]) {
                    ok &= ~0b0100;
                }
                if (mat[n - 1 - j][i] != target[i][j]) {
                    ok &= ~0b1000;
                }
                if (ok == 0) {
                    return false;
                }
            }
        }
        return ok != 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool findRotation(vector<vector<int>>& mat, vector<vector<int>>& target) {
        int n = mat.size();
        int ok = 0b1111;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (mat[i][j] != target[i][j]) {
                    ok &= ~0b0001;
                }
                if (mat[j][n - 1 - i] != target[i][j]) {
                    ok &= ~0b0010;
                }
                if (mat[n - 1 - i][n - 1 - j] != target[i][j]) {
                    ok &= ~0b0100;
                }
                if (mat[n - 1 - j][i] != target[i][j]) {
                    ok &= ~0b1000;
                }
                if (ok == 0) {
                    return false;
                }
            }
        }
        return ok != 0;
    }
};
```

#### Go

```go
func findRotation(mat [][]int, target [][]int) bool {
	n := len(mat)
	ok := 0b1111

	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if mat[i][j] != target[i][j] {
				ok &= ^0b0001
			}
			if mat[j][n-1-i] != target[i][j] {
				ok &= ^0b0010
			}
			if mat[n-1-i][n-1-j] != target[i][j] {
				ok &= ^0b0100
			}
			if mat[n-1-j][i] != target[i][j] {
				ok &= ^0b1000
			}
			if ok == 0 {
				return false
			}
		}
	}

	return ok != 0
}
```

#### TypeScript

```ts
function findRotation(mat: number[][], target: number[][]): boolean {
    const n = mat.length;
    let ok = 0b1111;

    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            if (mat[i][j] !== target[i][j]) {
                ok &= ~0b0001;
            }
            if (mat[j][n - 1 - i] !== target[i][j]) {
                ok &= ~0b0010;
            }
            if (mat[n - 1 - i][n - 1 - j] !== target[i][j]) {
                ok &= ~0b0100;
            }
            if (mat[n - 1 - j][i] !== target[i][j]) {
                ok &= ~0b1000;
            }
            if (ok === 0) {
                return false;
            }
        }
    }

    return ok !== 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_rotation(mat: Vec<Vec<i32>>, target: Vec<Vec<i32>>) -> bool {
        let n = mat.len();
        let mut ok: i32 = 0b1111;

        for i in 0..n {
            for j in 0..n {
                if mat[i][j] != target[i][j] {
                    ok &= !0b0001;
                }
                if mat[j][n - 1 - i] != target[i][j] {
                    ok &= !0b0010;
                }
                if mat[n - 1 - i][n - 1 - j] != target[i][j] {
                    ok &= !0b0100;
                }
                if mat[n - 1 - j][i] != target[i][j] {
                    ok &= !0b1000;
                }
                if ok == 0 {
                    return false;
                }
            }
        }

        ok != 0
    }
}
```

#### C#

```cs
public class Solution {
    public bool FindRotation(int[][] mat, int[][] target) {
        int n = mat.Length;
        int ok = 0b1111;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] != target[i][j]) {
                    ok &= ~0b0001;
                }
                if (mat[j][n - 1 - i] != target[i][j]) {
                    ok &= ~0b0010;
                }
                if (mat[n - 1 - i][n - 1 - j] != target[i][j]) {
                    ok &= ~0b0100;
                }
                if (mat[n - 1 - j][i] != target[i][j]) {
                    ok &= ~0b1000;
                }
                if (ok == 0) {
                    return false;
                }
            }
        }
        return ok != 0;
    }
}
```

#### Kotlin

```kotlin
class Solution {
    fun findRotation(mat: Array<IntArray>, target: Array<IntArray>): Boolean {
        val n = mat.size
        var ok = 0b1111
        for (i in 0 until n) {
            for (j in 0 until n) {
                if (mat[i][j] != target[i][j]) {
                    ok = ok and 0b1110
                }
                if (mat[j][n - 1 - i] != target[i][j]) {
                    ok = ok and 0b1101
                }
                if (mat[n - 1 - i][n - 1 - j] != target[i][j]) {
                    ok = ok and 0b1011
                }
                if (mat[n - 1 - j][i] != target[i][j]) {
                    ok = ok and 0b0111
                }
                if (ok == 0) {
                    return false
                }
            }
        }
        return ok != 0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
