---
comments: true
difficulty: Medium
rating: 1874
source: Weekly Contest 129 Q2
tags:
    - Hash Table
    - Math
    - Pigeonhole Principle
---

<!-- problem:start -->

# [1015. Smallest Integer Divisible by K](https://leetcode.com/problems/smallest-integer-divisible-by-k)

[中文文档](/solution/1000-1099/1015.Smallest%20Integer%20Divisible%20by%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên dương <code>k</code>. Hãy tìm <strong>độ dài</strong> của số nguyên dương <code>n</code> <strong>nhỏ nhất</strong> sao cho <code>n</code> chia hết cho <code>k</code> và chỉ gồm chữ số <code>1</code>.</p>

<p>Trả về <em><strong>độ dài</strong> của </em><code>n</code>. Nếu không tồn tại số <code>n</code> như vậy, trả về -1.</p>

<p><strong>Lưu ý:</strong> <code>n</code> có thể không vừa trong số nguyên có dấu 64-bit.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Đáp án nhỏ nhất là n = 1, có độ dài bằng 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không tồn tại số nguyên dương n như vậy chia hết cho 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Đáp án nhỏ nhất là n = 111, có độ dài bằng 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Nếu lần lượt tạo $1,11,111,\ldots$ cho đến khi chia hết cho $k$, số này có thể vượt giới hạn kiểu số nguyên; ngoài ra $k\le 10^5$ khiến không thể tìm kiếm không giới hạn. Tính chia hết chỉ phụ thuộc vào số dư modulo $k$.
>
> $(10n+1)\bmod k$ được xác định bởi $n\bmod k$. Chỉ có $k$ số dư khả dĩ; nếu số dư $0$ không xuất hiện trong $k$ bước thì các giá trị về sau sẽ lặp chu kỳ, nghĩa là không tồn tại số cần tìm.
>
> Bắt đầu với $1\bmod k$ và lặp tối đa $k$ lần. Khi số dư đầu tiên bằng 0, số vòng lặp chính là số chữ số cần tìm; nếu không thì đáp án là $-1$.

<!-- thinking:end -->

Ta nhận thấy số nguyên dương $n$ bắt đầu từ giá trị $1$; mỗi lần tạo chữ số tiếp theo, ta nhân $n$ với $10$ rồi cộng $1$, tức $n = n \times 10 + 1$. Vì $(n \times 10 + 1) \bmod k = ((n \bmod k) \times 10 + 1) \bmod k$, ta có thể kiểm tra $n$ có chia hết cho $k$ hay không bằng cách tính $n \bmod k$.

Bắt đầu với $n = 1$ và mỗi lần tính $n \bmod k$, cho đến khi $n \bmod k = 0$. Khi đó, ta đã tìm được số nguyên dương nhỏ nhất cần tìm và độ dài của nó là số chữ số đã tạo. Nếu chưa chia hết, cập nhật $n = (n \times 10 + 1) \bmod k$. Nếu sau $k$ vòng lặp vẫn chưa có $n \bmod k = 0$, thì không tồn tại số phù hợp và trả về $-1$.

Độ phức tạp thời gian là $O(k)$ và độ phức tạp không gian là $O(1)$, trong đó $k$ là số nguyên dương đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestRepunitDivByK(self, k: int) -> int:
        n = 1 % k
        for i in range(1, k + 1):
            if n == 0:
                return i
            n = (n * 10 + 1) % k
        return -1
```

#### Java

```java
class Solution {
    public int smallestRepunitDivByK(int k) {
        int n = 1 % k;
        for (int i = 1; i <= k; ++i) {
            if (n == 0) {
                return i;
            }
            n = (n * 10 + 1) % k;
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestRepunitDivByK(int k) {
        int n = 1 % k;
        for (int i = 1; i <= k; ++i) {
            if (n == 0) {
                return i;
            }
            n = (n * 10 + 1) % k;
        }
        return -1;
    }
};
```

#### Go

```go
func smallestRepunitDivByK(k int) int {
	n := 1 % k
	for i := 1; i <= k; i++ {
		if n == 0 {
			return i
		}
		n = (n*10 + 1) % k
	}
	return -1
}
```

#### TypeScript

```ts
function smallestRepunitDivByK(k: number): number {
    let n = 1 % k;
    for (let i = 1; i <= k; ++i) {
        if (n === 0) {
            return i;
        }
        n = (n * 10 + 1) % k;
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_repunit_div_by_k(k: i32) -> i32 {
        let mut n = 1 % k;
        for i in 1..=k {
            if n == 0 {
                return i;
            }
            n = (n * 10 + 1) % k;
        }
        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
