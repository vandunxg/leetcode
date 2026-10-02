---
comments: true
difficulty: Hard
rating: 2345
source: Weekly Contest 209 Q4
tags:
    - Bit Manipulation
    - Recursion
    - Memoization
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [1611. Minimum One Bit Operations to Make Integers Zero](https://leetcode.com/problems/minimum-one-bit-operations-to-make-integers-zero)

[中文文档](/solution/1600-1699/1611.Minimum%20One%20Bit%20Operations%20to%20Make%20Integers%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy biến đổi nó thành <code>0</code> bằng cách thực hiện các thao tác sau một số lần tùy ý:</p>

<ul>
<li>Thay đổi bit ngoài cùng bên phải (bit <code>0<sup>th</sup></code>) trong biểu diễn nhị phân của <code>n</code>.</li>
<li>Thay đổi bit <code>i<sup>th</sup></code> trong biểu diễn nhị phân của <code>n</code> nếu bit <code>(i-1)<sup>th</sup></code> bằng <code>1</code> và các bit từ <code>(i-2)<sup>th</sup></code> đến <code>0<sup>th</sup></code> đều bằng <code>0</code>.</li>
</ul>

<p>Trả về <em>số thao tác ít nhất để biến đổi </em><code>n</code><em> thành </em><code>0</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 3
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Biểu diễn nhị phân của 3 là &quot;11&quot;.
&quot;<u>1</u>1&quot; -&gt; &quot;<u>0</u>1&quot; with the 2<sup>nd</sup> operation since the 0<sup>th</sup> bit is 1.
&quot;0<u>1</u>&quot; -&gt; &quot;0<u>0</u>&quot; with the 1<sup>st</sup> operation.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 6
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Biểu diễn nhị phân của 6 là &quot;110&quot;.
&quot;<u>1</u>10&quot; -&gt; &quot;<u>0</u>10&quot; with the 2<sup>nd</sup> operation since the 1<sup>st</sup> bit is 1 and 0<sup>th</sup> through 0<sup>th</sup> bits are 0.
&quot;01<u>0</u>&quot; -&gt; &quot;01<u>1</u>&quot; with the 1<sup>st</sup> operation.
&quot;0<u>1</u>1&quot; -&gt; &quot;0<u>0</u>1&quot; with the 2<sup>nd</sup> operation since the 0<sup>th</sup> bit is 1.
&quot;00<u>1</u>&quot; -&gt; &quot;00<u>0</u>&quot; with the 1<sup>st</sup> operation.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phép biến đổi ngược Gray Code (Gray Code sang Binary Code)

<!-- thinking:start -->

> **Tư duy**
>
> Hai thao tác lần lượt đảo bit thấp nhất hoặc bit ngay phía trên bit $1$ thấp nhất. $n$ có thể bằng $10^9$, nên không thể mô phỏng từng lần đảo.
>
> Các quy tắc này chính là cách tạo mã Gray nhị phân: xem $n$ là một mã Gray, số thao tác ít nhất là số nhị phân tự nhiên tương ứng.
>
> Chuyển Gray sang nhị phân là phép XOR tiền tố từ cao xuống thấp, được cài đặt bằng $\textit{ans} \oplus= n$ và dịch phải cho đến khi $n$ bằng $0$.

<!-- thinking:end -->

Về bản chất, bài toán yêu cầu phép biến đổi ngược của mã Gray tại vị trí $n$, tức là dựng lại số ban đầu từ mã Gray.

Trước hết, hãy xem cách chuyển mã nhị phân sang mã Gray nhị phân. Bit có trọng số cao nhất của mã nhị phân được giữ làm bit cao nhất của mã Gray; bit cao thứ hai của mã Gray nhận được bằng cách XOR bit cao nhất với bit cao thứ hai của mã nhị phân. Các bit còn lại cũng được tính tương tự.

Giả sử số nhị phân được biểu diễn là $B_{n-1}B_{n-2}...B_2B_1B_0$, còn mã Gray tương ứng là $G_{n-1}G_{n-2}...G_2G_1G_0$. Bit cao nhất được giữ nguyên, nên $G_{n-1} = B_{n-1}$; với các bit khác, $G_i = B_{i+1} \oplus B_{i}$, trong đó $i=0,1,2..,n-2$.

Vậy phép biến đổi ngược từ mã Gray sang mã nhị phân là gì?

Ta nhận thấy bit cao nhất của mã Gray được giữ nguyên, nên $B_{n-1} = G_{n-1}$; và $B_{n-2} = G_{n-2} \oplus B_{n-1} = G_{n-2} \oplus G_{n-1}$; với các bit khác, $B_i = G_{i} \oplus G_{i+1} \cdots \oplus G_{n-1}$, trong đó $i=0,1,2..,n-2$. Vì vậy, ta có thể dùng hàm $rev(x)$ sau để thu được mã nhị phân:

```java
int rev(int x) {
    int n = 0;
    for (; x != 0; x >>= 1) {
        n ^= x;
    }
    return n;
}
```

Độ phức tạp thời gian là $O(\log n)$, với $n$ là số nguyên được cho trong bài. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOneBitOperations(self, n: int) -> int:
        ans = 0
        while n:
            ans ^= n
            n >>= 1
        return ans
```

#### Java

```java
class Solution {
    public int minimumOneBitOperations(int n) {
        int ans = 0;
        for (; n > 0; n >>= 1) {
            ans ^= n;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOneBitOperations(int n) {
        int ans = 0;
        for (; n > 0; n >>= 1) {
            ans ^= n;
        }
        return ans;
    }
};
```

#### Go

```go
func minimumOneBitOperations(n int) (ans int) {
	for ; n > 0; n >>= 1 {
		ans ^= n
	}
	return
}
```

#### TypeScript

```ts
function minimumOneBitOperations(n: number): number {
    let ans = 0;
    for (; n > 0; n >>= 1) {
        ans ^= n;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_one_bit_operations(mut n: i32) -> i32 {
        let mut ans = 0;
        while n > 0 {
            ans ^= n;
            n >>= 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
