---
comments: true
difficulty: Easy
tags:
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [392. Is Subsequence](https://leetcode.com/problems/is-subsequence)

[中文文档](/solution/0300-0399/0392.Is%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code>, trả về <code>true</code><em> nếu </em><code>s</code><em> là </em><strong>dãy con</strong><em> của </em><code>t</code><em>, nếu không thì trả về </em><code>false</code>.</p>

<p><strong>Dãy con</strong> của một chuỗi là chuỗi mới được tạo từ chuỗi ban đầu bằng cách xóa một số ký tự (có thể không xóa ký tự nào), nhưng vẫn giữ nguyên thứ tự tương đối của các ký tự còn lại. (Ví dụ, <code>&quot;ace&quot;</code> là dãy con của <code>&quot;<u>a</u>b<u>c</u>d<u>e</u>&quot;</code>, còn <code>&quot;aec&quot;</code> thì không.)</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = "abc", t = "ahbgdc"
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = "axc", t = "ahbgdc"
<strong>Đầu ra:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 100</code></li>
	<li><code>0 &lt;= t.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Giả sử có rất nhiều chuỗi <code>s</code> cần xử lý, chẳng hạn <code>s<sub>1</sub>, s<sub>2</sub>, ..., s<sub>k</sub></code> với <code>k &gt;= 10<sup>9</sup></code>, và bạn cần lần lượt kiểm tra xem mỗi chuỗi có phải là dãy con của <code>t</code> hay không. Trong trường hợp này, bạn sẽ thay đổi code như thế nào?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> $s$ có phải là dãy con của $t$ không? Mỗi ký tự của $s$ cần được ghép với một ký tự tương ứng ở phía sau trong $t$. Dùng hai con trỏ và duyệt một lượt.
>
> Con trỏ $i$ duyệt $s$, còn $j$ duyệt $t$; khi hai ký tự khớp nhau, tăng $i$. $s$ là dãy con khi và chỉ khi $i$ đi đến cuối chuỗi.

<!-- thinking:end -->

Ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến vị trí đầu của chuỗi $s$ và $t$. Mỗi lần so sánh hai ký tự mà chúng trỏ tới, nếu hai ký tự giống nhau thì tăng cả hai con trỏ; nếu khác nhau thì chỉ tăng $j$. Khi con trỏ $i$ đi đến cuối chuỗi $s$, điều đó có nghĩa là $s$ là dãy con của $t$.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của chuỗi $s$ và $t$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        i = j = 0
        while i < len(s) and j < len(t):
            if s[i] == t[j]:
                i += 1
            j += 1
        return i == len(s)
```

#### Java

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int m = s.length(), n = t.length();
        int i = 0, j = 0;
        while (i < m && j < n) {
            if (s.charAt(i) == t.charAt(j)) {
                ++i;
            }
            ++j;
        }
        return i == m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int m = s.size(), n = t.size();
        int i = 0, j = 0;
        for (; i < m && j < n; ++j) {
            if (s[i] == t[j]) {
                ++i;
            }
        }
        return i == m;
    }
};
```

#### Go

```go
func isSubsequence(s string, t string) bool {
	i, j, m, n := 0, 0, len(s), len(t)
	for i < m && j < n {
		if s[i] == t[j] {
			i++
		}
		j++
	}
	return i == m
}
```

#### TypeScript

```ts
function isSubsequence(s: string, t: string): boolean {
    const m = s.length;
    const n = t.length;
    let i = 0;
    for (let j = 0; i < m && j < n; ++j) {
        if (s[i] === t[j]) {
            ++i;
        }
    }
    return i === m;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_subsequence(s: String, t: String) -> bool {
        let (s, t) = (s.as_bytes(), t.as_bytes());
        let n = t.len();
        let mut i = 0;
        for &c in s.iter() {
            while i < n && t[i] != c {
                i += 1;
            }
            if i == n {
                return false;
            }
            i += 1;
        }
        true
    }
}
```

#### C#

```cs
public class Solution {
    public bool IsSubsequence(string s, string t) {
        int m = s.Length, n = t.Length;
        int i = 0, j = 0;
        for (; i < m && j < n; ++j) {
            if (s[i] == t[j]) {
                ++i;
            }
        }
        return i == m;
    }
}
```

#### C

```c
bool isSubsequence(char* s, char* t) {
    int m = strlen(s);
    int n = strlen(t);
    int i = 0;
    for (int j = 0; i < m && j < n; ++j) {
        if (s[i] == t[j]) {
            ++i;
        }
    }
    return i == m;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
