---
comments: true
difficulty: Easy
rating: 1182
source: Weekly Contest 506 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [3959. Check Good Integer](https://leetcode.com/problems/check-good-integer)

[中文文档](/solution/3900-3999/3959.Check%20Good%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code>.</p>

<p>Gọi <code>digitSum</code> là tổng các chữ số của <code>n</code>, và <code>squareSum</code> là tổng bình phương các chữ số của <code>n</code>.</p>

<p>Một số nguyên được gọi là <strong>tốt</strong> nếu <code>squareSum - digitSum &gt;= 50</code>.</p>

<p>Trả về <code>true</code> nếu <code>n</code> là số tốt. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1000</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các chữ số của 1000 là 1, 0, 0 và 0.</li>
	<li><code>digitSum</code> là <code>1 + 0 + 0 + 0 = 1</code>.</li>
	<li><code>squareSum</code> là <code>1<sup>2</sup> + 0<sup>2</sup> + 0<sup>2</sup> + 0<sup>2</sup> = 1</code>.</li>
	<li><code>squareSum - digitSum</code> là <code>1 - 1 = 0</code>. Vì 0 không lớn hơn hoặc bằng 50, đầu ra là <code>false</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 19</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các chữ số của 19 là 1 và 9.</li>
	<li><code>digitSum</code> là <code>1 + 9 = 10</code>.</li>
	<li><code>squareSum</code> là <code>1<sup>2</sup> + 9<sup>2</sup> = 1 + 81 = 82</code>.</li>
	<li><code>squareSum - digitSum</code> là <code>82 - 10 = 72</code>. Vì 72 lớn hơn hoặc bằng 50, đầu ra là <code>true</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một số nguyên tốt là số mà các chữ số $d$ đóng góp $d(d-1)$, và tổng các đóng góp ít nhất là $50$. Tách từng chữ số và cộng $x(x-1)$, sau đó so sánh với $50$.
>
> Vì $n\le 10^9$ có ít chữ số, chỉ cần một vòng lặp.

<!-- thinking:end -->

Ta dùng một biến $s$ để ghi nhận hiệu giữa tổng bình phương và tổng các chữ số của $n$. Nếu $s$ lớn hơn hoặc bằng 50, ta trả về $\textit{true}$; ngược lại, ta trả về $\textit{false}$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $\log n$ là số chữ số của $n$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkGoodInteger(self, n: int) -> bool:
        s = 0
        while n:
            n, x = divmod(n, 10)
            s += x * (x - 1)
        return s >= 50
```

#### Java

```java
class Solution {
    public boolean checkGoodInteger(int n) {
        int s = 0;
        for (; n > 0; n /= 10) {
            int x = n % 10;
            s += x * (x - 1);
        }
        return s >= 50;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkGoodInteger(int n) {
        int s = 0;
        for (; n > 0; n /= 10) {
            int x = n % 10;
            s += x * (x - 1);
        }
        return s >= 50;
    }
};
```

#### Go

```go
func checkGoodInteger(n int) bool {
    s := 0
    for ; n > 0; n /= 10 {
        x := n % 10
        s += x * (x - 1)
    }
    return s >= 50
}
```

#### TypeScript

```ts
function checkGoodInteger(n: number): boolean {
    let s: number = 0;
    for (; n; n = Math.floor(n / 10)) {
        const x = n % 10;
        s += x * (x - 1);
    }
    return s >= 50;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
