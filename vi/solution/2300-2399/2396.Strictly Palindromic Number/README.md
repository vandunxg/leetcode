---
comments: true
difficulty: Medium
rating: 1328
source: Biweekly Contest 86 Q2
tags:
    - Brainteaser
    - Math
    - Two Pointers
---

<!-- problem:start -->

# [2396. Strictly Palindromic Number](https://leetcode.com/problems/strictly-palindromic-number)

[中文文档](/solution/2300-2399/2396.Strictly%20Palindromic%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên <code>n</code> được gọi là <strong>đối xứng nghiêm ngặt</strong> nếu, với <strong>mọi</strong> cơ số <code>b</code> từ <code>2</code> đến <code>n - 2</code> (<strong>bao gồm cả hai đầu</strong>), biểu diễn của số nguyên <code>n</code> trong cơ số <code>b</code> là <strong>đối xứng</strong>.</p>

<p>Cho một số nguyên <code>n</code>, trả về <code>true</code> <em>nếu </em><code>n</code><em> là <strong>đối xứng nghiêm ngặt</strong> và </em><code>false</code><em> nếu không</em>.</p>

<p>Một chuỗi là <strong>đối xứng</strong> nếu đọc xuôi hay ngược đều giống nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 9
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Trong cơ số 2: 9 = 1001 (cơ số 2), là một chuỗi đối xứng.
Trong cơ số 3: 9 = 100 (cơ số 3), không phải là chuỗi đối xứng.
Do đó, 9 không đối xứng nghiêm ngặt, nên ta trả về false.
Lưu ý rằng trong các cơ số 4, 5, 6 và 7, n = 9 cũng không đối xứng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Ta chỉ xét cơ số 2: 4 = 100 (cơ số 2), không phải là chuỗi đối xứng.
Do đó, ta trả về false.

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>4 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tư duy nhanh

<!-- thinking:start -->

> **Tư duy**
>
> $n$ phải là số đối xứng trong mọi cơ số từ $2$ đến $n-2$. Vì $n \ge 4$, ta chỉ cần kiểm tra một số cơ số cụ thể thay vì chuyển đổi sang tất cả các cơ số.
>
> Với $n=4$, biểu diễn trong cơ số 2 là $100$; với $n>4$, biểu diễn trong cơ số $n-2$ là $12$. Cả hai đều không phải số đối xứng, nên đáp án luôn là false.

<!-- thinking:end -->

Khi $n = 4$, biểu diễn nhị phân của nó là $100$, không phải là số đối xứng;

Khi $n \gt 4$, biểu diễn của nó trong cơ số $(n - 2)$ là $12$, không phải là số đối xứng.

Do đó, ta có thể trực tiếp trả về `false`.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isStrictlyPalindromic(self, n: int) -> bool:
        return False
```

#### Java

```java
class Solution {
    public boolean isStrictlyPalindromic(int n) {
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isStrictlyPalindromic(int n) {
        return false;
    }
};
```

#### Go

```go
func isStrictlyPalindromic(n int) bool {
	return false
}
```

#### TypeScript

```ts
function isStrictlyPalindromic(n: number): boolean {
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_strictly_palindromic(n: i32) -> bool {
        false
    }
}
```

#### C

```c
bool isStrictlyPalindromic(int n) {
    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
