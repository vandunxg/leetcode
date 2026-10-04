---
comments: true
difficulty: Easy
rating: 1173
source: Weekly Contest 389 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [3083. Existence of a Substring in a String and Its Reverse](https://leetcode.com/problems/existence-of-a-substring-in-a-string-and-its-reverse)

[中文文档](/solution/3000-3099/3083.Existence%20of%20a%20Substring%20in%20a%20String%20and%20Its%20Reverse/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một<strong> </strong>chuỗi <code>s</code>, hãy tìm một <span data-keyword="substring">chuỗi con</span> có độ dài <code>2</code> cũng xuất hiện trong chuỗi đảo ngược của <code>s</code>.</p>

<p>Trả về <code>true</code><em> nếu tồn tại chuỗi con như vậy, </em><code>false</code><em> nếu không.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">s = &quot;leetcode&quot;</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">true</span></p>

<p><strong>Giải thích:</strong> Chuỗi con <code>&quot;ee&quot;</code> có độ dài <code>2</code> và cũng xuất hiện trong <code>reverse(s) == &quot;edocteel&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">s = &quot;abcba&quot;</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">true</span></p>

<p><strong>Giải thích:</strong> Tất cả các chuỗi con có độ dài <code>2</code> <code>&quot;ab&quot;</code>, <code>&quot;bc&quot;</code>, <code>&quot;cb&quot;</code>, <code>&quot;ba&quot;</code> đều xuất hiện trong <code>reverse(s) == &quot;abcba&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">s = &quot;abcd&quot;</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">false</span></p>

<p><strong>Giải thích:</strong> Không có chuỗi con nào có độ dài <code>2</code> trong <code>s</code> đồng thời xuất hiện trong chuỗi đảo ngược của <code>s</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table hoặc Mảng

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 100$ và chỉ có vài chuỗi con độ dài $2$. Chúng ta chỉ cần tìm một cặp ký tự liền kề xuất hiện trong cả $s$ và chuỗi đảo ngược của nó.
>
> Lưu mọi cặp ký tự liền kề của chuỗi đảo ngược, sau đó truy vấn các cặp ký tự của $s$.
>
> Cả bước xây dựng và truy vấn đều có độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta có thể sử dụng hash table hoặc mảng hai chiều $st$ để lưu tất cả các chuỗi con có độ dài $2$ của chuỗi đảo ngược của $s$.

Sau đó, ta duyệt chuỗi $s$. Với mỗi chuỗi con có độ dài $2$, ta kiểm tra xem nó đã xuất hiện trong $st$ hay chưa. Nếu có, ta trả về `true`. Nếu không, sau khi duyệt xong ta trả về `false`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(|\Sigma|^2)$. Trong đó, $n$ là độ dài của chuỗi $s$, còn $\Sigma$ là tập ký tự của chuỗi $s$. Trong bài toán này, $\Sigma$ gồm các chữ cái tiếng Anh viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isSubstringPresent(self, s: str) -> bool:
        st = {(a, b) for a, b in pairwise(s[::-1])}
        return any((a, b) in st for a, b in pairwise(s))
```

#### Java

```java
class Solution {
    public boolean isSubstringPresent(String s) {
        boolean[][] st = new boolean[26][26];
        int n = s.length();
        for (int i = 0; i < n - 1; ++i) {
            st[s.charAt(i + 1) - 'a'][s.charAt(i) - 'a'] = true;
        }
        for (int i = 0; i < n - 1; ++i) {
            if (st[s.charAt(i) - 'a'][s.charAt(i + 1) - 'a']) {
                return true;
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
    bool isSubstringPresent(string s) {
        bool st[26][26]{};
        int n = s.size();
        for (int i = 0; i < n - 1; ++i) {
            st[s[i + 1] - 'a'][s[i] - 'a'] = true;
        }
        for (int i = 0; i < n - 1; ++i) {
            if (st[s[i] - 'a'][s[i + 1] - 'a']) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func isSubstringPresent(s string) bool {
	st := [26][26]bool{}
	for i := 0; i < len(s)-1; i++ {
		st[s[i+1]-'a'][s[i]-'a'] = true
	}
	for i := 0; i < len(s)-1; i++ {
		if st[s[i]-'a'][s[i+1]-'a'] {
			return true
		}
	}
	return false
}
```

#### TypeScript

```ts
function isSubstringPresent(s: string): boolean {
    const st: boolean[][] = Array.from({ length: 26 }, () => Array(26).fill(false));
    for (let i = 0; i < s.length - 1; ++i) {
        st[s.charCodeAt(i + 1) - 97][s.charCodeAt(i) - 97] = true;
    }
    for (let i = 0; i < s.length - 1; ++i) {
        if (st[s.charCodeAt(i) - 97][s.charCodeAt(i + 1) - 97]) {
            return true;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
