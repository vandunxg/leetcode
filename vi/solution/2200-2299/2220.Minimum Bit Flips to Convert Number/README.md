---
comments: true
difficulty: Easy
rating: 1282
source: Biweekly Contest 75 Q1
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [2220. Minimum Bit Flips to Convert Number](https://leetcode.com/problems/minimum-bit-flips-to-convert-number)

[中文文档](/solution/2200-2299/2220.Minimum%20Bit%20Flips%20to%20Convert%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>lần lật bit</strong> của số <code>x</code> là chọn một bit trong biểu diễn nhị phân của <code>x</code> và <strong>lật</strong> nó từ <code>0</code> thành <code>1</code> hoặc từ <code>1</code> thành <code>0</code>.</p>

<ul>
	<li>Ví dụ, với <code>x = 7</code>, biểu diễn nhị phân là <code>111</code>, và ta có thể chọn bất kỳ bit nào (bao gồm cả các số 0 đứng đầu không được hiển thị) để lật. Ta có thể lật bit đầu tiên tính từ bên phải để được <code>110</code>, lật bit thứ hai tính từ bên phải để được <code>101</code>, lật bit thứ năm tính từ bên phải (một số 0 đứng đầu) để được <code>10111</code>, v.v.</li>
</ul>

<p>Cho hai số nguyên <code>start</code> và <code>goal</code>, hãy trả về <em>số <strong>lần lật bit</strong> <strong>ít nhất</strong> để chuyển </em><code>start</code><em> thành </em><code>goal</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = 10, goal = 7
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Biểu diễn nhị phân của 10 và 7 lần lượt là 1010 và 0111. Ta có thể chuyển 10 thành 7 trong 3 bước:
- Lật bit đầu tiên tính từ bên phải: 101<u>0</u> -&gt; 101<u>1</u>.
- Lật bit thứ ba tính từ bên phải: 1<u>0</u>11 -&gt; 1<u>1</u>11.
- Lật bit thứ tư tính từ bên phải: <u>1</u>111 -&gt; <u>0</u>111.
Có thể chứng minh rằng không thể chuyển 10 thành 7 trong ít hơn 3 bước. Do đó, ta trả về 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = 3, goal = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Biểu diễn nhị phân của 3 và 4 lần lượt là 011 và 100. Ta có thể chuyển 3 thành 4 trong 3 bước:
- Lật bit đầu tiên tính từ bên phải: 01<u>1</u> -&gt; 01<u>0</u>.
- Lật bit thứ hai tính từ bên phải: 0<u>1</u>0 -&gt; 0<u>0</u>0.
- Lật bit thứ ba tính từ bên phải: <u>0</u>00 -&gt; <u>1</u>00.
Có thể chứng minh rằng không thể chuyển 3 thành 4 trong ít hơn 3 bước. Do đó, ta trả về 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= start, goal &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài toán này giống với <a href="https://leetcode.com/problems/hamming-distance/description/" target="_blank">461: Hamming Distance.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần lật sẽ thay đổi một bit của $start$ để nó trở thành $goal$. Các số nguyên chỉ có khoảng $30$ bit, nhưng các bit độc lập với nhau: chỉ cần lật những vị trí mà $start$ và $goal$ khác nhau.
>
> $start \oplus goal$ có giá trị $1$ đúng tại những bit đó; số lượng bit 1 là số lần lật ít nhất.

<!-- thinking:end -->

Theo mô tả bài toán, ta chỉ cần đếm số bit 1 trong biểu diễn nhị phân của $\textit{start} \oplus \textit{goal}$.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là kích thước của các số nguyên trong bài toán. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minBitFlips(self, start: int, goal: int) -> int:
        return (start ^ goal).bit_count()
```

#### Java

```java
class Solution {
    public int minBitFlips(int start, int goal) {
        return Integer.bitCount(start ^ goal);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minBitFlips(int start, int goal) {
        return __builtin_popcount(start ^ goal);
    }
};
```

#### Go

```go
func minBitFlips(start int, goal int) int {
	return bits.OnesCount(uint(start ^ goal))
}
```

#### TypeScript

```ts
function minBitFlips(start: number, goal: number): number {
    return bitCount(start ^ goal);
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
    pub fn min_bit_flips(start: i32, goal: i32) -> i32 {
        (start ^ goal).count_ones() as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {number} start
 * @param {number} goal
 * @return {number}
 */
var minBitFlips = function (start, goal) {
    return bitCount(start ^ goal);
};

function bitCount(i) {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

#### C

```c
int minBitFlips(int start, int goal) {
    int x = start ^ goal;
    int ans = 0;
    while (x) {
        ans += (x & 1);
        x >>= 1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
