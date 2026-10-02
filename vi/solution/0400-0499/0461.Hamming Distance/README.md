---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [461. Hamming Distance](https://leetcode.com/problems/hamming-distance)

[中文文档](/solution/0400-0499/0461.Hamming%20Distance/README.md)

## Mô tả

<!-- description:start -->

<p><a href="https://en.wikipedia.org/wiki/Hamming_distance" target="_blank">Hamming distance</a> giữa hai số nguyên là số vị trí mà các bit tương ứng khác nhau.</p>

<p>Cho hai số nguyên <code>x</code> và <code>y</code>, hãy trả về <em><strong>Hamming distance</strong> giữa chúng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 1, y = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
1   (0 0 0 1)
4   (0 1 0 0)
       &uarr;   &uarr;
Các mũi tên phía trên chỉ những vị trí có bit tương ứng khác nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 3, y = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;=&nbsp;x, y &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống với <a href="https://leetcode.com/problems/minimum-bit-flips-to-convert-number/description/" target="_blank"> 2220: Minimum Bit Flips to Convert Number.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hamming distance là số bit khác nhau. So sánh từng bit cần một vòng lặp $32$ lượt.
>
> Các bit $1$ trong $x\oplus y$ chính là những vị trí đó; $\textit{bit\_count}$ trả về số lượng bit này.
>
> Phép XOR gộp việc so sánh vào một phép toán trên word; sau đó popcount được tính bằng lệnh phần cứng hoặc một vòng lặp ngắn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hammingDistance(self, x: int, y: int) -> int:
        return (x ^ y).bit_count()
```

#### Java

```java
class Solution {
    public int hammingDistance(int x, int y) {
        return Integer.bitCount(x ^ y);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        return __builtin_popcount(x ^ y);
    }
};
```

#### Go

```go
func hammingDistance(x int, y int) int {
	return bits.OnesCount(uint(x ^ y))
}
```

#### TypeScript

```ts
function hammingDistance(x: number, y: number): number {
    x ^= y;
    let ans = 0;
    while (x) {
        x -= x & -x;
        ++ans;
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number} x
 * @param {number} y
 * @return {number}
 */
var hammingDistance = function (x, y) {
    x ^= y;
    let ans = 0;
    while (x) {
        x -= x & -x;
        ++ans;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
