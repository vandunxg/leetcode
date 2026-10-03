---
comments: true
difficulty: Easy
rating: 1521
source: Biweekly Contest 46 Q1
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Divide and Conquer
    - Sliding Window
---

<!-- problem:start -->

# [1763. Longest Nice Substring](https://leetcode.com/problems/longest-nice-substring)

[中文文档](/solution/1700-1799/1763.Longest%20Nice%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi <code>s</code> là <strong>đẹp</strong> nếu với mỗi chữ cái xuất hiện trong <code>s</code>, cả dạng viết hoa và viết thường của nó đều xuất hiện. Ví dụ, <code>&quot;abABB&quot;</code> là chuỗi đẹp vì có <code>&#39;A&#39;</code>, <code>&#39;a&#39;</code>, <code>&#39;B&#39;</code> và <code>&#39;b&#39;</code>. Tuy nhiên, <code>&quot;abA&quot;</code> không đẹp vì có <code>&#39;b&#39;</code> nhưng không có <code>&#39;B&#39;</code>.</p>

<p>Cho chuỗi <code>s</code>, hãy trả về <em><strong>chuỗi con</strong> <strong>dài nhất</strong> của <code>s</code> là chuỗi <strong>đẹp</strong>. Nếu có nhiều chuỗi, trả về chuỗi con xuất hiện <strong>sớm nhất</strong>. Nếu không có, trả về chuỗi rỗng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;YazaAay&quot;
<strong>Đầu ra:</strong> &quot;aAa&quot;
<strong>Giải thích: </strong>&quot;aAa&quot; là chuỗi đẹp vì &#39;A/a&#39; là chữ cái duy nhất xuất hiện trong s và cả &#39;A&#39; lẫn &#39;a&#39; đều xuất hiện.
&quot;aAa&quot; là chuỗi con đẹp dài nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Bb&quot;
<strong>Đầu ra:</strong> &quot;Bb&quot;
<strong>Giải thích:</strong> &quot;Bb&quot; là chuỗi đẹp vì cả &#39;B&#39; và &#39;b&#39; đều xuất hiện. Toàn bộ chuỗi là một chuỗi con.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;c&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> Không có chuỗi con đẹp nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết hoa và viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi con đẹp chứa cả hai dạng viết hoa và viết thường của mọi chữ cái xuất hiện. $n$ đủ nhỏ để thử tất cả chuỗi con.
>
> Cố định đầu trái $i$, mở rộng sang phải và lưu các ký tự vào một set. Khi mọi chữ cái đều có đủ hai dạng và cửa sổ dài hơn, ghi nhận nó. Nếu bằng nhau, giữ chuỗi con xuất hiện sớm hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestNiceSubstring(self, s: str) -> str:
        n = len(s)
        ans = ''
        for i in range(n):
            ss = set()
            for j in range(i, n):
                ss.add(s[j])
                if (
                    all(c.lower() in ss and c.upper() in ss for c in ss)
                    and len(ans) < j - i + 1
                ):
                    ans = s[i : j + 1]
        return ans
```

#### Java

```java
class Solution {
    public String longestNiceSubstring(String s) {
        int n = s.length();
        int k = -1;
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            Set<Character> ss = new HashSet<>();
            for (int j = i; j < n; ++j) {
                ss.add(s.charAt(j));
                boolean ok = true;
                for (char a : ss) {
                    char b = (char) (a ^ 32);
                    if (!(ss.contains(a) && ss.contains(b))) {
                        ok = false;
                        break;
                    }
                }
                if (ok && mx < j - i + 1) {
                    mx = j - i + 1;
                    k = i;
                }
            }
        }
        return k == -1 ? "" : s.substring(k, k + mx);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string longestNiceSubstring(string s) {
        int n = s.size();
        int k = -1, mx = 0;
        for (int i = 0; i < n; ++i) {
            unordered_set<char> ss;
            for (int j = i; j < n; ++j) {
                ss.insert(s[j]);
                bool ok = true;
                for (auto& a : ss) {
                    char b = a ^ 32;
                    if (!(ss.count(a) && ss.count(b))) {
                        ok = false;
                        break;
                    }
                }
                if (ok && mx < j - i + 1) {
                    mx = j - i + 1;
                    k = i;
                }
            }
        }
        return k == -1 ? "" : s.substr(k, mx);
    }
};
```

#### Go

```go
func longestNiceSubstring(s string) string {
	n := len(s)
	k, mx := -1, 0
	for i := 0; i < n; i++ {
		ss := map[byte]bool{}
		for j := i; j < n; j++ {
			ss[s[j]] = true
			ok := true
			for a := range ss {
				b := a ^ 32
				if !(ss[a] && ss[b]) {
					ok = false
					break
				}
			}
			if ok && mx < j-i+1 {
				mx = j - i + 1
				k = i
			}
		}
	}
	if k < 0 {
		return ""
	}
	return s[k : k+mx]
}
```

#### TypeScript

```ts
function longestNiceSubstring(s: string): string {
    const n = s.length;
    let ans = '';
    for (let i = 0; i < n; i++) {
        let lower = 0,
            upper = 0;
        for (let j = i; j < n; j++) {
            const c = s.charCodeAt(j);
            if (c > 96) {
                lower |= 1 << (c - 97);
            } else {
                upper |= 1 << (c - 65);
            }
            if (lower == upper && j - i + 1 > ans.length) {
                ans = s.substring(i, j + 1);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt set cho mỗi cửa sổ, phát sinh thêm hệ số $C$. Với $26$ chữ cái, chỉ cần hai bitmask cho chữ thường và chữ hoa; cửa sổ đẹp khi và chỉ khi hai bitmask bằng nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestNiceSubstring(self, s: str) -> str:
        n = len(s)
        ans = ''
        for i in range(n):
            lower = upper = 0
            for j in range(i, n):
                if s[j].islower():
                    lower |= 1 << (ord(s[j]) - ord('a'))
                else:
                    upper |= 1 << (ord(s[j]) - ord('A'))
                if lower == upper and len(ans) < j - i + 1:
                    ans = s[i : j + 1]
        return ans
```

#### Java

```java
class Solution {
    public String longestNiceSubstring(String s) {
        int n = s.length();
        int k = -1;
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            int lower = 0, upper = 0;
            for (int j = i; j < n; ++j) {
                char c = s.charAt(j);
                if (Character.isLowerCase(c)) {
                    lower |= 1 << (c - 'a');
                } else {
                    upper |= 1 << (c - 'A');
                }
                if (lower == upper && mx < j - i + 1) {
                    mx = j - i + 1;
                    k = i;
                }
            }
        }
        return k == -1 ? "" : s.substring(k, k + mx);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string longestNiceSubstring(string s) {
        int n = s.size();
        int k = -1, mx = 0;
        for (int i = 0; i < n; ++i) {
            int lower = 0, upper = 0;
            for (int j = i; j < n; ++j) {
                char c = s[j];
                if (islower(c))
                    lower |= 1 << (c - 'a');
                else
                    upper |= 1 << (c - 'A');
                if (lower == upper && mx < j - i + 1) {
                    mx = j - i + 1;
                    k = i;
                }
            }
        }
        return k == -1 ? "" : s.substr(k, mx);
    }
};
```

#### Go

```go
func longestNiceSubstring(s string) string {
	n := len(s)
	k, mx := -1, 0
	for i := 0; i < n; i++ {
		var lower, upper int
		for j := i; j < n; j++ {
			if unicode.IsLower(rune(s[j])) {
				lower |= 1 << (s[j] - 'a')
			} else {
				upper |= 1 << (s[j] - 'A')
			}
			if lower == upper && mx < j-i+1 {
				mx = j - i + 1
				k = i
			}
		}
	}
	if k < 0 {
		return ""
	}
	return s[k : k+mx]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
