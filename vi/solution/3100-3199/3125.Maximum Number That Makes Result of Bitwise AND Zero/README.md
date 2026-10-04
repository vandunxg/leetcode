---
comments: true
difficulty: Medium
tags:
    - Greedy
    - String
    - Sorting
---

<!-- problem:start -->

# [3125. Maximum Number That Makes Result of Bitwise AND Zero 🔒](https://leetcode.com/problems/maximum-number-that-makes-result-of-bitwise-and-zero)

[中文文档](/solution/3100-3199/3125.Maximum%20Number%20That%20Makes%20Result%20of%20Bitwise%20AND%20Zero/README.md)

## Mô tả

<!-- description:start -->

Cho một số nguyên <code>n</code>, hãy trả về số nguyên <strong>lớn nhất</strong> <code>x</code> sao cho <code>x &lt;= n</code> và phép <code>AND</code> bit của tất cả các số trong đoạn <code>[x, n]</code> bằng 0.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phép <code>AND</code> bit của <code>[6, 7]</code> là 6.<br />
Phép <code>AND</code> bit của <code>[5, 6, 7]</code> là 4.<br />
Phép <code>AND</code> bit của <code>[4, 5, 6, 7]</code> là 4.<br />
Phép <code>AND</code> bit của <code>[3, 4, 5, 6, 7]</code> là 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 9</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phép <code>AND</code> bit của <code>[7, 8, 9]</code> là 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 17</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phép <code>AND</code> bit của <code>[15, 16, 17]</code> là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm giá trị lớn nhất thỏa mãn $x\le n$ sao cho $x$ AND $x+1$ bằng $0$. Nếu thử lần lượt $n-1,n-2,\ldots$ thì trong trường hợp xấu nhất sẽ mất thời gian tuyến tính.
>
> $x$ và $x+1$ là hai số liên tiếp, nên AND của chúng chỉ bằng 0 khi $x$ là một chuỗi các bit 1 ở cuối. Giá trị lớn nhất như vậy nhỏ hơn $n$ là mask nằm dưới bit cao nhất của $n$.
>
> Trả về $2^{\textit{bit\_length}(n)-1}-1$. Giá trị này AND với số kế tiếp của nó bằng 0 và là giá trị lớn nhất không vượt quá $n$.

<!-- thinking:end -->

Ta có thể tìm bit $1$ cao nhất trong biểu diễn nhị phân của $n$. $x$ lớn nhất phải nhỏ hơn $n$ và có bit này bằng $0$, trong khi tất cả các bit thấp hơn đều bằng $1$, tức là $x = 2^{\textit{number of the highest bit}} - 1$. Điều này là do phải thỏa mãn $x \textit{ and } (x + 1) = 0$.

Độ phức tạp thời gian là $O(\log n)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxNumber(self, n: int) -> int:
        return (1 << (n.bit_length() - 1)) - 1
```

#### Java

```java
class Solution {
    public long maxNumber(long n) {
        return (1L << (63 - Long.numberOfLeadingZeros(n))) - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxNumber(long long n) {
        return (1LL << (63 - __builtin_clzll(n))) - 1;
    }
};
```

#### Go

```go
func maxNumber(n int64) int64 {
	return int64(1<<(bits.Len64(uint64(n))-1)) - 1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
