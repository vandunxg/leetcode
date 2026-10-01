---
comments: true
difficulty: Easy
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [367. Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square)

[中文文档](/solution/0300-0399/0367.Valid%20Perfect%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên dương num, trả về <code>true</code> <em>nếu</em> <code>num</code> <em>là số chính phương, hoặc</em> <code>false</code> <em>nếu không phải</em>.</p>

<p><strong>Số chính phương</strong> là số nguyên bằng bình phương của một số nguyên khác. Nói cách khác, đó là tích của một số nguyên với chính nó.</p>

<p>Bạn không được dùng hàm có sẵn trong thư viện như <code>sqrt</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 16
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta trả về true vì 4 * 4 = 16 và 4 là số nguyên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 14
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Ta trả về false vì 3.742 * 3.742 = 14 nhưng 3.742 không phải số nguyên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> $num$ có phải số chính phương không? Duyệt đến $\sqrt{num}$ tốn $O(\sqrt n)$. Chỉ cần tìm kiếm nhị phân trên $x^2$.
>
> Tìm $x$ nhỏ nhất trong $[1,num]$ sao cho $x^2\ge num$, rồi kiểm tra xem hai vế có bằng nhau không. Code dùng `bisect_left` với hàm $x\mapsto x^2$.

<!-- thinking:end -->

Ta có thể dùng tìm kiếm nhị phân để giải bài toán này. Đặt cận trái $l = 1$ và cận phải $r = num$, rồi tìm số nguyên nhỏ nhất $x$ trong khoảng $[l, r]$ thỏa mãn $x^2 \geq num$. Cuối cùng, nếu $x^2 = num$ thì $num$ là số chính phương.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số đã cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        l = bisect_left(range(1, num + 1), num, key=lambda x: x * x) + 1
        return l * l == num
```

#### Java

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        int l = 1, r = num;
        while (l < r) {
            int mid = (l + r) >>> 1;
            if (1L * mid * mid >= num) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l * l == num;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {
        int l = 1, r = num;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (1LL * mid * mid >= num) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return 1LL * l * l == num;
    }
};
```

#### Go

```go
func isPerfectSquare(num int) bool {
	l := sort.Search(num, func(i int) bool { return i*i >= num })
	return l*l == num
}
```

#### TypeScript

```ts
function isPerfectSquare(num: number): boolean {
    let [l, r] = [1, num];
    while (l < r) {
        const mid = (l + r) >> 1;
        if (mid >= num / mid) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l * l === num;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_perfect_square(num: i32) -> bool {
        let mut l = 1;
        let mut r = num as i64;
        while l < r {
            let mid = (l + r) / 2;
            if mid * mid >= (num as i64) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        l * l == (num as i64)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm nhị phân dựa vào tính đơn điệu của các bình phương. Tổng các số lẻ $1+3+\cdots+(2n-1)=n^2$, nên ta trừ dần các số lẻ tăng liên tiếp cho đến khi $num$ bằng $0$. Cách này có code ngắn hơn và chạy trong $O(\sqrt n)$.

<!-- thinking:end -->

Vì $1 + 3 + 5 + \cdots + (2n - 1) = n^2$, ta có thể lần lượt trừ $1, 3, 5, \cdots$ khỏi $num$. Nếu cuối cùng $num$ bằng $0$, thì $num$ là số chính phương.

Độ phức tạp thời gian là $O(\sqrt n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        i = 1
        while num > 0:
            num -= i
            i += 2
        return num == 0
```

#### Java

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        for (int i = 1; num > 0; i += 2) {
            num -= i;
        }
        return num == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {
        for (int i = 1; num > 0; i += 2) {
            num -= i;
        }
        return num == 0;
    }
};
```

#### Go

```go
func isPerfectSquare(num int) bool {
	for i := 1; num > 0; i += 2 {
		num -= i
	}
	return num == 0
}
```

#### TypeScript

```ts
function isPerfectSquare(num: number): boolean {
    let i = 1;
    while (num > 0) {
        num -= i;
        i += 2;
    }
    return num === 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_perfect_square(mut num: i32) -> bool {
        let mut i = 1;
        while num > 0 {
            num -= i;
            i += 2;
        }
        num == 0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
