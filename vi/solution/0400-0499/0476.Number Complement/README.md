---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [476. Number Complement](https://leetcode.com/problems/number-complement)

[中文文档](/solution/0400-0499/0476.Number%20Complement/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Phần bù</strong> của một số nguyên là số nhận được khi đảo tất cả bit <code>0</code> thành <code>1</code> và tất cả bit <code>1</code> thành <code>0</code> trong biểu diễn nhị phân của nó.</p>

<ul>
	<li>Ví dụ, số nguyên <code>5</code> có biểu diễn nhị phân là <code>&quot;101&quot;</code>; <strong>phần bù</strong> của nó là <code>&quot;010&quot;</code>, tương ứng với số nguyên <code>2</code>.</li>
</ul>

<p>Cho số nguyên <code>num</code>, hãy trả về <em>phần bù của nó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Biểu diễn nhị phân của 5 là 101 (không có bit 0 ở đầu), phần bù của nó là 010. Vì vậy, kết quả cần trả về là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Biểu diễn nhị phân của 1 là 1 (không có bit 0 ở đầu), phần bù của nó là 0. Vì vậy, kết quả cần trả về là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt; 2<sup>31</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống bài 1009: <a href="https://leetcode.com/problems/complement-of-base-10-integer/" target="_blank">https://leetcode.com/problems/complement-of-base-10-integer/</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Phần bù đảo các bit trong biểu diễn nhị phân, không tính các số 0 ở đầu. Nếu dùng phép NOT bitwise trực tiếp, các bit 0 ở phía cao cũng sẽ bị đổi thành 1.
>
> XOR với mask $2^{\textit{bit\_length}}-1$ chỉ đảo các bit có nghĩa.
>
> Độ rộng của mask vừa khít với bit $1$ cao nhất, nên không phát sinh các bit 0 ở đầu.

<!-- thinking:end -->

Theo đề bài, ta có thể dùng phép XOR để đảo bit. Các bước như sau:

Trước tiên, tìm vị trí bit $1$ cao nhất trong biểu diễn nhị phân của $\textit{num}$, ký hiệu vị trí đó là $k$.

Tiếp theo, tạo một số nhị phân có bit thứ $k$ bằng $0$ và các bit thấp hơn còn lại bằng $1$, tức là $2^k - 1$;

Cuối cùng, XOR $\textit{num}$ với số nhị phân vừa tạo để nhận đáp án.

Độ phức tạp thời gian là $O(\log \textit{num})$, trong đó $\textit{num}$ là số nguyên đầu vào. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findComplement(self, num: int) -> int:
        return num ^ ((1 << num.bit_length()) - 1)
```

#### Java

```java
class Solution {
    public int findComplement(int num) {
        return num ^ ((1 << (32 - Integer.numberOfLeadingZeros(num))) - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findComplement(int num) {
        return num ^ ((1LL << (64 - __builtin_clzll(num))) - 1);
    }
};
```

#### Go

```go
func findComplement(num int) int {
	return num ^ ((1 << bits.Len(uint(num))) - 1)
}
```

#### TypeScript

```ts
function findComplement(num: number): number {
    return num ^ (2 ** num.toString(2).length - 1);
}
```

#### JavaScript

```js
/**
 * @param {number} num
 * @return {number}
 */
var findComplement = function (num) {
    return num ^ (2 ** num.toString(2).length - 1);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit. Đảo bit + AND

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng XOR. Thực hiện NOT bitwise rồi AND với cùng mask có các bit thấp bằng 1 sẽ giữ nguyên độ rộng bit. Cách viết khác nhau nhưng cùng ý nghĩa.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function findComplement(num: number): number {
    return ~num & (2 ** num.toString(2).length - 1);
}
```

#### JavaScript

```js
/**
 * @param {number} num
 * @return {number}
 */
function findComplement(num) {
    return ~num & (2 ** num.toString(2).length - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
