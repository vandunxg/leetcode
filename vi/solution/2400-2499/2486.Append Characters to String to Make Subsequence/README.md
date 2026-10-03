---
comments: true
difficulty: Medium
rating: 1362
source: Weekly Contest 321 Q2
tags:
    - Greedy
    - Two Pointers
    - String
---

<!-- problem:start -->

# [2486. Append Characters to String to Make Subsequence](https://leetcode.com/problems/append-characters-to-string-to-make-subsequence)

[Tài liệu tiếng Trung](/solution/2400-2499/2486.Append%20Characters%20to%20String%20to%20Make%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Hãy trả về <em>số ký tự tối thiểu cần nối vào cuối </em><code>s</code><em> để </em><code>t</code><em> trở thành một <strong>chuỗi con</strong> của </em><code>s</code>.</p>

<p><strong>Chuỗi con</strong> là một chuỗi có thể được tạo ra từ một chuỗi khác bằng cách xóa một số ký tự hoặc không xóa ký tự nào, nhưng không thay đổi thứ tự của các ký tự còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;coaching&quot;, t = &quot;coding&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Nối các ký tự &quot;ding&quot; vào cuối s để s = &quot;coachingding&quot;.
Khi đó, t là chuỗi con của s (&quot;<u><strong>co</strong></u>aching<u><strong>ding</strong></u>&quot;).
Có thể chứng minh rằng nối bất kỳ 3 ký tự nào vào cuối s cũng không thể làm t trở thành chuỗi con.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcde&quot;, t = &quot;a&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> t đã là chuỗi con của s (&quot;<u><strong>a</strong></u>bcde&quot;).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;z&quot;, t = &quot;abcde&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Nối các ký tự &quot;abcde&quot; vào cuối s để s = &quot;zabcde&quot;.
Khi đó, t là chuỗi con của s (&quot;z<u><strong>abcde</strong></u>&quot;).
Có thể chứng minh rằng nối bất kỳ 4 ký tự nào vào cuối s cũng không thể làm t trở thành chuỗi con.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Vì chỉ có thể nối thêm vào $s$ để $t$ trở thành một chuỗi con, số ký tự cần thêm chính là độ dài hậu tố chưa khớp của $t$. Với $n\le 10^5$, ta tham lam khớp tiền tố của $t$ trong $s$ rồi trả về $n-j$.

<!-- thinking:end -->

Ta dùng hai con trỏ $i$ và $j$, lần lượt trỏ đến các ký tự đầu tiên của hai chuỗi $s$ và $t$. Ta duyệt qua chuỗi $s$; nếu $s[i] = t[j]$ thì tăng $j$ lên một bước. Cuối cùng, ta trả về $n - j$, trong đó $n$ là độ dài của chuỗi $t$.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của các chuỗi $s$ và $t$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def appendCharacters(self, s: str, t: str) -> int:
        n, j = len(t), 0
        for c in s:
            if j < n and c == t[j]:
                j += 1
        return n - j
```

#### Java

```java
class Solution {
    public int appendCharacters(String s, String t) {
        int n = t.length(), j = 0;
        for (int i = 0; i < s.length() && j < n; ++i) {
            if (s.charAt(i) == t.charAt(j)) {
                ++j;
            }
        }
        return n - j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int appendCharacters(string s, string t) {
        int n = t.length(), j = 0;
        for (int i = 0; i < s.size() && j < n; ++i) {
            if (s[i] == t[j]) {
                ++j;
            }
        }
        return n - j;
    }
};
```

#### Go

```go
func appendCharacters(s string, t string) int {
	n, j := len(t), 0
	for _, c := range s {
		if j < n && byte(c) == t[j] {
			j++
		}
	}
	return n - j
}
```

#### TypeScript

```ts
function appendCharacters(s: string, t: string): number {
    let j = 0;
    for (const c of s) {
        if (c === t[j]) {
            ++j;
        }
    }
    return t.length - j;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
