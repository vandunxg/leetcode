---
comments: true
difficulty: Medium
rating: 2132
source: Weekly Contest 351 Q2
tags:
    - Bit Manipulation
    - Brainteaser
    - Enumeration
---

<!-- problem:start -->

# [2749. Minimum Operations to Make the Integer Zero](https://leetcode.com/problems/minimum-operations-to-make-the-integer-zero)

[中文文档](/solution/2700-2799/2749.Minimum%20Operations%20to%20Make%20the%20Integer%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>num1</code> và <code>num2</code>.</p>

<p>Trong một thao tác, bạn có thể chọn số nguyên <code>i</code> trong khoảng <code>[0, 60]</code> và trừ <code>2<sup>i</sup> + num2</code> khỏi <code>num1</code>.</p>

<p>Trả về <em>số nguyên biểu thị số thao tác <strong>ít nhất</strong> cần thực hiện để đưa</em> <code>num1</code> <em>về</em> <code>0</code>.</p>

<p>Nếu không thể đưa <code>num1</code> về <code>0</code>, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = 3, num2 = -2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể đưa 3 về 0 bằng các thao tác sau:
- Chọn i = 2 và trừ 2<sup>2</sup> + (-2) khỏi 3, 3 - (4 + (-2)) = 1.
- Chọn i = 2 và trừ 2<sup>2</sup>&nbsp;+ (-2) khỏi 1, 1 - (4 + (-2)) = -1.
- Chọn i = 0 và trừ 2<sup>0</sup>&nbsp;+ (-2) khỏi -1, (-1) - (1 + (-2)) = 0.
Có thể chứng minh rằng 3 là số thao tác ít nhất cần thực hiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = 5, num2 = 7
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không thể đưa 5 về 0 với thao tác đã cho.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num1 &lt;= 10<sup>9</sup></code></li>
	<li><code><font face="monospace">-10<sup>9</sup>&nbsp;&lt;= num2 &lt;= 10<sup>9</sup></font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước trừ $2^i+num2$ khỏi $num1$; ta cần số bước ít nhất để đưa giá trị về $0$. Miền số mũ khá lớn, nên không thể tìm kiếm chuỗi các giá trị $i$.
>
> Sau đúng $k$ thao tác, $x=num1-k\cdot num2$ phải là tổng của $k$ lũy thừa của hai, tức là $x\ge k$ và số bit 1 của $x$ không vượt quá $k$. Ta tăng dần $k$ từ $1$ và dừng khi $x$ trở thành số âm.

<!-- thinking:end -->

Nếu thực hiện $k$ lần, bài toán về cơ bản trở thành: xác định xem $\textit{num1} - k \times \textit{num2}$ có thể được tách thành tổng của $k$ số $2^i$ hay không.

Đặt $x = \textit{num1} - k \times \textit{num2}$. Ta xét các trường hợp sau:

- Nếu $x < 0$, thì $x$ không thể được tách thành tổng của $k$ số $2^i$, vì $2^i > 0$, nên rõ ràng không có lời giải;
- Nếu số lượng bit $1$ trong biểu diễn nhị phân của $x$ lớn hơn $k$, trường hợp này cũng không có lời giải;
- Ngược lại, với $k$ hiện tại, chắc chắn tồn tại một cách tách.

Vì vậy, ta bắt đầu liệt kê $k$ từ $1$. Khi tìm được $k$ thỏa mãn điều kiện, ta có thể trả về đáp án ngay.

Độ phức tạp thời gian là $O(\log x)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeTheIntegerZero(self, num1: int, num2: int) -> int:
        for k in count(1):
            x = num1 - k * num2
            if x < 0:
                break
            if x.bit_count() <= k <= x:
                return k
        return -1
```

#### Java

```java
class Solution {
    public int makeTheIntegerZero(int num1, int num2) {
        for (long k = 1;; ++k) {
            long x = num1 - k * num2;
            if (x < 0) {
                break;
            }
            if (Long.bitCount(x) <= k && k <= x) {
                return (int) k;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int makeTheIntegerZero(int num1, int num2) {
        using ll = long long;
        for (ll k = 1;; ++k) {
            ll x = num1 - k * num2;
            if (x < 0) {
                break;
            }
            if (__builtin_popcountll(x) <= k && k <= x) {
                return k;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func makeTheIntegerZero(num1 int, num2 int) int {
	for k := 1; ; k++ {
		x := num1 - k*num2
		if x < 0 {
			break
		}
		if bits.OnesCount(uint(x)) <= k && k <= x {
			return k
		}
	}
	return -1
}
```

#### TypeScript

```ts
function makeTheIntegerZero(num1: number, num2: number): number {
    for (let k = 1; ; ++k) {
        let x = num1 - k * num2;
        if (x < 0) {
            break;
        }
        if (x.toString(2).replace(/0/g, '').length <= k && k <= x) {
            return k;
        }
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn make_the_integer_zero(num1: i32, num2: i32) -> i32 {
        let num1 = num1 as i64;
        let num2 = num2 as i64;
        for k in 1.. {
            let x = num1 - k * num2;
            if x < 0 {
                break;
            }
            if (x.count_ones() as i64) <= k && k <= x {
                return k as i32;
            }
        }
        -1
    }
}
```

#### C#

```cs
public class Solution {
    public int MakeTheIntegerZero(int num1, int num2) {
        long a = num1, b = num2;
        for (long k = 1; ; ++k) {
            long x = a - k * b;
            if (x < 0) {
                break;
            }
            if (BitOperations.PopCount((ulong)x) <= k && k <= x) {
                return (int)k;
            }
        }
        return -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
