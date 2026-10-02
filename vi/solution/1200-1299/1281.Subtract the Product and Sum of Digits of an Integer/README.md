---
comments: true
difficulty: Easy
rating: 1141
source: Weekly Contest 166 Q1
tags:
    - Math
---

<!-- problem:start -->

# [1281. Subtract the Product and Sum of Digits of an Integer](https://leetcode.com/problems/subtract-the-product-and-sum-of-digits-of-an-integer)

[中文文档](/solution/1200-1299/1281.Subtract%20the%20Product%20and%20Sum%20of%20Digits%20of%20an%20Integer/README.md)

## Mô tả

<!-- description:start -->

Cho số nguyên <code>n</code>, hãy trả về hiệu giữa tích các chữ số và tổng các chữ số của nó.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 234
<strong>Đầu ra:</strong> 15 
<b>Giải thích:</b> 
Tích các chữ số = 2 * 3 * 4 = 24 
Tổng các chữ số = 2 + 3 + 4 = 9 
Kết quả = 24 - 9 = 15
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4421
<strong>Đầu ra:</strong> 21
<b>Giải thích: 
</b>Tích các chữ số = 4 * 4 * 2 * 1 = 32 
Tổng các chữ số = 4 + 4 + 2 + 1 = 11 
Kết quả = 32 - 11 = 21
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10^5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n \le 10^5$, ta liên tục chia cho $10$ để tích lũy tích và tổng các chữ số, rồi lấy hiệu ở cuối. Không cần chuyển thành chuỗi. Vòng lặp chạy một lần cho mỗi chữ số.

<!-- thinking:end -->

Ta dùng hai biến $x$ và $y$ lần lượt lưu tích và tổng các chữ số. Ban đầu, $x=1,y=0$.

Khi $n \gt 0$, mỗi vòng lặp lấy phần dư của $n$ khi chia cho $10$ để nhận chữ số hiện tại $v$, rồi chia $n$ cho $10$ để chuẩn bị cho vòng tiếp theo. Trong mỗi vòng, cập nhật $x = x \times v$, $y = y + v$.

Cuối cùng, trả về $x - y$.

Độ phức tạp thời gian là $O(\log n)$, với $n$ là số nguyên đầu vào. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subtractProductAndSum(self, n: int) -> int:
        nums = list(map(int, str(n)))
        return prod(nums) - sum(nums)
```

#### Java

```java
class Solution {
    public int subtractProductAndSum(int n) {
        int x = 1, y = 0;
        for (; n > 0; n /= 10) {
            int v = n % 10;
            x *= v;
            y += v;
        }
        return x - y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subtractProductAndSum(int n) {
        int x = 1, y = 0;
        for (; n; n /= 10) {
            int v = n % 10;
            x *= v;
            y += v;
        }
        return x - y;
    }
};
```

#### Go

```go
func subtractProductAndSum(n int) int {
	x, y := 1, 0
	for ; n > 0; n /= 10 {
		v := n % 10
		x *= v
		y += v
	}
	return x - y
}
```

#### TypeScript

```ts
function subtractProductAndSum(n: number): number {
    let [x, y] = [1, 0];
    for (; n > 0; n = Math.floor(n / 10)) {
        const v = n % 10;
        x *= v;
        y += v;
    }
    return x - y;
}
```

#### Rust

```rust
impl Solution {
    pub fn subtract_product_and_sum(mut n: i32) -> i32 {
        let mut x = 1;
        let mut y = 0;
        while n != 0 {
            let v = n % 10;
            n /= 10;
            x *= v;
            y += v;
        }
        x - y
    }
}
```

#### C#

```cs
public class Solution {
    public int SubtractProductAndSum(int n) {
        int x = 1;
        int y = 0;
        for (; n > 0; n /= 10) {
            int v = n % 10;
            x *= v;
            y += v;
        }
        return x - y;
    }
}
```

#### C

```c
int subtractProductAndSum(int n) {
    int x = 1;
    int y = 0;
    for (; n > 0; n /= 10) {
        int v = n % 10;
        x *= v;
        y += v;
    }
    return x - y;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
