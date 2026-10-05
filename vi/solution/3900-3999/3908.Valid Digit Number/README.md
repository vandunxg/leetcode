---
comments: true
difficulty: Easy
rating: 1319
source: Biweekly Contest 181 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3908. Valid Digit Number](https://leetcode.com/problems/valid-digit-number)

[中文文档](/solution/3900-3999/3908.Valid%20Digit%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và một chữ số <code>x</code>.</p>

<p>Một số được xem là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li>Nó chứa <strong>ít nhất một</strong> lần xuất hiện của chữ số <code>x</code>, và</li>
	<li>Nó <strong>không bắt đầu</strong> bằng chữ số <code>x</code>.</li>
</ul>

<p>Trả về <code>true</code> nếu <code>n</code> là <strong>hợp lệ</strong>, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 101, x = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số này chứa chữ số 0 tại chỉ số 1. Nó không bắt đầu bằng 0, nên thỏa mãn cả hai điều kiện. Do đó, đáp án là <code>true</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 232, x = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số này bắt đầu bằng 2, vi phạm điều kiện. Do đó, đáp án là <code>false</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, x = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số này không chứa chữ số 1. Do đó, đáp án là <code>false</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>5</sup>​​​​​​​</code></li>
	<li><code>0 &lt;= x &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Việc chuyển $n$ thành chuỗi rồi kiểm tra chữ số đầu tiên cùng sự xuất hiện của $x$ phù hợp khi $n\le 10^5$, nhưng ta chỉ cần “ $x$ xuất hiện và chữ số đầu tiên không phải $x$”.
>
> Tách chữ số cuối và chia cho $10$ giúp duyệt các chữ số bằng phép tính: ghi nhớ xem có chữ số nào bằng $x$ hay không, rồi dừng tại chữ số đầu tiên. Khi đó, tính hợp lệ chính xác là “đã gặp $x$ và chữ số đầu tiên còn lại không phải $x$”.
>
> Điều kiện vòng lặp $n>9$ giữ cho $n$ bằng chữ số đầu tiên đó.

<!-- thinking:end -->

Dùng một biến Boolean $\textit{hasX}$ để ghi nhận việc chữ số $x$ có xuất hiện trong $n$ hay không.

Ta liên tục lấy chữ số cuối của $n$ và so sánh với $x$. Nếu chúng bằng nhau, ta đặt $\textit{hasX}$ thành $\texttt{true}$. Đồng thời, ta chia $n$ cho $10$ để loại bỏ chữ số cuối. Khi $n$ nhỏ hơn hoặc bằng $9$, nghĩa là ta đã kiểm tra tất cả các chữ số. Lúc này, nếu $\textit{hasX}$ là $\texttt{true}$ và $n$ không bằng $x$, thì $n$ là số hợp lệ và ta trả về $\texttt{true}$; nếu không, ta trả về $\texttt{false}$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số nguyên đầu vào. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validDigit(self, n: int, x: int) -> bool:
        has_x = False
        while n > 9:
            has_x = has_x or n % 10 == x
            n //= 10
        return has_x and n != x
```

#### Java

```java
class Solution {
    public boolean validDigit(int n, int x) {
        boolean hasX = false;
        while (n > 9) {
            hasX = hasX || (n % 10 == x);
            n /= 10;
        }
        return hasX && (n != x);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validDigit(int n, int x) {
        bool hasX = false;
        while (n > 9) {
            hasX = hasX || (n % 10 == x);
            n /= 10;
        }
        return hasX && (n != x);
    }
};
```

#### Go

```go
func validDigit(n int, x int) bool {
	hasX := false
	for n > 9 {
		hasX = hasX || (n%10 == x)
		n /= 10
	}
	return hasX && (n != x)
}
```

#### TypeScript

```ts
function validDigit(n: number, x: number): boolean {
    let hasX: boolean = false;
    while (n > 9) {
        hasX = hasX || n % 10 === x;
        n = Math.floor(n / 10);
    }
    return hasX && n !== x;
}
```

#### Rust

```rust
impl Solution {
    pub fn valid_digit(mut n: i32, x: i32) -> bool {
        let mut has_x = false;
        while n > 9 {
            has_x = has_x || (n % 10 == x);
            n /= 10;
        }
        has_x && (n != x)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
