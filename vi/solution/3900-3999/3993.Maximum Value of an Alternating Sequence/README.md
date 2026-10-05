---
comments: true
difficulty: Medium
rating: 1452
source: Biweekly Contest 187 Q2
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [3993. Maximum Value of an Alternating Sequence](https://leetcode.com/problems/maximum-value-of-an-alternating-sequence)

[中文文档](/solution/3900-3999/3993.Maximum%20Value%20of%20an%20Alternating%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code>n</code>, <code>s</code> và <code>m</code>.</p>

<p>Một dãy số nguyên <code>seq</code> có độ dài <code>n</code> được xem là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li><code>seq[0] = s</code>.</li>
	<li>Dãy là <strong>luân phiên</strong>, nghĩa là một trong hai điều kiện sau đúng:
	<ul>
		<li><code>seq[0] &gt; seq[1] &lt; seq[2] &gt; ...</code>, hoặc</li>
		<li><code>seq[0] &lt; seq[1] &gt; seq[2] &lt; ...</code>.</li>
	</ul>
	</li>
	<li>Với mọi cặp phần tử kề nhau, <code>|seq[i] - seq[i - 1]| &lt;= m</code>.</li>
</ul>

<p>Dãy có độ dài 1 được xem là luân phiên.</p>

<p>Trả về <strong>phần tử lớn nhất</strong> có thể xuất hiện trong bất kỳ dãy hợp lệ nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, s = 3, m = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Một dãy hợp lệ là <code>[3, 8, 7, 12]</code>.</li>
	<li>Phần tử lớn nhất trong dãy là 12.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, s = 4, m = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Một dãy hợp lệ là <code>[4, 7]</code>.</li>
	<li>Phần tử lớn nhất trong dãy là 7.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, s &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= m &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Hai giá trị kề nhau chênh lệch không quá $m$ và phải luân phiên. Để tăng một phần tử nào đó, mỗi lần tăng nên dùng đủ $m$ còn mỗi lần giảm chỉ dùng $1$, nhờ đó chừa lại nhiều khoảng trống nhất cho lần tăng tiếp theo.
>
> Nếu $n=1$ thì đáp án là $s$. Nếu không, mẫu tăng-rồi-giảm có $\lfloor n/2\rfloor$ lần tăng và đỉnh là $s+\lfloor n/2\rfloor(m-1)+1$. Mẫu giảm-rồi-tăng giảm trước nên không thể tốt hơn.
>
> $n$ và $s$ có thể đạt $10^9$, vì vậy công thức đóng phải chạy trong $O(1)$.

<!-- thinking:end -->

Nếu $n = 1$, dãy chỉ chứa giá trị bắt đầu $s$, nên đáp án là $s$.

Nếu không, độ dài dãy ít nhất là $2$. Vì độ chênh lệch tuyệt đối giữa hai phần tử kề nhau không quá $m$, đồng thời dãy phải luân phiên tăng giảm, để tối đa hóa một phần tử ta nên liên tục thực hiện thao tác "tăng $m$, rồi giảm $1$": bước giảm dùng giá trị nhỏ nhất là $1$ để lần tăng tiếp theo có nhiều khoảng trống nhất.

Xây dựng dãy theo mẫu "tăng trước":

$$
s,\ s+m,\ s+m-1,\ s+2m-1,\ s+2m-2,\ \ldots
$$

Với độ dài $n$, ta có thể thực hiện $\lfloor n / 2 \rfloor$ lần tăng, và đỉnh sau lần tăng thứ $k$ là $s + k(m - 1) + 1$. Do đó, phần tử lớn nhất là:

$$
s + \left\lfloor \frac{n}{2} \right\rfloor (m - 1) + 1
$$

Bắt đầu bằng một bước giảm chỉ làm các giá trị nhỏ đi trước, nên không thể tạo ra đỉnh lớn hơn. Vì vậy, cách xây dựng trên là tối ưu.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumValue(self, n: int, s: int, m: int) -> int:
        if n == 1:
            return s
        return s + n // 2 * (m - 1) + 1
```

#### Java

```java
class Solution {
    public long maximumValue(int n, int s, int m) {
        if (n == 1) {
            return s;
        }
        return (long) s + (long) (n / 2) * (m - 1) + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumValue(int n, int s, int m) {
        if (n == 1) {
            return s;
        }
        return 1LL * s + 1LL * (n / 2) * (m - 1) + 1;
    }
};
```

#### Go

```go
func maximumValue(n int, s int, m int) int64 {
	if n == 1 {
		return int64(s)
	}
	return int64(s) + int64(n/2)*int64(m-1) + 1
}
```

#### TypeScript

```ts
function maximumValue(n: number, s: number, m: number): number {
    if (n === 1) {
        return s;
    }
    return s + Math.floor(n / 2) * (m - 1) + 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
