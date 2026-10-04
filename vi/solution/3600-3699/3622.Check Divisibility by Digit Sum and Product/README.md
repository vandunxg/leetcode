---
comments: true
difficulty: Easy
rating: 1148
source: Weekly Contest 459 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3622. Check Divisibility by Digit Sum and Product](https://leetcode.com/problems/check-divisibility-by-digit-sum-and-product)

[中文文档](/solution/3600-3699/3622.Check%20Divisibility%20by%20Digit%20Sum%20and%20Product/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>. Hãy xác định liệu <code>n</code> có chia hết cho <strong>tổng</strong> của hai giá trị sau hay không:</p>

<ul>
    <li>
    <p><strong>Tổng các chữ số</strong> của <code>n</code> (tổng các chữ số của nó).</p>
    </li>
    <li>
    <p><strong>Tích</strong> các <strong>chữ số</strong> của <code>n</code> (tích các chữ số của nó).</p>
    </li>
</ul>

<p>Trả về <code>true</code> nếu <code>n</code> chia hết cho tổng này; ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 99</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì 99 chia hết cho tổng (9 + 9 = 18) cộng với tích (9 * 9 = 81) của các chữ số (tổng cộng là 99), kết quả là true.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 23</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì 23 không chia hết cho tổng (2 + 3 = 5) cộng với tích (2 * 3 = 6) của các chữ số (tổng cộng là 11), kết quả là false.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 10^6$, chỉ cần tách từng chữ số rồi tính tổng và tích của chúng. $\textit{divmod}$ lấy ra chữ số ở cuối mà không cần chuyển số sang chuỗi.
>
> Tích được khởi tạo bằng $1$, không phải $0$. Kiểm tra xem $n$ có chia hết cho $s+p$ không. Có $O(\log n)$ chữ số.

<!-- thinking:end -->

Ta có thể duyệt qua từng chữ số của số nguyên $n$, đồng thời tính tổng chữ số $s$ và tích chữ số $p$. Cuối cùng, kiểm tra xem $n$ có chia hết cho $s + p$ hay không.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là giá trị của số nguyên $n$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkDivisibility(self, n: int) -> bool:
        s, p = 0, 1
        x = n
        while x:
            x, v = divmod(x, 10)
            s += v
            p *= v
        return n % (s + p) == 0
```

#### Java

```java
class Solution {
    public boolean checkDivisibility(int n) {
        int s = 0, p = 1;
        int x = n;
        while (x != 0) {
            int v = x % 10;
            x /= 10;
            s += v;
            p *= v;
        }
        return n % (s + p) == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkDivisibility(int n) {
        int s = 0, p = 1;
        int x = n;
        while (x != 0) {
            int v = x % 10;
            x /= 10;
            s += v;
            p *= v;
        }
        return n % (s + p) == 0;
    }
};
```

#### Go

```go
func checkDivisibility(n int) bool {
    s, p := 0, 1
    x := n
    for x != 0 {
        v := x % 10
        x /= 10
        s += v
        p *= v
    }
    return n%(s+p) == 0
}
```

#### TypeScript

```ts
function checkDivisibility(n: number): boolean {
    let [s, p] = [0, 1];
    let x = n;
    while (x !== 0) {
        const v = x % 10;
        x = Math.floor(x / 10);
        s += v;
        p *= v;
    }
    return n % (s + p) === 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
