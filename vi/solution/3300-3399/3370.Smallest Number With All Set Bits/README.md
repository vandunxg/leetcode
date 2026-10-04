---
comments: true
difficulty: Easy
rating: 1198
source: Weekly Contest 426 Q1
tags:
    - Bit Manipulation
    - Math
---

<!-- problem:start -->

# [3370. Smallest Number With All Set Bits](https://leetcode.com/problems/smallest-number-with-all-set-bits)

[中文文档](/solution/3300-3399/3370.Smallest%20Number%20With%20All%20Set%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số <em>dương</em> <code>n</code>.</p>

<p>Trả về số <strong>nhỏ nhất</strong> <code>x</code> <strong>lớn hơn</strong> hoặc <strong>bằng</strong> <code>n</code>, sao cho biểu diễn nhị phân của <code>x</code> chỉ chứa <span data-keyword="set-bit">các bit 1</span></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu diễn nhị phân của 7 là <code>&quot;111&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu diễn nhị phân của 15 là <code>&quot;1111&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu diễn nhị phân của 3 là <code>&quot;11&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số nhỏ nhất có dạng $2^p-1$ và không nhỏ hơn $n$. Vì $n \le 1000$, ta dịch trái cho đến khi $2^p>n$.
>
> Bắt đầu với $x=1$ và dịch trái khi $x-1<n$; khi đó $x-1$ sẽ là một dãy toàn bit 1.

<!-- thinking:end -->

Bắt đầu với $x = 1$ và liên tục dịch trái $x$ cho đến khi $x - 1 \geq n$. Khi đó, $x - 1$ chính là đáp án cần tìm.

Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestNumber(self, n: int) -> int:
        x = 1
        while x - 1 < n:
            x <<= 1
        return x - 1
```

#### Java

```java
class Solution {
    public int smallestNumber(int n) {
        int x = 1;
        while (x - 1 < n) {
            x <<= 1;
        }
        return x - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestNumber(int n) {
        int x = 1;
        while (x - 1 < n) {
            x <<= 1;
        }
        return x - 1;
    }
};
```

#### Go

```go
func smallestNumber(n int) int {
	x := 1
	for x-1 < n {
		x <<= 1
	}
	return x - 1
}
```

#### TypeScript

```ts
function smallestNumber(n: number): number {
    let x = 1;
    while (x - 1 < n) {
        x <<= 1;
    }
    return x - 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_number(n: i32) -> i32 {
        let mut x = 1;
        while x - 1 < n {
            x <<= 1;
        }
        x - 1
    }
}
```

#### C#

```cs
public class Solution {
    public int SmallestNumber(int n) {
        int x = 1;
        while (x - 1 < n) {
            x <<= 1;
        }
        return x - 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
