---
comments: true
difficulty: Hard
rating: 1722
source: Biweekly Contest 20 Q4
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [1359. Count All Valid Pickup and Delivery Options](https://leetcode.com/problems/count-all-valid-pickup-and-delivery-options)

[中文文档](/solution/1300-1399/1359.Count%20All%20Valid%20Pickup%20and%20Delivery%20Options/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>n</code> đơn hàng, mỗi đơn gồm một lần lấy hàng và một lần giao hàng.</p>

<p>Hãy đếm số trình tự lấy/giao hàng hợp lệ, trong đó lần giao hàng của đơn <code>i</code> luôn diễn ra sau lần lấy hàng của đơn <code>i</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả modulo 10^9 + 7.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có một trình tự (P1, D1); lần giao hàng 1 luôn diễn ra sau lần lấy hàng 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Tất cả trình tự có thể là: 
(P1,P2,D1,D2), (P1,P2,D2,D1), (P1,D1,P2,D2), (P2,P1,D1,D2), (P2,P1,D2,D1) and (P2,D2,P1,D1).
Đây là trình tự không hợp lệ (P1,D2,P2,D1), vì lần lấy hàng 2 diễn ra sau lần giao hàng 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 90
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đơn trong $n$ đơn phải được lấy trước khi giao. Vì $n \le 500$, không thể liệt kê $(2n)!$ trình tự. Xét đơn được giao cuối là một đơn nào đó: lần lấy hàng của đơn này có thể nằm ở bất kỳ vị trí nào trong $2i-1$ vị trí trước đó; nhân với số cách sắp xếp $i-1$ đơn còn lại và $i$ lựa chọn cho đơn cuối. Chỉ cần cập nhật tuần tự $f \leftarrow f \cdot i \cdot (2i-1)$ theo modulo số nguyên tố.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số trình tự lấy/giao hàng hợp lệ của $i$ đơn. Ban đầu, $f[1] = 1$.

Ta có thể chọn bất kỳ đơn nào trong $i$ đơn làm đơn được giao cuối cùng $D_i$. Lần lấy hàng $P_i$ của đơn đó có thể nằm ở bất kỳ vị trí nào trong $2 \times i - 1$ vị trí trước đó; số trình tự lấy/giao hàng của $i - 1$ đơn còn lại là $f[i - 1]$. Do đó, ta có công thức:

$$
f[i] = i \times (2 \times i - 1) \times f[i - 1]
$$

Đáp án cuối cùng là $f[n]$.

Ta nhận thấy $f[i]$ chỉ phụ thuộc vào $f[i - 1]$, nên có thể dùng một biến thay cho mảng để giảm độ phức tạp không gian.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số đơn hàng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOrders(self, n: int) -> int:
        mod = 10**9 + 7
        f = 1
        for i in range(2, n + 1):
            f = (f * i * (2 * i - 1)) % mod
        return f
```

#### Java

```java
class Solution {
    public int countOrders(int n) {
        final int mod = (int) 1e9 + 7;
        long f = 1;
        for (int i = 2; i <= n; ++i) {
            f = f * i * (2 * i - 1) % mod;
        }
        return (int) f;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countOrders(int n) {
        const int mod = 1e9 + 7;
        long long f = 1;
        for (int i = 2; i <= n; ++i) {
            f = f * i * (2 * i - 1) % mod;
        }
        return f;
    }
};
```

#### Go

```go
func countOrders(n int) int {
	const mod = 1e9 + 7
	f := 1
	for i := 2; i <= n; i++ {
		f = f * i * (2*i - 1) % mod
	}
	return f
}
```

#### Rust

```rust
const MOD: i64 = (1e9 as i64) + 7;

impl Solution {
    #[allow(dead_code)]
    pub fn count_orders(n: i32) -> i32 {
        let mut f = 1;
        for i in 2..=n as i64 {
            f = (i * (2 * i - 1) * f) % MOD;
        }
        f as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
