---
comments: true
difficulty: Easy
rating: 1294
source: Biweekly Contest 43 Q1
tags:
    - Math
---

<!-- problem:start -->

# [1716. Calculate Money in Leetcode Bank](https://leetcode.com/problems/calculate-money-in-leetcode-bank)

[中文文档](/solution/1700-1799/1716.Calculate%20Money%20in%20Leetcode%20Bank/README.md)

## Mô tả

<!-- description:start -->

<p>Hercy muốn tiết kiệm tiền để mua chiếc xe đầu tiên. Cậu gửi tiền vào ngân hàng Leetcode&nbsp;<strong>mỗi ngày</strong>.</p>

<p>Ngày đầu tiên là thứ Hai, cậu gửi <code>$1</code>. Mỗi ngày từ thứ Ba đến Chủ nhật, cậu gửi nhiều hơn ngày trước <code>$1</code>. Vào mỗi thứ Hai tiếp theo, cậu gửi nhiều hơn <strong>thứ Hai trước đó</strong> <code>$1</code>.<span style="display: none;"> </span></p>

<p>Cho <code>n</code>, hãy trả về <em>tổng số tiền cậu có trong ngân hàng Leetcode vào cuối ngày thứ </em><code>n<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> n = 4
<strong>Output:</strong> 10
<strong>Explanation:</strong>&nbsp;Sau ngày thứ 4<sup>th</sup>, tổng tiền là 1 + 2 + 3 + 4 = 10.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 10
<strong>Output:</strong> 37
<strong>Explanation:</strong>&nbsp;Sau ngày thứ 10<sup>th</sup>, tổng tiền là (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4) = 37. Lưu ý rằng vào thứ Hai của tuần thứ 2<sup>nd</sup>, Hercy chỉ gửi $2.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> n = 20
<strong>Output:</strong> 96
<strong>Explanation:</strong>&nbsp;Sau ngày thứ 20<sup>th</sup>, tổng tiền là (1 + 2 + 3 + 4 + 5 + 6 + 7) + (2 + 3 + 4 + 5 + 6 + 7 + 8) + (3 + 4 + 5 + 6 + 7 + 8) = 96.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Math

<!-- thinking:start -->

> **Tư duy**
>
> Tuần $k$ gửi mỗi ngày nhiều hơn tuần trước một đơn vị. Các tuần đầy đủ và số ngày còn lại đều tạo thành cấp số cộng. Với $n\le 1000$ có thể dùng vòng lặp, nhưng công thức đóng cho kết quả ngay.
>
> Có $k=\lfloor n/7\rfloor$ tuần đầy đủ, bắt đầu với tổng $28$ và công sai $7$, cùng $b=n\bmod 7$ ngày còn lại bắt đầu từ $k+1$.
>
> Cộng hai cấp số cộng này.

<!-- thinking:end -->

Theo mô tả đề bài, số tiền gửi trong mỗi tuần như sau:

```bash
Week 1: 1, 2, 3, 4, 5, 6, 7
Week 2: 2, 3, 4, 5, 6, 7, 8
Week 3: 3, 4, 5, 6, 7, 8, 9
...
Week k: k, k+1, k+2, k+3, k+4, k+5, k+6
```

Với $n$ ngày gửi tiền, số tuần đầy đủ là $k = \lfloor n / 7 \rfloor$, còn số ngày dư là $b = n \mod 7$.

Tổng tiền gửi trong $k$ tuần đầy đủ được tính bằng công thức tổng cấp số cộng:

$$
S_1 = \frac{k}{2} \times (28 + 28 + 7 \times (k - 1))
$$

Tổng tiền gửi trong $b$ ngày còn lại cũng được tính bằng công thức tổng cấp số cộng:

$$
S_2 = \frac{b}{2} \times (k + 1 + k + 1 + b - 1)
$$

Tổng tiền gửi cuối cùng là $S = S_1 + S_2$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalMoney(self, n: int) -> int:
        k, b = divmod(n, 7)
        s1 = (28 + 28 + 7 * (k - 1)) * k // 2
        s2 = (k + 1 + k + 1 + b - 1) * b // 2
        return s1 + s2
```

#### Java

```java
class Solution {
    public int totalMoney(int n) {
        int k = n / 7, b = n % 7;
        int s1 = (28 + 28 + 7 * (k - 1)) * k / 2;
        int s2 = (k + 1 + k + 1 + b - 1) * b / 2;
        return s1 + s2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalMoney(int n) {
        int k = n / 7, b = n % 7;
        int s1 = (28 + 28 + 7 * (k - 1)) * k / 2;
        int s2 = (k + 1 + k + 1 + b - 1) * b / 2;
        return s1 + s2;
    }
};
```

#### Go

```go
func totalMoney(n int) int {
	k, b := n/7, n%7
	s1 := (28 + 28 + 7*(k-1)) * k / 2
	s2 := (k + 1 + k + 1 + b - 1) * b / 2
	return s1 + s2
}
```

#### TypeScript

```ts
function totalMoney(n: number): number {
    const k = (n / 7) | 0;
    const b = n % 7;
    const s1 = (((28 + 28 + 7 * (k - 1)) * k) / 2) | 0;
    const s2 = (((k + 1 + k + 1 + b - 1) * b) / 2) | 0;
    return s1 + s2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
