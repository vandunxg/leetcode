---
comments: true
difficulty: Easy
rating: 1149
source: Weekly Contest 493 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3870. Count Commas in Range](https://leetcode.com/problems/count-commas-in-range)

[中文文档](/solution/3800-3899/3870.Count%20Commas%20in%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Hãy trả về <strong>tổng</strong> số dấu phẩy được sử dụng khi viết tất cả các số nguyên từ <code>[1, n]</code> (bao gồm cả hai đầu mút) theo định dạng số <strong>tiêu chuẩn</strong>.</p>

<p>Trong định dạng <strong>tiêu chuẩn</strong>:</p>

<ul>
	<li>Một dấu phẩy được chèn sau <strong>mỗi ba</strong> chữ số tính từ bên phải.</li>
	<li>Các số có <strong>ít hơn</strong> 4 chữ số không chứa dấu phẩy.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1002</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số <code>&quot;1,000&quot;</code>, <code>&quot;1,001&quot;</code> và <code>&quot;1,002&quot;</code> đều chứa một dấu phẩy, nên tổng số dấu phẩy là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 998</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các số từ 1 đến 998 đều có ít hơn bốn chữ số. Do đó, không có dấu phẩy nào được sử dụng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố mẹo

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số dấu phẩy phân cách hàng nghìn khi viết $[1,n]$. Vì $n \le 10^5$, các số có nhiều nhất sáu chữ số và mỗi số có nhiều nhất một dấu phẩy.
>
> Các số từ $1$ đến $999$ không có dấu phẩy; mỗi số nguyên từ $1000$ đến $n$ có đúng một dấu phẩy.
>
> Đáp án là $\max(0,n-999)$.
>
> Độ phức tạp thời gian là hằng số, không cần duyệt các số.

<!-- thinking:end -->

Các số từ 1 đến 999 không chứa dấu phẩy, nên khi $n$ nhỏ hơn hoặc bằng 999, đáp án là 0.

Vì miền giá trị của $n$ là $[1, 10^5]$, khi $n$ lớn hơn hoặc bằng 1000, mỗi số đều chứa đúng một dấu phẩy, nên đáp án là $n - 999$.

Do đó, đáp án là $\max(0, n - 999)$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCommas(self, n: int) -> int:
        return max(0, n - 999)
```

#### Java

```java
class Solution {
    public int countCommas(int n) {
        return Math.max(0, n - 999);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countCommas(int n) {
        return max(0, n - 999);
    }
};
```

#### Go

```go
func countCommas(n int) int {
	return max(0, n-999)
}
```

#### TypeScript

```ts
function countCommas(n: number): number {
    return Math.max(0, n - 999);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
