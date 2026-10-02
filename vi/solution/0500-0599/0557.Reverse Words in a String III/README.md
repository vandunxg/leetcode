---
comments: true
difficulty: Easy
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [557. Reverse Words in a String III](https://leetcode.com/problems/reverse-words-in-a-string-iii)

[中文文档](/solution/0500-0599/0557.Reverse%20Words%20in%20a%20String%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy đảo ngược thứ tự ký tự trong từng từ của câu, đồng thời giữ nguyên khoảng trắng và thứ tự ban đầu của các từ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Let&#39;s take LeetCode contest&quot;
<strong>Đầu ra:</strong> &quot;s&#39;teL ekat edoCteeL tsetnoc&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Mr Ding&quot;
<strong>Đầu ra:</strong> &quot;rM gniD&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ chứa các ký tự <strong>ASCII</strong> có thể in được.</li>
	<li><code>s</code> không có khoảng trắng ở đầu hoặc cuối.</li>
	<li><code>s</code> có <strong>ít nhất một</strong> từ.</li>
	<li>Các từ trong <code>s</code> được phân tách bằng đúng một dấu cách.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Đảo ngược từng từ và giữ nguyên dấu cách. Tách chuỗi, đảo ngược từng từ rồi nối lại.
>
> `split` tách chuỗi thành các từ; đảo ngược từng từ rồi nối lại bằng dấu cách. Không cần tự cài đặt bằng two pointers, trừ khi không được dùng hàm thư viện.

<!-- thinking:end -->

Ta có thể tách chuỗi $\textit{s}$ theo dấu cách thành mảng các từ $\textit{words}$, sau đó đảo ngược từng từ rồi nối chúng lại thành một chuỗi.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{s}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        return " ".join(t[::-1] for t in s.split())
```

#### Java

```java
class Solution {
    public String reverseWords(String s) {
        String[] words = s.split(" ");
        for (int i = 0; i < words.length; ++i) {
            words[i] = new StringBuilder(words[i]).reverse().toString();
        }
        return String.join(" ", words);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reverseWords(string s) {
        stringstream ss(s);
        string t;
        string ans;
        while (ss >> t) {
            reverse(t.begin(), t.end());
            ans += t;
            ans.push_back(' ');
        }
        ans.pop_back();
        return ans;
    }
};
```

#### Go

```go
func reverseWords(s string) string {
	words := strings.Fields(s)
	for i, w := range words {
		t := []byte(w)
		slices.Reverse(t)
		words[i] = string(t)
	}
	return strings.Join(words, " ")
}
```

#### TypeScript

```ts
function reverseWords(s: string): string {
    return s
        .split(' ')
        .map(t => t.split('').reverse().join(''))
        .join(' ');
}
```

#### Rust

```rust
impl Solution {
    pub fn reverse_words(s: String) -> String {
        s.split(' ')
            .map(|s| s.chars().rev().collect::<String>())
            .collect::<Vec<_>>()
            .join(" ")
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {string}
 */
var reverseWords = function (s) {
    return s
        .split(' ')
        .map(t => t.split('').reverse().join(''))
        .join(' ');
};
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return String
     */
    function reverseWords($s) {
        $words = explode(' ', $s);
        foreach ($words as $i => $word) {
            $words[$i] = strrev($word);
        }
        return implode(' ', $words);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
