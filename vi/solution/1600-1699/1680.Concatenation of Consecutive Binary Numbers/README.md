---
comments: true
difficulty: Medium
rating: 1629
source: Weekly Contest 218 Q3
tags:
    - Bit Manipulation
    - Math
    - Simulation
---

<!-- problem:start -->

# [1680. Concatenation of Consecutive Binary Numbers](https://leetcode.com/problems/concatenation-of-consecutive-binary-numbers)

[中文文档](/solution/1600-1699/1680.Concatenation%20of%20Consecutive%20Binary%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về <em><strong>giá trị thập phân</strong> của chuỗi nhị phân tạo thành bằng cách nối các biểu diễn nhị phân của các số từ </em><code>1</code><em> đến </em><code>n</code><em> theo thứ tự, rồi lấy <strong>modulo </strong></em><code>10<sup>9 </sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 1
<strong>Output:</strong> 1
<strong>Giải thích: </strong>&quot;1&quot; ở dạng nhị phân tương ứng với giá trị thập phân 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 3
<strong>Output:</strong> 27
<strong>Giải thích: </strong>Ở dạng nhị phân, 1, 2 và 3 tương ứng với &quot;1&quot;, &quot;10&quot; và &quot;11&quot;.
Sau khi nối chúng, ta được &quot;11011&quot;, tương ứng với giá trị thập phân 27.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 12
<strong>Output:</strong> 505379714
<strong>Giải thích</strong>: Kết quả nối là &quot;1101110010111011110001001101010111100&quot;.
Giá trị thập phân của chuỗi này là 118505380540.
Sau khi lấy modulo 10<sup>9</sup> + 7, kết quả là 505379714.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Nối các dạng nhị phân của $1..n$ modulo $10^9+7$. Vì $n$ có thể bằng $10^5$, ta không thể tạo cả chuỗi. Thêm $i$ tương đương với dịch trái số bit bằng độ dài biểu diễn nhị phân của $i$, sau đó OR với $i$.
>
> Vòng lặp có dạng $\textit{ans} = (\textit{ans} \ll i.\texttt{bit\_length()} \mid i) \bmod (10^9+7)$.

<!-- thinking:end -->

Quan sát cách nối các số, ta thấy khi nối số thứ $i$, kết quả $ans$ tạo bởi $i-1$ số trước đó được dịch trái một số bit nhất định rồi thêm $i$ vào. Số bit cần dịch chính là số chữ số nhị phân của $i$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên đã cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def concatenatedBinary(self, n: int) -> int:
        mod = 10**9 + 7
        ans = 0
        for i in range(1, n + 1):
            ans = (ans << i.bit_length() | i) % mod
        return ans
```

#### Java

```java
class Solution {
    public int concatenatedBinary(int n) {
        final int mod = (int) 1e9 + 7;
        long ans = 0;
        for (int i = 1; i <= n; ++i) {
            ans = (ans << (32 - Integer.numberOfLeadingZeros(i)) | i) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int concatenatedBinary(int n) {
        const int mod = 1e9 + 7;
        long long ans = 0;
        for (int i = 1; i <= n; ++i) {
            ans = (ans << (32 - __builtin_clz(i)) | i) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func concatenatedBinary(n int) (ans int) {
	const mod = 1e9 + 7
	for i := 1; i <= n; i++ {
		ans = (ans<<bits.Len(uint(i)) | i) % mod
	}
	return
}
```

#### TypeScript

```ts
function concatenatedBinary(n: number): number {
    const mod = 1_000_000_007;
    let ans = 0;
    for (let i = 1; i <= n; i++) {
        ans = (((ans * (1 << (32 - Math.clz32(i)))) % mod) + i) % mod;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn concatenated_binary(n: i32) -> i32 {
        let mod_: i64 = 1_000_000_007;
        let mut ans: i64 = 0;
        for i in 1..=n as i64 {
            let bit_length: u32 = 64 - i.leading_zeros() as u32;
            ans = ((ans << bit_length) | i) % mod_;
        }
        ans as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var concatenatedBinary = function (n) {
    const mod = 1_000_000_007;
    let ans = 0;
    for (let i = 1; i <= n; i++) {
        ans = (((ans * (1 << (32 - Math.clz32(i)))) % mod) + i) % mod;
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int ConcatenatedBinary(int n) {
        const int mod = 1000000007;
        long ans = 0;
        for (int i = 1; i <= n; ++i) {
            int bitLength = 32 - System.Numerics.BitOperations.LeadingZeroCount((uint)i);
            ans = ((ans << bitLength) | i) % mod;
        }
        return (int)ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit (Tối ưu)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 gọi $\texttt{bit\_length}$ ở mỗi bước. Độ rộng chỉ tăng khi $i$ là lũy thừa của hai, nên ta dùng bộ đếm $\textit{shift}$ và tăng nó tại các giá trị $i$ đó để tránh truy vấn độ dài.

<!-- thinking:end -->

Ở Lời giải 1, ta phải tính số chữ số nhị phân của $i$ ở mỗi lần lặp, làm phát sinh thêm phép tính. Ta có thể dùng biến $\textit{shift}$ để lưu số bit cần dịch hiện tại. Khi $i$ là lũy thừa của $2$, tăng $\textit{shift}$ thêm $1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên đã cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def concatenatedBinary(self, n: int) -> int:
        mod = 10**9 + 7
        ans = shift = 0
        for i in range(1, n + 1):
            if (i & (i - 1)) == 0:
                shift += 1
            ans = (ans << shift | i) % mod
        return ans
```

#### Java

```java
class Solution {
    public int concatenatedBinary(int n) {
        final int mod = (int) 1e9 + 7;
        long ans = 0;
        int shift = 0;
        for (int i = 1; i <= n; ++i) {
            if ((i & (i - 1)) == 0) {
                ++shift;
            }
            ans = (ans << shift | i) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int concatenatedBinary(int n) {
        const int mod = 1e9 + 7;
        long ans = 0;
        int shift = 0;
        for (int i = 1; i <= n; ++i) {
            if ((i & (i - 1)) == 0) {
                ++shift;
            }
            ans = (ans << shift | i) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func concatenatedBinary(n int) (ans int) {
	const mod = 1e9 + 7
	shift := 0
	for i := 1; i <= n; i++ {
		if i&(i-1) == 0 {
			shift++
		}
		ans = (ans<<shift | i) % mod
	}
	return
}
```

#### TypeScript

```ts
function concatenatedBinary(n: number): number {
    const mod = 1_000_000_007;
    let ans = 0;
    let shift = 0;
    for (let i = 1; i <= n; i++) {
        if ((i & (i - 1)) === 0) {
            shift++;
        }
        ans = (((ans * (1 << shift)) % mod) + i) % mod;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn concatenated_binary(n: i32) -> i32 {
        let mod_: i64 = 1_000_000_007;
        let mut ans: i64 = 0;
        let mut shift: u32 = 0;
        for i in 1..=n as i64 {
            if (i & (i - 1)) == 0 {
                shift += 1;
            }
            ans = ((ans << shift) | i) % mod_;
        }
        ans as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var concatenatedBinary = function (n) {
    const mod = 1_000_000_007;
    let ans = 0;
    let shift = 0;
    for (let i = 1; i <= n; i++) {
        if ((i & (i - 1)) === 0) {
            shift++;
        }
        ans = (((ans * (1 << shift)) % mod) + i) % mod;
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int ConcatenatedBinary(int n) {
        const int mod = 1000000007;
        long ans = 0;
        int shift = 0;
        for (int i = 1; i <= n; ++i) {
            if ((i & (i - 1)) == 0) {
                ++shift;
            }
            ans = ((ans << shift) | i) % mod;
        }
        return (int)ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
