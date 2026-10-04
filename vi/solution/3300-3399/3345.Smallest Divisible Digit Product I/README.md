---
comments: true
difficulty: Easy
rating: 1235
source: Biweekly Contest 143 Q1
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [3345. Smallest Divisible Digit Product I](https://leetcode.com/problems/smallest-divisible-digit-product-i)

[中文文档](/solution/3300-3399/3345.Smallest%20Divisible%20Digit%20Product%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>n</code> và <code>t</code>. Hãy trả về <strong>số nhỏ nhất</strong> lớn hơn hoặc bằng <code>n</code> sao cho <strong>tích các chữ số</strong> của nó chia hết cho <code>t</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10, t = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tích các chữ số của 10 là 0, chia hết cho 2, nên 10 là số nhỏ nhất lớn hơn hoặc bằng 10 thỏa mãn điều kiện.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 15, t = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tích các chữ số của 16 là 6, chia hết cho 3, nên 16 là số nhỏ nhất lớn hơn hoặc bằng 15 thỏa mãn điều kiện.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 100</code></li>
    <li><code>1 &lt;= t &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số nguyên nhỏ nhất không nhỏ hơn $n$ có tích các chữ số chia hết cho $t$. Vì $n \le 100$ và $t \le 10$, trong mười ứng viên liên tiếp luôn có một số chứa chữ số $0$, mà tích các chữ số của số đó luôn là $0$ và luôn chia hết cho $t$.
>
> Do đó, việc duyệt tuyến tính từ $n$ được giới hạn bởi một hằng số; không cần phải tự xây dựng một số đặc biệt.
>
> Với mỗi ứng viên, ta nhân các chữ số rồi trả về $p$ đầu tiên thỏa mãn $p \bmod t = 0$.

<!-- thinking:end -->

Ta nhận thấy cứ mỗi $10$ số thì chắc chắn có một số nguyên có tích các chữ số bằng $0$. Vì vậy, ta có thể duyệt trực tiếp các số nguyên lớn hơn hoặc bằng $n$ cho đến khi tìm được một số có tích các chữ số chia hết cho $t$.

Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestNumber(self, n: int, t: int) -> int:
        for i in count(n):
            p = 1
            x = i
            while x:
                p *= x % 10
                x //= 10
            if p % t == 0:
                return i
```

#### Java

```java
class Solution {
    public int smallestNumber(int n, int t) {
        for (int i = n;; ++i) {
            int p = 1;
            for (int x = i; x > 0; x /= 10) {
                p *= (x % 10);
            }
            if (p % t == 0) {
                return i;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestNumber(int n, int t) {
        for (int i = n;; ++i) {
            int p = 1;
            for (int x = i; x > 0; x /= 10) {
                p *= (x % 10);
            }
            if (p % t == 0) {
                return i;
            }
        }
    }
};
```

#### Go

```go
func smallestNumber(n int, t int) int {
    for i := n; ; i++ {
        p := 1
        for x := i; x > 0; x /= 10 {
            p *= x % 10
        }
        if p%t == 0 {
            return i
        }
    }
}
```

#### TypeScript

```ts
function smallestNumber(n: number, t: number): number {
    for (let i = n; ; ++i) {
        let p = 1;
        for (let x = i; x; x = Math.floor(x / 10)) {
            p *= x % 10;
        }
        if (p % t === 0) {
            return i;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
