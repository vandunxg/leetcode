---
comments: true
difficulty: Easy
rating: 1234
source: Weekly Contest 253 Q1
tags:
    - Array
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1961. Check If String Is a Prefix of Array](https://leetcode.com/problems/check-if-string-is-a-prefix-of-array)

[中文文档](/solution/1900-1999/1961.Check%20If%20String%20Is%20a%20Prefix%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một mảng các chuỗi <code>words</code>, hãy xác định liệu <code>s</code> có phải là một <strong>chuỗi tiền tố</strong> của <code>words</code> hay không.</p>

<p>Một chuỗi <code>s</code> là <strong>chuỗi tiền tố</strong> của <code>words</code> nếu có thể tạo ra <code>s</code> bằng cách nối <code>k</code> chuỗi đầu tiên trong <code>words</code> với một <strong>số nguyên dương</strong> <code>k</code> không lớn hơn <code>words.length</code>.</p>

<p>Trả về <code>true</code><em> nếu </em><code>s</code><em> là một <strong>chuỗi tiền tố</strong> của </em><code>words</code><em>, hoặc </em><code>false</code><em> nếu không phải</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;iloveleetcode&quot;, words = [&quot;i&quot;,&quot;love&quot;,&quot;leetcode&quot;,&quot;apples&quot;]
<strong>Output:</strong> true
<strong>Giải thích:</strong>
Có thể tạo s bằng cách nối &quot;i&quot;, &quot;love&quot; và &quot;leetcode&quot; với nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;iloveleetcode&quot;, words = [&quot;apples&quot;,&quot;i&quot;,&quot;love&quot;,&quot;leetcode&quot;]
<strong>Output:</strong> false
<strong>Giải thích:</strong>
Không thể tạo s bằng một tiền tố của arr.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 20</code></li>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>words[i]</code> và <code>s</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> $s$ phải bằng phép nối của một tiền tố nào đó của $\textit{words}$, không được thừa cũng không được thiếu. Ta cộng dồn độ dài và khi tổng độ dài lần đầu bằng $|s|$, ta so sánh chuỗi nối được.
>
> Nếu không có tiền tố nào có độ dài phù hợp, $s$ không phải là chuỗi tiền tố.

<!-- thinking:end -->

Ta duyệt mảng $words$, sử dụng một biến $t$ để lưu chuỗi đang được nối. Nếu độ dài của $t$ lớn hơn độ dài của $s$, điều đó có nghĩa là $s$ không phải là chuỗi tiền tố của $words$, nên ta trả về $false$; nếu độ dài của $t$ bằng độ dài của $s$, ta trả về việc $t$ có bằng $s$ hay không.

Kết thúc quá trình duyệt, nếu độ dài của $t$ nhỏ hơn độ dài của $s$, điều đó có nghĩa là $s$ không phải là chuỗi tiền tố của $words$, nên ta trả về $false$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPrefixString(self, s: str, words: List[str]) -> bool:
        n, m = len(s), 0
        for i, w in enumerate(words):
            m += len(w)
            if m == n:
                return "".join(words[: i + 1]) == s
        return False
```

#### Java

```java
class Solution {
    public boolean isPrefixString(String s, String[] words) {
        StringBuilder t = new StringBuilder();
        for (var w : words) {
            t.append(w);
            if (t.length() > s.length()) {
                return false;
            }
            if (t.length() == s.length()) {
                return s.equals(t.toString());
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPrefixString(string s, vector<string>& words) {
        string t;
        for (auto& w : words) {
            t += w;
            if (t.size() > s.size()) {
                return false;
            }
            if (t.size() == s.size()) {
                return t == s;
            }
        }
        return false;
    }
};
```

#### Go

```go
func isPrefixString(s string, words []string) bool {
	t := strings.Builder{}
	for _, w := range words {
		t.WriteString(w)
		if t.Len() > len(s) {
			return false
		}
		if t.Len() == len(s) {
			return t.String() == s
		}
	}
	return false
}
```

#### TypeScript

```ts
function isPrefixString(s: string, words: string[]): boolean {
    const t: string[] = [];
    const n = s.length;
    let m = 0;
    for (const w of words) {
        m += w.length;
        if (m > n) {
            return false;
        }
        t.push(w);
        if (m === n) {
            return s === t.join('');
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
