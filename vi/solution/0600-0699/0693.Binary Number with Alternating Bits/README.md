---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [693. Binary Number with Alternating Bits](https://leetcode.com/problems/binary-number-with-alternating-bits)

[中文文档](/solution/0600-0699/0693.Binary%20Number%20with%20Alternating%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương, hãy kiểm tra xem các bit của nó có xen kẽ hay không, tức là hai bit liền kề luôn có giá trị khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Biểu diễn nhị phân của 5 là: 101
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Biểu diễn nhị phân của 7 là: 111.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 11
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Biểu diễn nhị phân của 11 là: 1011.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra các bit liền kề có luân phiên nghiêm ngặt hay không. Chỉ cần duyệt từng bit.
>
> Đọc bit thấp nhất, dịch bit rồi trả về thất bại nếu bit đó bằng bit trước.

<!-- thinking:end -->

Ta dịch phải tuần hoàn $n$ cho đến khi nó trở thành $0$, đồng thời kiểm tra các bit của $n$ có luân phiên hay không. Nếu trong vòng lặp phát hiện $0$ và $1$ không luân phiên, ta trả về ngay $\textit{false}$. Nếu vòng lặp kết thúc mà không gặp trường hợp đó, ta trả về $\textit{true}$.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasAlternatingBits(self, n: int) -> bool:
        prev = -1
        while n:
            curr = n & 1
            if prev == curr:
                return False
            prev = curr
            n >>= 1
        return True
```

#### Java

```java
class Solution {
    public boolean hasAlternatingBits(int n) {
        int prev = -1;
        while (n != 0) {
            int curr = n & 1;
            if (prev == curr) {
                return false;
            }
            prev = curr;
            n >>= 1;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasAlternatingBits(int n) {
        int prev = -1;
        while (n) {
            int curr = n & 1;
            if (prev == curr) return false;
            prev = curr;
            n >>= 1;
        }
        return true;
    }
};
```

#### Go

```go
func hasAlternatingBits(n int) bool {
	prev := -1
	for n != 0 {
		curr := n & 1
		if prev == curr {
			return false
		}
		prev = curr
		n >>= 1
	}
	return true
}
```

#### TypeScript

```ts
function hasAlternatingBits(n: number): boolean {
    let prev = -1;

    while (n !== 0) {
        const curr = n & 1;
        if (prev === curr) {
            return false;
        }
        prev = curr;
        n >>= 1;
    }

    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn has_alternating_bits(mut n: i32) -> bool {
        let mut prev: i32 = -1;

        while n != 0 {
            let curr = n & 1;
            if prev == curr {
                return false;
            }
            prev = curr;
            n >>= 1;
        }

        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt từng bit tốn $O(\log n)$. Biểu thức $n\oplus(n\gg 1)$ tạo thành một dãy toàn bit 1 khi và chỉ khi các bit của $n$ luân phiên; sau đó $n\&(n+1)=0$ xác nhận điều này trong thời gian hằng số.

<!-- thinking:end -->

Giả sử $\text{01}$ xuất hiện xen kẽ, phép XOR lệch vị trí sẽ biến toàn bộ các bit phía sau thành $\text{1}$. Cộng thêm $\text{1}$ sẽ được một lũy thừa của $2$, tức một số $n$ chỉ có duy nhất một bit bằng $\text{1}$. Khi đó, dùng $\text{n} \& (\text{n} + 1)$ sẽ loại bỏ bit $\text{1}$ cuối cùng.

Lúc này, kiểm tra kết quả có bằng $\text{0}$ hay không. Nếu có, giả định ban đầu đúng và dãy $\text{01}$ luân phiên.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasAlternatingBits(self, n: int) -> bool:
        n ^= n >> 1
        return (n & (n + 1)) == 0
```

#### Java

```java
class Solution {
    public boolean hasAlternatingBits(int n) {
        n ^= (n >> 1);
        return (n & (n + 1)) == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasAlternatingBits(int n) {
        n ^= (n >> 1);
        return (n & ((long) n + 1)) == 0;
    }
};
```

#### Go

```go
func hasAlternatingBits(n int) bool {
	n ^= (n >> 1)
	return (n & (n + 1)) == 0
}
```

#### TypeScript

```ts
function hasAlternatingBits(n: number): boolean {
    n ^= n >> 1;
    return (n & (n + 1)) === 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn has_alternating_bits(n: i32) -> bool {
        let mut x = n ^ (n >> 1);
        (x & (x + 1)) == 0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
