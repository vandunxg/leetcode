---
comments: true
difficulty: Easy
tags:
    - Math
---

<!-- problem:start -->

# [2235. Add Two Integers](https://leetcode.com/problems/add-two-integers)

[中文文档](/solution/2200-2299/2235.Add%20Two%20Integers/README.md)

## Mô tả

<!-- description:start -->

Cho hai số nguyên <code>num1</code> và <code>num2</code>, hãy trả về <em><strong>tổng</strong> của hai số nguyên này</em>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = 12, num2 = 5
<strong>Đầu ra:</strong> 17
<strong>Giải thích:</strong> num1 là 12, num2 là 5, và tổng của chúng là 12 + 5 = 17, nên trả về 17.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num1 = -10, num2 = 4
<strong>Đầu ra:</strong> -6
<strong>Giải thích:</strong> num1 + num2 = -6, nên trả về -6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-100 &lt;= num1, num2 &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cộng hai số nguyên trong $[-100,100]$. Toán tử cộng của ngôn ngữ đã thực hiện việc này trong thời gian hằng số; không cần xử lý bit nhớ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sum(self, num1: int, num2: int) -> int:
        return num1 + num2
```

#### Java

```java
class Solution {
    public int sum(int num1, int num2) {
        return num1 + num2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sum(int num1, int num2) {
        return num1 + num2;
    }
};
```

#### Go

```go
func sum(num1 int, num2 int) int {
	return num1 + num2
}
```

#### TypeScript

```ts
function sum(num1: number, num2: number): number {
    return num1 + num2;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum(num1: i32, num2: i32) -> i32 {
        num1 + num2
    }
}
```

#### C

```c
int sum(int num1, int num2) {
    return num1 + num2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng toán tử cộng. Khi không được dùng toán tử này, ta mô phỏng phép cộng thủ công trên các bit: xor là tổng không có bit nhớ, còn bit nhớ là phép and theo bit rồi dịch trái; lặp lại cho đến khi bit nhớ bằng 0.
>
> Số nguyên trong Python không bị giới hạn, nên ta dùng mặt nạ $32$ bit với $0\texttt{xFFFFFFFF}$. Nếu bit dấu được đặt, ta chuyển kết quả về số nguyên Python âm bằng bù hai.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sum(self, num1: int, num2: int) -> int:
        num1, num2 = num1 & 0xFFFFFFFF, num2 & 0xFFFFFFFF
        while num2:
            carry = ((num1 & num2) << 1) & 0xFFFFFFFF
            num1, num2 = num1 ^ num2, carry
        return num1 if num1 < 0x80000000 else ~(num1 ^ 0xFFFFFFFF)
```

#### Java

```java
class Solution {
    public int sum(int num1, int num2) {
        while (num2 != 0) {
            int carry = (num1 & num2) << 1;
            num1 ^= num2;
            num2 = carry;
        }
        return num1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sum(int num1, int num2) {
        while (num2) {
            unsigned int carry = (unsigned int) (num1 & num2) << 1;
            num1 ^= num2;
            num2 = carry;
        }
        return num1;
    }
};
```

#### Go

```go
func sum(num1 int, num2 int) int {
	for num2 != 0 {
		carry := (num1 & num2) << 1
		num1 ^= num2
		num2 = carry
	}
	return num1
}
```

#### TypeScript

```ts
function sum(num1: number, num2: number): number {
    while (num2) {
        const carry = (num1 & num2) << 1;
        num1 ^= num2;
        num2 = carry;
    }
    return num1;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum(num1: i32, num2: i32) -> i32 {
        let mut num1 = num1;
        let mut num2 = num2;
        while num2 != 0 {
            let carry = (num1 & num2) << 1;
            num1 ^= num2;
            num2 = carry;
        }
        num1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
