---
comments: true
difficulty: Easy
tags:
    - String
    - String Matching
    - KMP
    - Extended KMP
---

<!-- problem:start -->

# [459. Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern)

[中文文档](/solution/0400-0499/0459.Repeated%20Substring%20Pattern/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy kiểm tra xem có thể tạo chuỗi này bằng cách lấy một chuỗi con rồi ghép nhiều bản sao của nó lại với nhau hay không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abab&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể ghép chuỗi con &quot;ab&quot; hai lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aba&quot;
<strong>Đầu ra:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcabcabcabc&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể ghép chuỗi con &quot;abc&quot; bốn lần hoặc chuỗi con &quot;abcabc&quot; hai lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xác định liệu $s$ có được tạo bằng cách lặp lại một tiền tố ngắn hơn chính nó hay không. Thử mọi độ dài tiền tố sẽ tốn $O(n^2)$.
>
> Tạo $s+s$ rồi tìm $s$ từ chỉ số $1$. Nếu tìm thấy trước vị trí $n$, nghĩa là $s$ khớp bên trong chuỗi ghép, nên chuỗi có chu kỳ.
>
> Bắt đầu tìm từ chỉ số $1$ để bỏ qua kết quả khớp hiển nhiên tại $0$; nếu tìm thấy tại $n$ thì đó chỉ là bản sao ở giữa, nên không có chu kỳ nhỏ hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        return (s + s).index(s, 1) < len(s)
```

#### Java

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        String str = s + s;
        return str.substring(1, str.length() - 1).contains(s);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        return (s + s).find(s, 1) < s.size();
    }
};
```

#### Go

```go
func repeatedSubstringPattern(s string) bool {
	return strings.Index(s[1:]+s, s) < len(s)-1
}
```

#### TypeScript

```ts
function repeatedSubstringPattern(s: string): boolean {
    return (s + s).slice(1, (s.length << 1) - 1).includes(s);
}
```

#### Rust

```rust
impl Solution {
    pub fn repeated_substring_pattern(s: String) -> bool {
        (s.clone() + &s)[1..s.len() * 2 - 1].contains(&s)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
