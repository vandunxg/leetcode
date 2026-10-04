---
comments: true
difficulty: Easy
rating: 1247
source: Weekly Contest 407 Q1
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [3226. Number of Bit Changes to Make Two Integers Equal](https://leetcode.com/problems/number-of-bit-changes-to-make-two-integers-equal)

[中文文档](/solution/3200-3299/3226.Number%20of%20Bit%20Changes%20to%20Make%20Two%20Integers%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>n</code> và <code>k</code>.</p>

<p>Bạn có thể chọn <strong>bất kỳ</strong> bit nào trong <strong>biểu diễn nhị phân</strong> của <code>n</code> đang bằng 1 và đổi nó thành 0.</p>

<p>Trả về <em>số lần thay đổi</em> cần thực hiện để biến <code>n</code> thành <code>k</code>. Nếu không thể, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 13, k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong><br />
Ban đầu, biểu diễn nhị phân của <code>n</code> và <code>k</code> là <code>n = (1101)<sub>2</sub></code> và <code>k = (0100)<sub>2</sub></code>.<br />
Ta có thể đổi bit thứ nhất và thứ tư của <code>n</code>. Khi đó số nguyên nhận được là <code>n = (<u><strong>0</strong></u>10<u><strong>0</strong></u>)<sub>2</sub> = k</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 21, k = 21</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong><br />
<code>n</code> và <code>k</code> đã bằng nhau, nên không cần thay đổi.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 14, k = 13</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong><br />
Không thể biến <code>n</code> thành <code>k</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n, k &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ có thể đổi các bit $1$ của $n$ thành $0$. Nếu $k$ có bit $1$ tại vị trí mà $n$ là $0$, thì không thể đạt được mục tiêu. Vì $n,k\le 10^6$, chỉ cần duyệt qua các bit là đủ.
>
> Kiểm tra $n\land k=k$; nếu không đúng, trả về $-1$. Các bit cần đổi chính là các bit $1$ của $n\oplus k$, vì vậy popcount của nó là đáp án.

<!-- thinking:end -->

Nếu kết quả AND bit của $n$ và $k$ không bằng $k$, điều đó cho thấy tồn tại ít nhất một bit mà $k$ bằng $1$ còn bit tương ứng trong $n$ bằng $0$. Trong trường hợp này, không thể thay đổi một bit trong $n$ để biến $n$ thành $k$, nên ta trả về $-1$. Ngược lại, ta đếm số bit $1$ trong biểu diễn nhị phân của $n \oplus k$.

Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minChanges(self, n: int, k: int) -> int:
        return -1 if n & k != k else (n ^ k).bit_count()
```

#### Java

```java
class Solution {
    public int minChanges(int n, int k) {
        return (n & k) != k ? -1 : Integer.bitCount(n ^ k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minChanges(int n, int k) {
        return (n & k) != k ? -1 : __builtin_popcount(n ^ k);
    }
};
```

#### Go

```go
func minChanges(n int, k int) int {
    if n&k != k {
        return -1
    }
    return bits.OnesCount(uint(n ^ k))
}
```

#### TypeScript

```ts
function minChanges(n: number, k: number): number {
    return (n & k) !== k ? -1 : bitCount(n ^ k);
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_changes(n: i32, k: i32) -> i32 {
        if (n & k) != k {
            -1
        } else {
            (n ^ k).count_ones() as i32
        }
    }
}
```

#### C#

```cs
public class Solution {
    public int MinChanges(int n, int k) {
        return (n & k) != k ? -1 : BitOperations.PopCount((uint)(n ^ k));
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
