---
comments: true
difficulty: Easy
rating: 1125
source: Weekly Contest 190 Q1
tags:
    - Two Pointers
    - String
    - String Matching
---

<!-- problem:start -->

# [1455. Check If a Word Occurs As a Prefix of Any Word in a Sentence](https://leetcode.com/problems/check-if-a-word-occurs-as-a-prefix-of-any-word-in-a-sentence)

[中文文档](/solution/1400-1499/1455.Check%20If%20a%20Word%20Occurs%20As%20a%20Prefix%20of%20Any%20Word%20in%20a%20Sentence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>sentence</code> gồm các từ được phân tách bằng <strong>một dấu cách duy nhất</strong>, và một <code>searchWord</code>, hãy kiểm tra xem <code>searchWord</code> có phải là tiền tố của bất kỳ từ nào trong <code>sentence</code> hay không.</p>

<p>Trả về <em>chỉ số của từ trong </em><code>sentence</code><em> (<strong>đánh số từ 1</strong>) mà tại đó </em><code>searchWord</code><em> là tiền tố của từ này</em>. Nếu <code>searchWord</code> là tiền tố của nhiều hơn một từ, hãy trả về chỉ số của từ đầu tiên <strong>(chỉ số nhỏ nhất)</strong>. Nếu không có từ nào như vậy, trả về <code>-1</code>.</p>

<p><strong>Tiền tố</strong> của một chuỗi <code>s</code> là bất kỳ chuỗi con liên tiếp nào nằm ở đầu của <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;i love eating burger&quot;, searchWord = &quot;burg&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> &quot;burg&quot; là tiền tố của &quot;burger&quot;, là từ thứ 4 trong câu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;this problem is an easy problem&quot;, searchWord = &quot;pro&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> &quot;pro&quot; là tiền tố của &quot;problem&quot;, là từ thứ 2 và thứ 6 trong câu, nhưng ta trả về 2 vì đó là chỉ số nhỏ nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;i am tired&quot;, searchWord = &quot;you&quot;
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> &quot;you&quot; không phải là tiền tố của bất kỳ từ nào trong câu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 100</code></li>
	<li><code>1 &lt;= searchWord.length &lt;= 10</code></li>
	<li><code>sentence</code> chỉ gồm các chữ cái tiếng Anh viết thường và dấu cách.</li>
	<li><code>searchWord</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tách chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Câu có độ dài ngắn. Ta tách câu theo dấu cách và trả về chỉ số bắt đầu từ 1 của từ đầu tiên bắt đầu bằng $\textit{searchWord}$.

<!-- thinking:end -->

Ta tách $\textit{sentence}$ theo dấu cách thành $\textit{words}$, sau đó duyệt qua $\textit{words}$ để kiểm tra xem $\textit{words}[i]$ có bắt đầu bằng $\textit{searchWord}$ hay không. Nếu có, ta trả về $i+1$. Nếu duyệt hết mà không có từ nào thỏa mãn điều kiện, ta trả về $-1$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m)$. Trong đó, $m$ và $n$ lần lượt là độ dài của $\textit{sentence}$ và $\textit{searchWord}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPrefixOfWord(self, sentence: str, searchWord: str) -> int:
        for i, s in enumerate(sentence.split(), 1):
            if s.startswith(searchWord):
                return i
        return -1
```

#### Java

```java
class Solution {
    public int isPrefixOfWord(String sentence, String searchWord) {
        String[] words = sentence.split(" ");
        for (int i = 0; i < words.length; ++i) {
            if (words[i].startsWith(searchWord)) {
                return i + 1;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int isPrefixOfWord(string sentence, string searchWord) {
        stringstream ss(sentence);
        string s;
        for (int i = 1; ss >> s; ++i) {
            if (s.find(searchWord) == 0) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func isPrefixOfWord(sentence string, searchWord string) int {
	for i, s := range strings.Split(sentence, " ") {
		if strings.HasPrefix(s, searchWord) {
			return i + 1
		}
	}
	return -1
}
```

#### TypeScript

```ts
function isPrefixOfWord(sentence: string, searchWord: string): number {
    const ss = sentence.split(/\s/);
    const n = ss.length;
    for (let i = 0; i < n; i++) {
        if (ss[i].startsWith(searchWord)) {
            return i + 1;
        }
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_prefix_of_word(sentence: String, search_word: String) -> i32 {
        let ss = sentence.split_whitespace().collect::<Vec<&str>>();
        for i in 0..ss.len() {
            if ss[i].starts_with(&search_word) {
                return (i + 1) as i32;
            }
        }
        -1
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $sentence
     * @param String $searchWord
     * @return Integer
     */
    function isPrefixOfWord($sentence, $searchWord) {
        $words = explode(' ', $sentence);
        for ($i = 0; $i < count($words); ++$i) {
            if (strpos($words[$i], $searchWord) === 0) {
                return $i + 1;
            }
        }
        return -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
