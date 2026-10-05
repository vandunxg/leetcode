---
comments: true
difficulty: Easy
rating: 1202
source: Weekly Contest 497 Q1
tags:
    - Graph
    - Array
    - Matrix
---

<!-- problem:start -->

# [3898. Find the Degree of Each Vertex](https://leetcode.com/problems/find-the-degree-of-each-vertex)

[中文文档](/solution/3800-3899/3898.Find%20the%20Degree%20of%20Each%20Vertex/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>matrix</code> có kích thước <code>n x n</code>, biểu diễn ma trận kề của một đồ thị vô hướng có <code>n</code> đỉnh được đánh nhãn từ 0 đến <code>n - 1</code>.</p>

<ul>
	<li><code>matrix[i][j] = 1</code> cho biết có một cạnh nối đỉnh <code>i</code> và đỉnh <code>j</code>.</li>
	<li><code>matrix[i][j] = 0</code> cho biết không có cạnh nối đỉnh <code>i</code> và đỉnh <code>j</code>.</li>
</ul>

<p><strong>Bậc</strong> của một đỉnh là số cạnh nối với đỉnh đó.</p>

<p>Hãy trả về một mảng số nguyên <code>ans</code> có kích thước <code>n</code>, trong đó <code>ans[i]</code> biểu thị bậc của đỉnh <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3898.Find%20the%20Degree%20of%20Each%20Vertex/images/g41f.png" style="width: 180px; height: 142px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [[0,1,1],[1,0,1],[1,1,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đỉnh 0 nối với các đỉnh 1 và 2, nên bậc của nó là 2.</li>
	<li>Đỉnh 1 nối với các đỉnh 0 và 2, nên bậc của nó là 2.</li>
	<li>Đỉnh 2 nối với các đỉnh 0 và 1, nên bậc của nó là 2.</li>
</ul>

<p>Vậy đáp án là <code>[2, 2, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3898.Find%20the%20Degree%20of%20Each%20Vertex/images/g42f.png" style="width: 180px; height: 145px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [[0,1,0],[1,0,0],[0,0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đỉnh 0 nối với đỉnh 1, nên bậc của nó là 1.</li>
	<li>Đỉnh 1 nối với đỉnh 0, nên bậc của nó là 1.</li>
	<li>Đỉnh 2 không nối với đỉnh nào, nên bậc của nó là 0.</li>
</ul>

<p>Vậy đáp án là <code>[1, 1, 0]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">matrix = [[0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0]</span></p>

<p><strong>Giải thích:</strong></p>

<p data-end="1129" data-start="1068">Chỉ có một đỉnh và không có cạnh nào nối với nó. Vậy đáp án là <code data-end="1156" data-start="1151">[0]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == matrix.length == matrix[i].length &lt;= 100</code>​​​​​​​</li>
	<li><code>​​​​​​​matrix[i][i] == 0</code></li>
	<li><code>matrix[i][j]</code> chỉ có thể là 0 hoặc 1</li>
	<li><code>matrix[i][j] == matrix[j][i]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Đồ thị được biểu diễn bằng ma trận kề; bậc là số lượng $1$ trong một hàng. Vì $n \le 100$, ta chỉ cần tính tổng từng hàng.
>
> Không có cạnh nối một đỉnh với chính nó, nên đường chéo có giá trị $0$ và phép tính tổng trực tiếp là chính xác.
>
> Tính đối xứng đã thể hiện các cạnh vô hướng; không cần đọc riêng phần tam giác dưới.
>
> $O(n^2)$ là đủ để đọc toàn bộ ma trận.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quá trình tính bậc của từng đỉnh.

Với mỗi đỉnh $i$, ta duyệt qua hàng tương ứng $\text{matrix}[i]$ và đếm số phần tử bằng 1. Đây chính là bậc của đỉnh $i$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là số đỉnh của đồ thị. Không tính phần không gian dành cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDegrees(self, matrix: list[list[int]]) -> list[int]:
        ans = [0] * len(matrix)
        for i, row in enumerate(matrix):
            for x in row:
                ans[i] += x
        return ans
```

#### Java

```java
class Solution {
    public int[] findDegrees(int[][] matrix) {
        int n = matrix.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            for (int x : matrix[i]) {
                ans[i] += x;
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
    vector<int> findDegrees(vector<vector<int>>& matrix) {
        int n = matrix.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            for (int x : matrix[i]) {
                ans[i] += x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findDegrees(matrix [][]int) []int {
	ans := make([]int, len(matrix))
	for i, row := range matrix {
		for _, x := range row {
			ans[i] += x
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findDegrees(matrix: number[][]): number[] {
    const n = matrix.length;
    const ans: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        for (const x of matrix[i]) {
            ans[i] += x;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
