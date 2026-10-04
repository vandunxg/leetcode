---
comments: true
difficulty: Easy
rating: 1184
source: Weekly Contest 329 Q1
tags:
    - Math
---

<!-- problem:start -->

# [2544. Alternating Digit Sum](https://leetcode.com/problems/alternating-digit-sum)

[中文文档](/solution/2500-2599/2544.Alternating%20Digit%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code>. Mỗi chữ số của <code>n</code> được gán một dấu theo các quy tắc sau:</p>

<ul>
	<li><strong>Chữ số ở hàng cao nhất</strong> được gán dấu <strong>dương</strong>.</li>
	<li>Mỗi chữ số còn lại có dấu ngược với các chữ số liền kề.</li>
</ul>

<p>Hãy trả về <em>tổng của tất cả các chữ số cùng với dấu tương ứng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 521
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> (+5) + (-2) + (+1) = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 111
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> (+1) + (-1) + (+1) = 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 886996
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> (+8) + (-8) + (+6) + (-9) + (+9) + (-6) = 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<style type="text/css">.spoilerbutton {display:block; border:dashed; padding: 0px 0px; margin:10px 0px; font-size:150%; font-weight: bold; color:#000000; background-color:cyan; outline:0;
}
.spoiler {overflow:hidden;}
.spoiler > div {-webkit-transition: all 0s ease;-moz-transition: margin 0s ease;-o-transition: margin 0s ease;transition: margin 0s ease;}
.spoilerbutton[value="Show Message"] + .spoiler > div {margin-top:-500%;}
.spoilerbutton[value="Hide Message"] + .spoiler {padding:5px;}
</style>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Luân phiên các dấu bắt đầu từ chữ số có hàng cao nhất. Số chữ số không nhiều: chuyển số thành chuỗi thập phân và gán trọng số cho mỗi chỉ số dựa trên tính chẵn lẻ của nó.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quá trình như mô tả trong đề bài.

Ta đặt ký hiệu ban đầu là $sign=1$. Bắt đầu từ chữ số có hàng cao nhất, mỗi lần ta lấy một chữ số $x$, nhân chữ số đó với $sign$, cộng kết quả vào đáp án, sau đó đổi dấu $sign$ và tiếp tục xử lý chữ số tiếp theo cho đến khi xử lý hết các chữ số.

Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số được cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def alternateDigitSum(self, n: int) -> int:
        return sum((-1) ** i * int(x) for i, x in enumerate(str(n)))
```

#### Java

```java
class Solution {
    public int alternateDigitSum(int n) {
        int ans = 0, sign = 1;
        for (char c : String.valueOf(n).toCharArray()) {
            int x = c - '0';
            ans += sign * x;
            sign *= -1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int alternateDigitSum(int n) {
        int ans = 0, sign = 1;
        for (char c : to_string(n)) {
            int x = c - '0';
            ans += sign * x;
            sign *= -1;
        }
        return ans;
    }
};
```

#### Go

```go
func alternateDigitSum(n int) (ans int) {
	sign := 1
	for _, c := range strconv.Itoa(n) {
		x := int(c - '0')
		ans += sign * x
		sign *= -1
	}
	return
}
```

#### TypeScript

```ts
function alternateDigitSum(n: number): number {
    let ans = 0;
    let sign = 1;
    while (n) {
        ans += (n % 10) * sign;
        sign = -sign;
        n = Math.floor(n / 10);
    }
    return ans * -sign;
}
```

#### Rust

```rust
impl Solution {
    pub fn alternate_digit_sum(mut n: i32) -> i32 {
        let mut ans = 0;
        let mut sign = 1;
        while n != 0 {
            ans += (n % 10) * sign;
            sign = -sign;
            n /= 10;
        }
        ans * -sign
    }
}
```

#### C

```c
int alternateDigitSum(int n) {
    int ans = 0;
    int sign = 1;
    while (n) {
        ans += (n % 10) * sign;
        sign = -sign;
        n /= 10;
    }
    return ans * -sign;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 xây dựng dấu theo $(-1)^i$. Duy trì một biến $\textit{sign}$ và đổi giữa $+1$ và $-1$ giúp tránh phép lũy thừa mà vẫn cho cùng một tổng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def alternateDigitSum(self, n: int) -> int:
        ans, sign = 0, 1
        for c in str(n):
            x = int(c)
            ans += sign * x
            sign *= -1
        return ans
```

#### Rust

```rust
impl Solution {
    pub fn alternate_digit_sum(n: i32) -> i32 {
        let mut ans = 0;
        let mut sign = 1;

        for c in format!("{}", n).chars() {
            let x = c.to_digit(10).unwrap() as i32;
            ans += x * sign;
            sign *= -1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
