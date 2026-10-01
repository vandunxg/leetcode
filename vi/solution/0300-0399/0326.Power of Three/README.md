---
comments: true
difficulty: Easy
tags:
    - Recursion
    - Math
---

<!-- problem:start -->

# [326. Power of Three](https://leetcode.com/problems/power-of-three)

[中文文档](/solution/0300-0399/0326.Power%20of%20Three/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về <em><code>true</code> nếu nó là lũy thừa của 3; nếu không thì trả về <code>false</code></em>.</p>

<p>Số nguyên <code>n</code> là lũy thừa của 3 nếu tồn tại số nguyên <code>x</code> sao cho <code>n == 3<sup>x</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 27
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 27 = 3<sup>3</sup>
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 0
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không tồn tại x sao cho 3<sup>x</sup> = 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = -1
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không tồn tại x sao cho 3<sup>x</sup> = (-1).
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

### Lời giải 1: Chia thử

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem $n$ có phải lũy thừa của 3 hay không. Có thể nhân lặp, nhưng chia thử trực tiếp hơn: khi $n>2$, nếu $n$ không chia hết cho $3$ thì loại; nếu chia hết thì chia tiếp cho $3$. Giá trị còn lại cuối cùng phải là $1$.

<!-- thinking:end -->

Nếu $n \gt 2$, ta liên tục chia $n$ cho $3$. Nếu $n$ không chia hết cho $3$ thì nó không phải lũy thừa của $3$; ngược lại, tiếp tục chia cho $3$ cho đến khi $n$ nhỏ hơn hoặc bằng $2$. Nếu $n$ bằng $1$ thì nó là lũy thừa của $3$; nếu không thì không phải.

Độ phức tạp thời gian là $O(\log_3n)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPowerOfThree(self, n: int) -> bool:
        while n > 2:
            if n % 3:
                return False
            n //= 3
        return n == 1
```

#### Java

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        while (n > 2) {
            if (n % 3 != 0) {
                return false;
            }
            n /= 3;
        }
        return n == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPowerOfThree(int n) {
        while (n > 2) {
            if (n % 3) {
                return false;
            }
            n /= 3;
        }
        return n == 1;
    }
};
```

#### Go

```go
func isPowerOfThree(n int) bool {
	for n > 2 {
		if n%3 != 0 {
			return false
		}
		n /= 3
	}
	return n == 1
}
```

#### TypeScript

```ts
function isPowerOfThree(n: number): boolean {
    while (n > 2) {
        if (n % 3 !== 0) {
            return false;
        }
        n = Math.floor(n / 3);
    }
    return n === 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_power_of_three(mut n: i32) -> bool {
        while n > 2 {
            if n % 3 != 0 {
                return false;
            }
            n /= 3;
        }
        n == 1
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfThree = function (n) {
    while (n > 2) {
        if (n % 3 !== 0) {
            return false;
        }
        n = Math.floor(n / 3);
    }
    return n === 1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Chia thử cần $O(\log n)$ bước. Lũy thừa lớn nhất của 3 biểu diễn được trong $32$ bit là $3^{19}=1162261467$; số dương $n$ là lũy thừa của 3 khi và chỉ khi nó là ước của hằng số này. Chỉ cần một phép modulo.

<!-- thinking:end -->

Nếu $n$ là lũy thừa của $3$, giá trị lớn nhất của $n$ là $3^{19} = 1162261467$. Vì vậy, chỉ cần kiểm tra xem $n$ có phải là ước của $3^{19}$ hay không.

Độ phức tạp thời gian là $O(1)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPowerOfThree(self, n: int) -> bool:
        return n > 0 and 1162261467 % n == 0
```

#### Java

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
};
```

#### Go

```go
func isPowerOfThree(n int) bool {
	return n > 0 && 1162261467%n == 0
}
```

#### TypeScript

```ts
function isPowerOfThree(n: number): boolean {
    return n > 0 && 1162261467 % n == 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_power_of_three(mut n: i32) -> bool {
        n > 0 && 1162261467 % n == 0
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfThree = function (n) {
    return n > 0 && 1162261467 % n == 0;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
