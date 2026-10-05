---
comments: true
difficulty: Easy
rating: 1247
source: Weekly Contest 477 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3754. Concatenate Non-Zero Digits and Multiply by Sum I](https://leetcode.com/problems/concatenate-non-zero-digits-and-multiply-by-sum-i)

[中文文档](/solution/3700-3799/3754.Concatenate%20Non-Zero%20Digits%20and%20Multiply%20by%20Sum%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>.</p>

<p>Hãy tạo một số nguyên mới <code>x</code> bằng cách nối tất cả <strong>chữ số khác 0</strong> của <code>n</code> theo đúng thứ tự ban đầu. Nếu không có <strong>chữ số khác 0</strong> nào, <code>x = 0</code>.</p>

<p>Gọi <code>sum</code> là <strong>tổng các chữ số</strong> trong <code>x</code>.</p>

<p>Trả về một số nguyên biểu diễn giá trị của <code>x * sum</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10203004</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12340</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các chữ số khác 0 là 1, 2, 3 và 4. Do đó, <code>x = 1234</code>.</li>
	<li>Tổng các chữ số là <code>sum = 1 + 2 + 3 + 4 = 10</code>.</li>
	<li>Vì vậy, đáp án là <code>x * sum = 1234 * 10 = 12340</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1000</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chữ số khác 0 duy nhất là 1, nên <code>x = 1</code> và <code>sum = 1</code>.</li>
	<li>Vì vậy, đáp án là <code>x * sum = 1 * 1 = 1</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $n$ có ít chữ số, nên ta có thể làm trực tiếp theo định nghĩa. Tách từng chữ số từ phải sang trái, mỗi chữ số khác 0 sẽ đồng thời cập nhật số nguyên được nối $x$ và tổng các chữ số $s$; đáp án là $x\cdot s$.

<!-- thinking:end -->

Ta có thể mô phỏng thao tác cần thực hiện bằng cách xử lý từng chữ số của số nguyên. Khi xử lý mỗi chữ số, ta nối các chữ số khác 0 để tạo thành số nguyên mới $x$ và tính tổng các chữ số $s$. Cuối cùng, trả về $x \times s$.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumAndMultiply(self, n: int) -> int:
        p = 1
        x = s = 0
        while n:
            n, v = divmod(n, 10)
            if v:
                s += v
                x += p * v
                p *= 10
        return x * s
```

#### Java

```java
class Solution {
    public long sumAndMultiply(int n) {
        int p = 1;
        int x = 0, s = 0;
        for (; n > 0; n /= 10) {
            int v = n % 10;
            if (v != 0) {
                s += v;
                x += p * v;
                p *= 10;
            }
        }
        return 1L * x * s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long sumAndMultiply(int n) {
        int p = 1;
        int x = 0, s = 0;
        for (; n > 0; n /= 10) {
            int v = n % 10;
            if (v != 0) {
                s += v;
                x += p * v;
                p *= 10;
            }
        }
        return 1LL * x * s;
    }
};
```

#### Go

```go
func sumAndMultiply(n int) int64 {
	p := 1
	x := 0
	s := 0
	for n > 0 {
		v := n % 10
		if v != 0 {
			s += v
			x += p * v
			p *= 10
		}
		n /= 10
	}
	return int64(x) * int64(s)
}
```

#### TypeScript

```ts
function sumAndMultiply(n: number): number {
    let p = 1;
    let x = 0;
    let s = 0;

    while (n > 0) {
        const v = n % 10;
        if (v !== 0) {
            s += v;
            x += p * v;
            p *= 10;
        }
        n = Math.floor(n / 10);
    }

    return x * s;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_and_multiply(mut n: i32) -> i64 {
        let mut p = 1;
        let mut x = 0;
        let mut s = 0;

        while n > 0 {
            let v = n % 10;
            if v != 0 {
                s += v;
                x += p * v;
                p *= 10;
            }
            n /= 10;
        }

        x as i64 * s as i64
    }
}
```

#### C#

```cs
public class Solution {
    public long SumAndMultiply(int n) {
        int p = 1;
        int x = 0, s = 0;

        while (n > 0) {
            int v = n % 10;
            if (v != 0) {
                s += v;
                x += p * v;
                p *= 10;
            }
            n /= 10;
        }

        return 1L * x * s;
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var sumAndMultiply = function (n) {
    let p = 1;
    let x = 0;
    let s = 0;

    while (n > 0) {
        const v = n % 10;
        if (v !== 0) {
            s += v;
            x += p * v;
            p *= 10;
        }
        n = Math.floor(n / 10);
    }

    return x * s;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
