---
comments: true
difficulty: Easy
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [598. Range Addition II](https://leetcode.com/problems/range-addition-ii)

[中文文档](/solution/0500-0599/0598.Range%20Addition%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>m x n</code> <code>M</code> được khởi tạo toàn số <code>0</code> và mảng thao tác <code>ops</code>, trong đó <code>ops[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> nghĩa là tăng <code>M[x][y]</code> lên một với mọi <code>0 &lt;= x &lt; a<sub>i</sub></code> và <code>0 &lt;= y &lt; b<sub>i</sub></code>.</p>

<p>Hãy đếm và trả về <em>số lượng phần tử có giá trị lớn nhất trong ma trận sau khi thực hiện tất cả thao tác</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0598.Range%20Addition%20II/images/ex1.jpg" style="width: 750px; height: 176px;" />
<pre>
<strong>Đầu vào:</strong> m = 3, n = 3, ops = [[2,2],[3,3]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Giá trị lớn nhất trong M là 2 và có bốn phần tử mang giá trị này. Vì vậy, trả về 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 3, n = 3, ops = [[2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3]]
<strong>Đầu ra:</strong> 4
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 3, n = 3, ops = []
<strong>Đầu ra:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= ops.length &lt;= 10<sup>4</sup></code></li>
	<li><code>ops[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub> &lt;= m</code></li>
	<li><code>1 &lt;= b<sub>i</sub> &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bài toán mẹo

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác tăng các phần tử trong một ma trận con ở góc trên bên trái thêm một đơn vị. Giá trị lớn nhất cuối cùng bằng số thao tác và xuất hiện tại phần giao của các ma trận con đó. Mọi thao tác đều bắt đầu tại $(0,0)$, nên phần giao có kích thước $\min a_i$ nhân với $\min b_i$.
>
> Chỉ cần duyệt các thao tác một lượt; tích hai giá trị nhỏ nhất là đáp án. Không cần dùng mảng hiệu kích thước $m \times n$.

<!-- thinking:end -->

Ta nhận thấy phần giao của tất cả ma trận con thao tác chính là vùng chứa giá trị lớn nhất cuối cùng; mỗi ma trận con đều bắt đầu từ góc trên bên trái $(0, 0)$. Vì vậy, ta duyệt các thao tác để tìm số hàng và số cột nhỏ nhất, rồi trả về tích của hai giá trị này.

Lưu ý, nếu mảng thao tác rỗng thì số phần tử có giá trị lớn nhất trong ma trận là $m \times n$.

Độ phức tạp thời gian là $O(k)$, trong đó $k$ là độ dài của mảng thao tác $\textit{ops}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCount(self, m: int, n: int, ops: List[List[int]]) -> int:
        for a, b in ops:
            m = min(m, a)
            n = min(n, b)
        return m * n
```

#### Java

```java
class Solution {
    public int maxCount(int m, int n, int[][] ops) {
        for (int[] op : ops) {
            m = Math.min(m, op[0]);
            n = Math.min(n, op[1]);
        }
        return m * n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxCount(int m, int n, vector<vector<int>>& ops) {
        for (const auto& op : ops) {
            m = min(m, op[0]);
            n = min(n, op[1]);
        }
        return m * n;
    }
};
```

#### Go

```go
func maxCount(m int, n int, ops [][]int) int {
	for _, op := range ops {
		m = min(m, op[0])
		n = min(n, op[1])
	}
	return m * n
}
```

#### TypeScript

```ts
function maxCount(m: number, n: number, ops: number[][]): number {
    for (const [a, b] of ops) {
        m = Math.min(m, a);
        n = Math.min(n, b);
    }
    return m * n;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_count(mut m: i32, mut n: i32, ops: Vec<Vec<i32>>) -> i32 {
        for op in ops {
            m = m.min(op[0]);
            n = n.min(op[1]);
        }
        m * n
    }
}
```

#### JavaScript

```js
/**
 * @param {number} m
 * @param {number} n
 * @param {number[][]} ops
 * @return {number}
 */
var maxCount = function (m, n, ops) {
    for (const [a, b] of ops) {
        m = Math.min(m, a);
        n = Math.min(n, b);
    }
    return m * n;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
