---
comments: true
difficulty: Easy
rating: 1199
source: Weekly Contest 448 Q1
tags:
    - Math
    - Sorting
---

<!-- problem:start -->

# [3536. Maximum Product of Two Digits](https://leetcode.com/problems/maximum-product-of-two-digits)

[中文文档](/solution/3500-3599/3536.Maximum%20Product%20of%20Two%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>.</p>

<p>Hãy trả về tích <strong>lớn nhất</strong> của hai chữ số bất kỳ trong <code>n</code>.</p>

<p><strong>Lưu ý:</strong> Bạn có thể sử dụng <strong>cùng một</strong> chữ số hai lần nếu chữ số đó xuất hiện nhiều hơn một lần trong <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 31</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các chữ số của <code>n</code> là <code>[3, 1]</code>.</li>
    <li>Các tích có thể tạo bởi hai chữ số bất kỳ là: <code>3 * 1 = 3</code>.</li>
    <li>Tích lớn nhất là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 22</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các chữ số của <code>n</code> là <code>[2, 2]</code>.</li>
    <li>Các tích có thể tạo bởi hai chữ số bất kỳ là: <code>2 * 2 = 4</code>.</li>
    <li>Tích lớn nhất là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 124</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các chữ số của <code>n</code> là <code>[1, 2, 4]</code>.</li>
    <li>Các tích có thể tạo bởi hai chữ số bất kỳ là: <code>1 * 2 = 2</code>, <code>1 * 4 = 4</code>, <code>2 * 4 = 8</code>.</li>
    <li>Tích lớn nhất là 8.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>10 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm chữ số lớn nhất và lớn thứ hai

<!-- thinking:start -->

> **Tư duy**
>
> Tích của hai chữ số đạt lớn nhất khi chọn chữ số lớn nhất và lớn thứ hai, không phụ thuộc vào thứ tự. Ta duy trì $a \ge b$ trong khi tách từng chữ số; không cần lưu rồi sắp xếp chúng.
>
> Sau $O(\log n)$ chữ số, trả về $a \cdot b$.

<!-- thinking:end -->

Ta dùng hai biến $a$ và $b$ để lần lượt lưu chữ số lớn nhất và lớn thứ hai hiện tại. Ta duyệt qua từng chữ số của $n$; nếu chữ số hiện tại lớn hơn $a$, ta gán giá trị của $a$ cho $b$, sau đó đặt $a$ bằng chữ số hiện tại. Nếu không, nhưng chữ số hiện tại lớn hơn $b$, ta đặt $b$ bằng chữ số đó. Cuối cùng, ta trả về $a \times b$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số đầu vào, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProduct(self, n: int) -> int:
        a = b = 0
        while n:
            n, x = divmod(n, 10)
            if a < x:
                a, b = x, a
            elif b < x:
                b = x
        return a * b
```

#### Java

```java
class Solution {
    public int maxProduct(int n) {
        int a = 0, b = 0;
        for (; n > 0; n /= 10) {
            int x = n % 10;
            if (a < x) {
                b = a;
                a = x;
            } else if (b < x) {
                b = x;
            }
        }
        return a * b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProduct(int n) {
        int a = 0, b = 0;
        for (; n; n /= 10) {
            int x = n % 10;
            if (a < x) {
                b = a;
                a = x;
            } else if (b < x) {
                b = x;
            }
        }
        return a * b;
    }
};
```

#### Go

```go
func maxProduct(n int) int {
    a, b := 0, 0
    for ; n > 0; n /= 10 {
        x := n % 10
        if a < x {
            b, a = a, x
        } else if b < x {
            b = x
        }
    }
    return a * b
}
```

#### TypeScript

```ts
function maxProduct(n: number): number {
    let [a, b] = [0, 0];
    for (; n; n = Math.floor(n / 10)) {
        const x = n % 10;
        if (a < x) {
            [a, b] = [x, a];
        } else if (b < x) {
            b = x;
        }
    }
    return a * b;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_product(mut n: i32) -> i32 {
        let (mut a, mut b) = (0, 0);

        while n > 0 {
            let x = n % 10;
            if a < x {
                b = a;
                a = x;
            } else if b < x {
                b = x;
            }
            n /= 10;
        }

        a * b
    }
}
```

#### C#

```cs
public class Solution {
    public int MaxProduct(int n) {
        int a = 0, b = 0;
        while (n > 0) {
            int x = n % 10;
            if (a < x) {
                b = a;
                a = x;
            } else if (b < x) {
                b = x;
            }
            n /= 10;
        }
        return a * b;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
