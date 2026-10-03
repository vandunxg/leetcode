---
comments: true
difficulty: Medium
rating: 1505
source: Biweekly Contest 47 Q2
tags:
    - Math
---

<!-- problem:start -->

# [1780. Check if Number is a Sum of Powers of Three](https://leetcode.com/problems/check-if-number-is-a-sum-of-powers-of-three)

[中文文档](/solution/1700-1799/1780.Check%20if%20Number%20is%20a%20Sum%20of%20Powers%20of%20Three/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, trả về <code>true</code> <em>nếu có thể biểu diễn </em><code>n</code><em> dưới dạng tổng của các lũy thừa khác nhau của ba.</em> Nếu không, trả về <code>false</code>.</p>

<p>Số nguyên <code>y</code> là lũy thừa của ba nếu tồn tại số nguyên <code>x</code> sao cho <code>y == 3<sup>x</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 12
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 12 = 3<sup>1</sup> + 3<sup>2</sup>
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 91
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 91 = 3<sup>0</sup> + 3<sup>2</sup> + 3<sup>4</sup>
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 21
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích toán học

<!-- thinking:start -->

> **Tư duy**
>
> $n$ là tổng của các lũy thừa khác nhau của ba khi và chỉ khi mọi chữ số trong biểu diễn tam phân đều là $0$ hoặc $1$, không bao giờ là $2$.
>
> Kiểm tra lần lượt $n\bmod 3$; thất bại nếu số dư lớn hơn $1$, nếu không thì chia $n$ cho $3$ cho đến khi $n$ bằng $0$.

<!-- thinking:end -->

Ta nhận thấy nếu số $n$ có thể biểu diễn dưới dạng tổng của một số lũy thừa "khác nhau" của ba, thì trong biểu diễn tam phân của $n$, mỗi chữ số chỉ có thể là $0$ hoặc $1$.

Vì vậy, ta chuyển $n$ sang hệ tam phân rồi kiểm tra mỗi chữ số có phải là $0$ hoặc $1$ hay không. Nếu không, $n$ không thể biểu diễn dưới dạng tổng của các lũy thừa của ba và ta trả về $\textit{false}$; ngược lại, ta trả về $\textit{true}$.

Độ phức tạp thời gian là $O(\log_3 n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkPowersOfThree(self, n: int) -> bool:
        while n:
            if n % 3 > 1:
                return False
            n //= 3
        return True
```

#### Java

```java
class Solution {
    public boolean checkPowersOfThree(int n) {
        while (n > 0) {
            if (n % 3 > 1) {
                return false;
            }
            n /= 3;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkPowersOfThree(int n) {
        while (n) {
            if (n % 3 > 1) return false;
            n /= 3;
        }
        return true;
    }
};
```

#### Go

```go
func checkPowersOfThree(n int) bool {
	for n > 0 {
		if n%3 > 1 {
			return false
		}
		n /= 3
	}
	return true
}
```

#### TypeScript

```ts
function checkPowersOfThree(n: number): boolean {
    while (n) {
        if (n % 3 > 1) return false;
        n = Math.floor(n / 3);
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn check_powers_of_three(n: i32) -> bool {
        let mut n = n;
        while n > 0 {
            if n % 3 > 1 {
                return false;
            }
            n /= 3;
        }
        true
    }
}
```

#### C#

```cs
public class Solution {
    public bool CheckPowersOfThree(int n) {
        while (n > 0) {
            if (n % 3 > 1) {
                return false;
            }
            n /= 3;
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
