---
comments: true
difficulty: Easy
rating: 1229
source: Biweekly Contest 173 Q1
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [3794. Reverse String Prefix](https://leetcode.com/problems/reverse-string-prefix)

[Tài liệu tiếng Trung](/solution/3700-3799/3794.Reverse%20String%20Prefix/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> và một số nguyên <code>k</code>.</p>

<p>Đảo ngược <code>k</code> ký tự đầu tiên của <code>s</code> và trả về chuỗi thu được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcd&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;bacd&quot;</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<p><code>k = 2</code> ký tự đầu tiên <code>&quot;ab&quot;</code> được đảo ngược thành <code>&quot;ba&quot;</code>. Chuỗi cuối cùng thu được là <code>&quot;bacd&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;xyz&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;zyx&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>k = 3</code> ký tự đầu tiên <code>&quot;xyz&quot;</code> được đảo ngược thành <code>&quot;zyx&quot;</code>. Chuỗi cuối cùng thu được là <code>&quot;zyx&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;hey&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;hey&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>k = 1</code> ký tự đầu tiên <code>&quot;h&quot;</code> không thay đổi khi đảo ngược. Chuỗi cuối cùng thu được là <code>&quot;hey&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ cần đảo ngược $k$ ký tự đầu tiên, và $k$ không vượt quá độ dài chuỗi. Ta đảo ngược lát cắt tiền tố đó rồi nối với phần hậu tố.

<!-- thinking:end -->

Theo mô tả bài toán, chúng ta đảo ngược $k$ ký tự đầu tiên của chuỗi, sau đó nối chúng với các ký tự còn lại.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reversePrefix(self, s: str, k: int) -> str:
        return s[:k][::-1] + s[k:]
```

#### Java

```java
class Solution {
    public String reversePrefix(String s, int k) {
        StringBuilder sb = new StringBuilder(s.substring(0, k));
        return sb.reverse().toString() + s.substring(k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reversePrefix(string s, int k) {
        string t = s.substr(0, k);
        reverse(t.begin(), t.end());
        return t + s.substr(k);
    }
};
```

#### Go

```go
func reversePrefix(s string, k int) string {
	t := []byte(s[:k])
	slices.Reverse(t)
	return string(t) + s[k:]
}
```

#### TypeScript

```ts
function reversePrefix(s: string, k: number): string {
    return s.slice(0, k).split('').reverse().join('') + s.slice(k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
