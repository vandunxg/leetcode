---
comments: true
difficulty: Medium
rating: 1356
source: Biweekly Contest 99 Q2
tags:
    - Math
---

<!-- problem:start -->

# [2579. Count Total Number of Colored Cells](https://leetcode.com/problems/count-total-number-of-colored-cells)

[中文文档](/solution/2500-2599/2579.Count%20Total%20Number%20of%20Colored%20Cells/README.md)

## Mô tả

<!-- description:start -->

<p>Có một lưới hai chiều gồm vô hạn các ô đơn vị chưa được tô màu. Cho một số nguyên dương <code>n</code>, bạn cần thực hiện quy trình sau trong <code>n</code> phút:</p>

<ul>
	<li>Ở phút đầu tiên, tô màu xanh cho <strong>bất kỳ</strong> ô đơn vị nào.</li>
	<li>Trong mỗi phút tiếp theo, tô màu xanh cho <strong>mọi</strong> ô chưa được tô màu tiếp giáp với một ô màu xanh.</li>
</ul>

<p>Dưới đây là hình minh họa trạng thái của lưới sau phút 1, 2 và 3.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2579.Count%20Total%20Number%20of%20Colored%20Cells/images/example-copy-2.png" style="width: 500px; height: 279px;" />
<p>Trả về <em>số lượng <strong>ô đã được tô màu</strong> ở cuối</em> <code>n</code> <em>phút</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Sau 1 phút, chỉ có 1 ô màu xanh, nên ta trả về 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Sau 2 phút, có 4 ô đã tô màu ở biên và 1 ô ở trung tâm, nên ta trả về 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Ở phút $1$, ta tô một ô; mỗi phút sau đó tô các ô chưa được tô màu lân cận với hình hiện tại. Mô phỏng $n\le 10^5$ phút sẽ phải tạo ra số lượng ô tăng theo bậc hai.
>
> Sau $n$ phút, hình thu được là một hình thoi: có $n$ ô trên mỗi trục, với diện tích $1+4(1+2+\cdots+(n-1))=2n(n-1)+1$.

<!-- thinking:end -->

Ta nhận thấy sau phút thứ $n$, lưới có tổng cộng $2 \times n - 1$ cột, và số ô trên mỗi cột lần lượt là $1, 3, 5, \cdots, 2 \times n - 1, 2 \times n - 3, \cdots, 3, 1$. Phần bên trái và bên phải đều là các cấp số cộng, nên có thể tính tổng bằng $2 \times n \times (n - 1) + 1$.

Độ phức tạp thời gian là $O(1)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def coloredCells(self, n: int) -> int:
        return 2 * n * (n - 1) + 1
```

#### Java

```java
class Solution {
    public long coloredCells(int n) {
        return 2L * n * (n - 1) + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long coloredCells(int n) {
        return 2LL * n * (n - 1) + 1;
    }
};
```

#### Go

```go
func coloredCells(n int) int64 {
	return int64(2*n*(n-1) + 1)
}
```

#### TypeScript

```ts
function coloredCells(n: number): number {
    return 2 * n * (n - 1) + 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn colored_cells(n: i32) -> i64 {
        2 * (n as i64) * ((n as i64) - 1) + 1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
