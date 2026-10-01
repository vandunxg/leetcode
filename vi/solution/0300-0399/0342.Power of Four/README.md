---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Recursion
    - Math
---

<!-- problem:start -->

# [342. Power of Four](https://leetcode.com/problems/power-of-four)

[中文文档](/solution/0300-0399/0342.Power%20of%20Four/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>. Hãy trả về <em><code>true</code> nếu nó là lũy thừa của 4; nếu không, trả về <code>false</code></em>.</p>

<p>Số nguyên <code>n</code> là lũy thừa của 4 nếu tồn tại số nguyên <code>x</code> sao cho <code>n == 4<sup>x</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> n = 16
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> false
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> true
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-2<sup>31</sup> &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này mà không dùng vòng lặp hay đệ quy không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem $n$ có phải lũy thừa của 4 không. Có thể chia liên tiếp cho 4; còn kiểm tra bit thì cho lời giải dạng công thức đóng. Vì $4^x=2^{2x}$, nên $n$ phải dương, chỉ có một bit $1$, và bit đó phải nằm ở vị trí chẵn.
>
> Điều kiện $n\&(n-1)=0$ xác nhận $n$ là lũy thừa của 2; còn $n\&\texttt{0xAAAAAAAA}=0$ loại trường hợp bit $1$ nằm ở vị trí lẻ. Kết hợp cả ba điều kiện là đủ.

<!-- thinking:end -->

Nếu một số là lũy thừa của $4$, thì nó phải lớn hơn $0$. Gọi số đó là $4^x$, tức $2^{2x}$. Do đó, biểu diễn nhị phân của nó chỉ có một bit $1$, và bit này nằm ở vị trí chẵn.

Trước tiên, ta kiểm tra số đó có lớn hơn $0$ không. Tiếp theo, ta kiểm tra nó có dạng $2^{2x}$ bằng cách xác nhận phép AND bitwise giữa $n$ và $n-1$ bằng $0$. Cuối cùng, ta kiểm tra bit $1$ có nằm ở vị trí chẵn không bằng cách xác nhận phép AND bitwise giữa $n$ và $\textit{0xAAAAAAAA}$ bằng $0$. Nếu cả ba điều kiện đều thỏa mãn thì số đó là lũy thừa của $4$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian cũng là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPowerOfFour(self, n: int) -> bool:
        return n > 0 and (n & (n - 1)) == 0 and (n & 0xAAAAAAAA) == 0
```

#### Java

```java
class Solution {
    public boolean isPowerOfFour(int n) {
        return n > 0 && (n & (n - 1)) == 0 && (n & 0xaaaaaaaa) == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPowerOfFour(int n) {
        return n > 0 && (n & (n - 1)) == 0 && (n & 0xaaaaaaaa) == 0;
    }
};
```

#### Go

```go
func isPowerOfFour(n int) bool {
	return n > 0 && (n&(n-1)) == 0 && (n&0xaaaaaaaa) == 0
}
```

#### TypeScript

```ts
function isPowerOfFour(n: number): boolean {
    return n > 0 && (n & (n - 1)) == 0 && (n & 0xaaaaaaaa) == 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_power_of_four(n: i32) -> bool {
        n > 0 && (n & (n - 1)) == 0 && (n & 0xaaaaaaaa_u32 as i32) == 0
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfFour = function (n) {
    return n > 0 && (n & (n - 1)) == 0 && (n & 0xaaaaaaaa) == 0;
};
```

#### C#

```cs
public class Solution {
    public bool IsPowerOfFour(int n) {
        return n > 0 && (n & (n - 1)) == 0 && (n & 0xaaaaaaaa) == 0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
