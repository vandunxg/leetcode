---
comments: true
difficulty: Easy
rating: 1220
source: Weekly Contest 464 Q1
tags:
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3658. GCD of Odd and Even Sums](https://leetcode.com/problems/gcd-of-odd-and-even-sums)

[中文文档](/solution/3600-3699/3658.GCD%20of%20Odd%20and%20Even%20Sums/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>. Nhiệm vụ của bạn là tính <strong>GCD</strong> (ước chung lớn nhất) của hai giá trị:</p>

<ul>
	<li>
	<p><code>sumOdd</code>: tổng của <code>n</code> số lẻ dương nhỏ nhất.</p>
	</li>
	<li>
	<p><code>sumEven</code>: tổng của <code>n</code> số chẵn dương nhỏ nhất.</p>
	</li>
</ul>

<p>Trả về GCD của <code>sumOdd</code> và <code>sumEven</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tổng của 4 số lẻ đầu tiên là <code>sumOdd = 1 + 3 + 5 + 7 = 16</code></li>
	<li>Tổng của 4 số chẵn đầu tiên là <code>sumEven = 2 + 4 + 6 + 8 = 20</code></li>
</ul>

<p>Do đó, <code>GCD(sumOdd, sumEven) = GCD(16, 20) = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tổng của 5 số lẻ đầu tiên là <code>sumOdd = 1 + 3 + 5 + 7 + 9 = 25</code></li>
	<li>Tổng của 5 số chẵn đầu tiên là <code>sumEven = 2 + 4 + 6 + 8 + 10 = 30</code></li>
</ul>

<p>Do đó, <code>GCD(sumOdd, sumEven) = GCD(25, 30) = 5</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10​​​​​​​00</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Tổng của $n$ số lẻ đầu tiên là $n^2$ và tổng của $n$ số chẵn đầu tiên là $n(n+1)$. Mọi ước chung đều là bội của $n$.
>
> Vì $n$ và $n+1$ nguyên tố cùng nhau, $\gcd(n^2,n(n+1))=n$. Không cần dùng một vòng lặp gcd tổng quát.
>
> Trả về $n$.

<!-- thinking:end -->

Tổng của $n$ số lẻ đầu tiên là $n^2$, còn tổng của $n$ số chẵn đầu tiên là $n(n + 1)$. Ước chung lớn nhất của hai số này ít nhất là $n$. Vì $n$ và $n + 1$ nguyên tố cùng nhau, đáp án là $n$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def gcdOfOddEvenSums(self, n: int) -> int:
        return n
```

#### Java

```java
class Solution {
    public int gcdOfOddEvenSums(int n) {
        return n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int gcdOfOddEvenSums(int n) {
        return n;
    }
};
```

#### Go

```go
func gcdOfOddEvenSums(n int) int {
	return n
}
```

#### TypeScript

```ts
function gcdOfOddEvenSums(n: number): number {
    return n;
}
```

#### Rust

```rust
impl Solution {
    pub fn gcd_of_odd_even_sums(n: i32) -> i32 {
        n
    }
}
```

#### C#

```cs
public class Solution {
    public int GcdOfOddEvenSums(int n) {
        return n;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
