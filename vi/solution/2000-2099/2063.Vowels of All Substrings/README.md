---
comments: true
difficulty: Medium
rating: 1663
source: Weekly Contest 266 Q2
tags:
    - Math
    - String
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [2063. Vowels of All Substrings](https://leetcode.com/problems/vowels-of-all-substrings)

[中文文档](/solution/2000-2099/2063.Vowels%20of%20All%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code>, hãy trả về <em><strong>tổng số nguyên âm</strong> (</em><code>&#39;a&#39;</code>, <code>&#39;e&#39;</code><em>,</em> <code>&#39;i&#39;</code><em>,</em> <code>&#39;o&#39;</code><em> và</em> <code>&#39;u&#39;</code><em>)</em> <em>trong mọi chuỗi con của </em><code>word</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp (không rỗng) trong một chuỗi.</p>

<p><strong>Lưu ý:</strong> Do các ràng buộc lớn, đáp án có thể không vừa với số nguyên 32-bit có dấu. Hãy cẩn thận trong quá trình tính toán.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aba&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Tất cả các chuỗi con có thể là: &quot;a&quot;, &quot;ab&quot;, &quot;aba&quot;, &quot;b&quot;, &quot;ba&quot; và &quot;a&quot;.
- &quot;b&quot; có 0 nguyên âm
- &quot;a&quot;, &quot;ab&quot;, &quot;ba&quot; và &quot;a&quot; mỗi chuỗi có 1 nguyên âm
- &quot;aba&quot; có 2 nguyên âm
Do đó, tổng số nguyên âm = 0 + 1 + 1 + 1 + 1 + 2 = 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abc&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Tất cả các chuỗi con có thể là: &quot;a&quot;, &quot;ab&quot;, &quot;abc&quot;, &quot;b&quot;, &quot;bc&quot; và &quot;c&quot;.
- &quot;a&quot;, &quot;ab&quot; và &quot;abc&quot; mỗi chuỗi có 1 nguyên âm
- &quot;b&quot;, &quot;bc&quot; và &quot;c&quot; mỗi chuỗi có 0 nguyên âm
Do đó, tổng số nguyên âm = 1 + 1 + 1 + 0 + 0 + 0 = 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;ltcd&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có nguyên âm nào trong bất kỳ chuỗi con nào của &quot;ltcd&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 10<sup>5</sup></code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm đóng góp

<!-- thinking:start -->

> **Tư duy**
>
> Có $O(n^2)$ chuỗi con và $n \le 10^5$, nên không thể liệt kê chúng. Một nguyên âm ở vị trí $i$ xuất hiện trong $(i+1)(n-i)$ chuỗi con.
>
> Chỉ cần cộng tích đó với các nguyên âm.

<!-- thinking:end -->

Ta có thể duyệt qua từng ký tự $\textit{word}[i]$ trong chuỗi. Nếu $\textit{word}[i]$ là một nguyên âm, thì $\textit{word}[i]$ xuất hiện trong $(i + 1) \times (n - i)$ chuỗi con. Ta cộng số lượng các chuỗi con này lại.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{word}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countVowels(self, word: str) -> int:
        n = len(word)
        return sum((i + 1) * (n - i) for i, c in enumerate(word) if c in 'aeiou')
```

#### Java

```java
class Solution {
    public long countVowels(String word) {
        long ans = 0;
        for (int i = 0, n = word.length(); i < n; ++i) {
            char c = word.charAt(i);
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                ans += (i + 1L) * (n - i);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countVowels(string word) {
        long long ans = 0;
        for (int i = 0, n = word.size(); i < n; ++i) {
            char c = word[i];
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                ans += (i + 1LL) * (n - i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countVowels(word string) (ans int64) {
	for i, c := range word {
		if c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' {
			ans += int64((i + 1) * (len(word) - i))
		}
	}
	return
}
```

#### TypeScript

```ts
function countVowels(word: string): number {
    const n = word.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (['a', 'e', 'i', 'o', 'u'].includes(word[i])) {
            ans += (i + 1) * (n - i);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_vowels(word: String) -> i64 {
        let n = word.len() as i64;
        word.chars()
            .enumerate()
            .filter(|(_, c)| "aeiou".contains(*c))
            .map(|(i, _)| (i as i64 + 1) * (n - i as i64))
            .sum()
    }
}
```

#### JavaScript

```js
/**
 * @param {string} word
 * @return {number}
 */
var countVowels = function (word) {
    const n = word.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (['a', 'e', 'i', 'o', 'u'].includes(word[i])) {
            ans += (i + 1) * (n - i);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
