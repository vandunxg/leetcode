---
comments: true
difficulty: Easy
rating: 1182
source: Weekly Contest 342 Q2
tags:
    - Math
---

<!-- problem:start -->

# [2652. Sum Multiples](https://leetcode.com/problems/sum-multiples)

[中文文档](/solution/2600-2699/2652.Sum%20Multiples/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code>, hãy tính tổng tất cả các số nguyên trong đoạn <code>[1, n]</code> <strong>bao gồm cả hai đầu mút</strong> và chia hết cho <code>3</code>, <code>5</code> hoặc <code>7</code>.</p>

<p>Trả về <em>một số nguyên biểu thị tổng của tất cả các số trong đoạn đã cho thỏa mãn điều kiện trên.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7
<strong>Đầu ra:</strong> 21
<strong>Giải thích:</strong> Các số trong đoạn <code>[1, 7]</code> chia hết cho <code>3</code>, <code>5,</code> hoặc <code>7 </code>là <code>3, 5, 6, 7</code>. Tổng của các số này là <code>21</code>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 40
<strong>Giải thích:</strong> Các số trong đoạn <code>[1, 10] that are</code> chia hết cho <code>3</code>, <code>5,</code> hoặc <code>7</code> là <code>3, 5, 6, 7, 9, 10</code>. Tổng của các số này là 40.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 9
<strong>Đầu ra:</strong> 30
<strong>Giải thích:</strong> Các số trong đoạn <code>[1, 9]</code> chia hết cho <code>3</code>, <code>5</code> hoặc <code>7</code> là <code>3, 5, 6, 7, 9</code>. Tổng của các số này là <code>30</code>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Cần tính tổng các số trong $[1,n]$ chia hết cho $3$, $5$ hoặc $7$. Vì $n \le 1000$, ta có thể duyệt qua mọi $x$ và kiểm tra các số chia hết này.

<!-- thinking:end -->

Ta duyệt trực tiếp mọi số $x$ trong $[1,..n]$, nếu $x$ chia hết cho $3$, $5$ hoặc $7$ thì cộng $x$ vào đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên được cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfMultiples(self, n: int) -> int:
        return sum(x for x in range(1, n + 1) if x % 3 == 0 or x % 5 == 0 or x % 7 == 0)
```

#### Java

```java
class Solution {
    public int sumOfMultiples(int n) {
        int ans = 0;
        for (int x = 1; x <= n; ++x) {
            if (x % 3 == 0 || x % 5 == 0 || x % 7 == 0) {
                ans += x;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfMultiples(int n) {
        int ans = 0;
        for (int x = 1; x <= n; ++x) {
            if (x % 3 == 0 || x % 5 == 0 || x % 7 == 0) {
                ans += x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfMultiples(n int) (ans int) {
	for x := 1; x <= n; x++ {
		if x%3 == 0 || x%5 == 0 || x%7 == 0 {
			ans += x
		}
	}
	return
}
```

#### TypeScript

```ts
function sumOfMultiples(n: number): number {
    let ans = 0;
    for (let x = 1; x <= n; ++x) {
        if (x % 3 === 0 || x % 5 === 0 || x % 7 === 0) {
            ans += x;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_of_multiples(n: i32) -> i32 {
        let mut ans = 0;

        for x in 1..=n {
            if x % 3 == 0 || x % 5 == 0 || x % 7 == 0 {
                ans += x;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học (Nguyên lý bao hàm - loại trừ)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt tuyến tính theo $n$. Các bội của $x$ tạo thành một cấp số cộng có tổng tính được trực tiếp; nguyên lý bao hàm - loại trừ loại bỏ phần giao trong $O(1)$.
>
> Giá trị cần tìm là $f(3)+f(5)+f(7)-f(15)-f(21)-f(35)+f(105)$.

<!-- thinking:end -->

Ta định nghĩa hàm $f(x)$ biểu thị tổng các số trong $[1,..n]$ chia hết cho $x$. Có $m = \left\lfloor \frac{n}{x} \right\rfloor$ số chia hết cho $x$, lần lượt là $x$, $2x$, $3x$, $\cdots$, $mx$, tạo thành một cấp số cộng với số hạng đầu là $x$, số hạng cuối là $mx$ và có $m$ số hạng. Do đó, $f(x) = \frac{(x + mx) \times m}{2}$.

Theo nguyên lý bao hàm - loại trừ, ta có thể tính đáp án như sau:

$$
f(3) + f(5) + f(7) - f(3 \times 5) - f(3 \times 7) - f(5 \times 7) + f(3 \times 5 \times 7)
$$

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfMultiples(self, n: int) -> int:
        def f(x: int) -> int:
            m = n // x
            return (x + m * x) * m // 2

        return f(3) + f(5) + f(7) - f(3 * 5) - f(3 * 7) - f(5 * 7) + f(3 * 5 * 7)
```

#### Java

```java
class Solution {
    private int n;

    public int sumOfMultiples(int n) {
        this.n = n;
        return f(3) + f(5) + f(7) - f(3 * 5) - f(3 * 7) - f(5 * 7) + f(3 * 5 * 7);
    }

    private int f(int x) {
        int m = n / x;
        return (x + m * x) * m / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfMultiples(int n) {
        auto f = [&](int x) {
            int m = n / x;
            return (x + m * x) * m / 2;
        };
        return f(3) + f(5) + f(7) - f(3 * 5) - f(3 * 7) - f(5 * 7) + f(3 * 5 * 7);
    }
};
```

#### Go

```go
func sumOfMultiples(n int) int {
	f := func(x int) int {
		m := n / x
		return (x + m*x) * m / 2
	}
	return f(3) + f(5) + f(7) - f(3*5) - f(3*7) - f(5*7) + f(3*5*7)
}
```

#### TypeScript

```ts
function sumOfMultiples(n: number): number {
    const f = (x: number): number => {
        const m = Math.floor(n / x);
        return ((x + m * x) * m) >> 1;
    };
    return f(3) + f(5) + f(7) - f(3 * 5) - f(3 * 7) - f(5 * 7) + f(3 * 5 * 7);
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_of_multiples(n: i32) -> i32 {
        (1..=n)
            .filter(|&x| (x % 3 == 0 || x % 5 == 0 || x % 7 == 0))
            .sum()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Đây là công thức bao hàm - loại trừ giống Lời giải 2, được viết lại bằng Rust; thuật toán và các hằng số không thay đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
impl Solution {
    pub fn sum_of_multiples(n: i32) -> i32 {
        fn f(x: i32, n: i32) -> i32 {
            let m = n / x;
            ((x + m * x) * m) / 2
        }

        f(3, n) + f(5, n) + f(7, n) - f(3 * 5, n) - f(3 * 7, n) - f(5 * 7, n) + f(3 * 5 * 7, n)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
