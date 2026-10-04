---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Interactive
---

<!-- problem:start -->

# [3064. Guess the Number Using Bitwise Questions I 🔒](https://leetcode.com/problems/guess-the-number-using-bitwise-questions-i)

[中文文档](/solution/3000-3099/3064.Guess%20the%20Number%20Using%20Bitwise%20Questions%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số <code>n</code> mà bạn cần tìm.</p>

<p>Ngoài ra còn có một API được định nghĩa sẵn <code>int commonSetBits(int num)</code>, trả về số bit mà cả <code>n</code> và <code>num</code> đều bằng <code>1</code> tại vị trí đó trong biểu diễn nhị phân của chúng. Nói cách khác, API trả về số <span data-keyword="set-bit">bit được bật</span> trong <code>n &amp; num</code>, trong đó <code>&amp;</code> là toán tử <code>AND</code> theo bit.</p>

<p>Hãy trả về <em>số</em> <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> n = 31 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 31 </span></p>

<p><strong>Giải thích: </strong> Có thể chứng minh rằng ta có thể tìm được <code>31</code> bằng API đã cho.</p>
</div>

<p><strong class="example">Ví dụ 2: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> n = 33 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 33 </span></p>

<p><strong>Giải thích: </strong> Có thể chứng minh rằng ta có thể tìm được <code>33</code> bằng API đã cho.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 2<sup>30</sup> - 1</code></li>
    <li><code>0 &lt;= num &lt;= 2<sup>30</sup> - 1</code></li>
    <li>Nếu bạn yêu cầu một <code>num</code> nằm ngoài phạm vi đã cho, kết quả trả về sẽ không đáng tin cậy.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> $\texttt{commonSetBits}(x)$ là số bit $1$ mà $n$ và $x$ cùng có. $n < 2^{30}$, vì vậy ta có thể truy vấn từng bit.
>
> Với $x=2^i$, kết quả khác không khi bit $i$ của $n$ được bật.
>
> Ta thử $32$ lũy thừa của hai và dùng phép OR để đặt mọi bit có kết quả đúng.

<!-- thinking:end -->

Ta có thể liệt kê các lũy thừa của 2, sau đó gọi phương thức `commonSetBits`. Nếu giá trị trả về lớn hơn 0, điều đó có nghĩa là bit tương ứng trong biểu diễn nhị phân của `n` bằng 1.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n \le 2^{30}$ trong bài này. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition of commonSetBits API.
# def commonSetBits(num: int) -> int:


class Solution:
    def findNumber(self) -> int:
        return sum(1 << i for i in range(32) if commonSetBits(1 << i))
```

#### Java

```java
/**
 * Definition of commonSetBits API (defined in the parent class Problem).
 * int commonSetBits(int num);
 */

public class Solution extends Problem {
    public int findNumber() {
        int n = 0;
        for (int i = 0; i < 32; ++i) {
            if (commonSetBits(1 << i) > 0) {
                n |= 1 << i;
            }
        }
        return n;
    }
}
```

#### C++

```cpp
/**
 * Definition of commonSetBits API.
 * int commonSetBits(int num);
 */

class Solution {
public:
    int findNumber() {
        int n = 0;
        for (int i = 0; i < 32; ++i) {
            if (commonSetBits(1 << i)) {
                n |= 1 << i;
            }
        }
        return n;
    }
};
```

#### Go

```go
/**
 * Definition of commonSetBits API.
 * func commonSetBits(num int) int;
 */

func findNumber() (n int) {
    for i := 0; i < 32; i++ {
        if commonSetBits(1<<i) > 0 {
            n |= 1 << i
        }
    }
    return
}
```

#### TypeScript

```ts
/**
 * Definition of commonSetBits API.
 * var commonSetBits = function(num: number): number {}
 */

function findNumber(): number {
    let n = 0;
    for (let i = 0; i < 32; ++i) {
        if (commonSetBits(1 << i)) {
            n |= 1 << i;
        }
    }
    return n;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
