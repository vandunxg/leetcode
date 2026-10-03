---
comments: true
difficulty: Easy
rating: 1206
source: Weekly Contest 231 Q1
tags:
    - String
---

<!-- problem:start -->

# [1784. Check if Binary String Has at Most One Segment of Ones](https://leetcode.com/problems/check-if-binary-string-has-at-most-one-segment-of-ones)

[中文文档](/solution/1700-1799/1784.Check%20if%20Binary%20String%20Has%20at%20Most%20One%20Segment%20of%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi nhị phân <code>s</code> <strong>​​​​​không có các số 0 ở đầu</strong>, trả về <code>true</code>​​​ <em>nếu </em><code>s</code><em> chứa <strong>nhiều nhất một đoạn liên tiếp gồm các số 1</strong></em>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1001&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích: </strong>Chuỗi có hai đoạn, mỗi đoạn có độ dài 1.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;110&quot;
<strong>Đầu ra:</strong> true</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s[i]</code>​​​​ is either <code>&#39;0&#39;</code> or <code>&#39;1&#39;</code>.</li>
	<li><code>s[0]</code> is&nbsp;<code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố mẹo

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi bắt đầu bằng $1$ và không có số 0 ở đầu. Đoạn số 1 thứ hai xuất hiện khi và chỉ khi một $0$ được theo sau bởi $1$, tức là chuỗi con $01$ xuất hiện.
>
> Chỉ cần kiểm tra $01$, không cần đếm các đoạn.

<!-- thinking:end -->

Vì chuỗi $s$ không có số 0 ở đầu nên $s$ bắt đầu bằng `'1'`.

Nếu chuỗi $s$ chứa chuỗi con `"01"`, thì $s$ có dạng `"1...01..."`, nghĩa là $s$ có ít nhất hai đoạn các ký tự `'1'` liên tiếp, vi phạm điều kiện, nên trả về $\textit{false}$.

Nếu chuỗi $s$ không chứa chuỗi con `"01"`, thì $s$ chỉ có thể có dạng `"1..1000..."`, nghĩa là $s$ có đúng một đoạn các ký tự `'1'` liên tiếp, nên trả về $\textit{true}$.

Do đó, ta chỉ cần kiểm tra chuỗi $s$ có chứa chuỗi con `"01"` hay không.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkOnesSegment(self, s: str) -> bool:
        return '01' not in s
```

#### Java

```java
class Solution {
    public boolean checkOnesSegment(String s) {
        return !s.contains("01");
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkOnesSegment(string s) {
        return s.find("01") == -1;
    }
};
```

#### Go

```go
func checkOnesSegment(s string) bool {
	return !strings.Contains(s, "01")
}
```

#### TypeScript

```ts
function checkOnesSegment(s: string): boolean {
    return !s.includes('01');
}
```

#### Rust

```rust
impl Solution {
    pub fn check_ones_segment(s: String) -> bool {
        !s.contains("01")
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var checkOnesSegment = function (s) {
    return !s.includes('01');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
