---
comments: true
difficulty: Easy
rating: 1628
source: Weekly Contest 173 Q1
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1332. Remove Palindromic Subsequences](https://leetcode.com/problems/remove-palindromic-subsequences)

[中文文档](/solution/1300-1399/1332.Remove%20Palindromic%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho chuỗi <code>s</code> chỉ gồm các chữ cái <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code>. Trong một bước, bạn có thể xóa một <strong>dãy con palindrome</strong> khỏi <code>s</code>.</p>

<p>Hãy trả về <em>số bước <strong>ít nhất</strong> để xóa hết chuỗi đã cho</em>.</p>

<p>Một chuỗi là <strong>dãy con</strong> của chuỗi đã cho nếu có thể tạo ra nó bằng cách xóa một số ký tự mà không thay đổi thứ tự các ký tự còn lại. Lưu ý rằng dãy con <strong>không nhất thiết</strong> phải gồm các ký tự liên tiếp.</p>

<p>Một chuỗi được gọi là <strong>palindrome</strong> nếu đọc xuôi hay đọc ngược đều giống nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ababa&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> s đã là palindrome, nên có thể xóa toàn bộ chuỗi chỉ trong một bước.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abb&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> &quot;<u>a</u>bb&quot; -&gt; &quot;<u>bb</u>&quot; -&gt; &quot;&quot;. 
Xóa dãy con palindrome &quot;a&quot;, sau đó xóa &quot;bb&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;baabb&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> &quot;<u>baa</u>b<u>b</u>&quot; -&gt; &quot;<u>b</u>&quot; -&gt; &quot;&quot;. 
Xóa dãy con palindrome &quot;baab&quot;, sau đó xóa &quot;b&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> là <code>&#39;a&#39;</code> hoặc <code>&#39;b&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần xóa loại bỏ một dãy con palindrome, và chuỗi chỉ có hai ký tự `'a'` và `'b'`. Mọi dãy con chỉ gồm một loại ký tự đều là palindrome, nên luôn có thể xóa hết trong hai bước. Nếu bản thân $s$ là palindrome thì chỉ cần một bước. So sánh $s$ với chuỗi đảo ngược của nó để xác định đáp án là $1$ hay $2$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removePalindromeSub(self, s: str) -> int:
        return 1 if s[::-1] == s else 2
```

#### Java

```java
class Solution {
    public int removePalindromeSub(String s) {
        for (int i = 0, j = s.length() - 1; i < j; ++i, --j) {
            if (s.charAt(i) != s.charAt(j)) {
                return 2;
            }
        }
        return 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int removePalindromeSub(string s) {
        for (int i = 0, j = s.size() - 1; i < j; ++i, --j) {
            if (s[i] != s[j]) {
                return 2;
            }
        }
        return 1;
    }
};
```

#### Go

```go
func removePalindromeSub(s string) int {
	for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
		if s[i] != s[j] {
			return 2
		}
	}
	return 1
}
```

#### TypeScript

```ts
function removePalindromeSub(s: string): number {
    for (let i = 0, j = s.length - 1; i < j; ++i, --j) {
        if (s[i] !== s[j]) {
            return 2;
        }
    }
    return 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn remove_palindrome_sub(s: String) -> i32 {
        let mut l = 0;
        let mut r = s.len() - 1;
        let s: Vec<char> = s.chars().collect();
        while l < r {
            if s[l] != s[r] {
                return 2;
            }
            l += 1;
            r -= 1;
        }
        1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
