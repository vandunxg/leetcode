---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [804. Unique Morse Code Words](https://leetcode.com/problems/unique-morse-code-words)

[中文文档](/solution/0800-0899/0804.Unique%20Morse%20Code%20Words/README.md)

## Mô tả

<!-- description:start -->

<p>Mã Morse quốc tế quy định cách mã hóa chuẩn, trong đó mỗi chữ cái được ánh xạ thành một chuỗi dấu chấm và dấu gạch như sau:</p>

<ul>
	<li><code>&#39;a&#39;</code> được ánh xạ thành <code>&quot;.-&quot;</code>,</li>
	<li><code>&#39;b&#39;</code> được ánh xạ thành <code>&quot;-...&quot;</code>,</li>
	<li><code>&#39;c&#39;</code> được ánh xạ thành <code>&quot;-.-.&quot;</code>, và các chữ cái còn lại cũng tương tự.</li>
</ul>

<p>Để tiện sử dụng, bảng mã đầy đủ cho <code>26</code> chữ cái trong bảng chữ cái tiếng Anh được cho bên dưới:</p>

<pre>
[&quot;.-&quot;,&quot;-...&quot;,&quot;-.-.&quot;,&quot;-..&quot;,&quot;.&quot;,&quot;..-.&quot;,&quot;--.&quot;,&quot;....&quot;,&quot;..&quot;,&quot;.---&quot;,&quot;-.-&quot;,&quot;.-..&quot;,&quot;--&quot;,&quot;-.&quot;,&quot;---&quot;,&quot;.--.&quot;,&quot;--.-&quot;,&quot;.-.&quot;,&quot;...&quot;,&quot;-&quot;,&quot;..-&quot;,&quot;...-&quot;,&quot;.--&quot;,&quot;-..-&quot;,&quot;-.--&quot;,&quot;--..&quot;]</pre>

<p>Cho mảng chuỗi <code>words</code>, trong đó mỗi từ có thể được biểu diễn bằng cách nối mã Morse của từng chữ cái.</p>

<ul>
	<li>Ví dụ, <code>&quot;cab&quot;</code> có thể được viết thành <code>&quot;-.-..--...&quot;</code>, là kết quả nối <code>&quot;-.-.&quot;</code>, <code>&quot;.-&quot;</code> và <code>&quot;-...&quot;</code>. Ta gọi chuỗi được nối này là <strong>mã biến đổi</strong> của một từ.</li>
</ul>

<p>Hãy trả về <em>số lượng <strong>mã biến đổi</strong> khác nhau của tất cả các từ đã cho</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;gin&quot;,&quot;zen&quot;,&quot;gig&quot;,&quot;msg&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Mã biến đổi của từng từ là:
&quot;gin&quot; -&gt; &quot;--...-.&quot;
&quot;zen&quot; -&gt; &quot;--...-.&quot;
&quot;gig&quot; -&gt; &quot;--...--.&quot;
&quot;msg&quot; -&gt; &quot;--...--.&quot;
Có 2 mã biến đổi khác nhau: &quot;--...-.&quot; và &quot;--...--.&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 12</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi từ được chuyển thành chuỗi Morse bằng cách mã hóa từng chữ cái; ta chỉ cần đếm số chuỗi mã hóa khác nhau. Với tối đa $100$ từ, mỗi từ có độ dài $\le 12$, chỉ cần chuyển đổi trực tiếp.
>
> Lưu các chuỗi mã hóa vào một set rồi trả về kích thước của set. Những từ có cùng mã hóa chỉ được tính là một mã biến đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uniqueMorseRepresentations(self, words: List[str]) -> int:
        codes = [
            ".-",
            "-...",
            "-.-.",
            "-..",
            ".",
            "..-.",
            "--.",
            "....",
            "..",
            ".---",
            "-.-",
            ".-..",
            "--",
            "-.",
            "---",
            ".--.",
            "--.-",
            ".-.",
            "...",
            "-",
            "..-",
            "...-",
            ".--",
            "-..-",
            "-.--",
            "--..",
        ]
        s = {''.join([codes[ord(c) - ord('a')] for c in word]) for word in words}
        return len(s)
```

#### Java

```java
class Solution {
    public int uniqueMorseRepresentations(String[] words) {
        String[] codes = new String[] {".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....",
            "..", ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-",
            "..-", "...-", ".--", "-..-", "-.--", "--.."};
        Set<String> s = new HashSet<>();
        for (String word : words) {
            StringBuilder t = new StringBuilder();
            for (char c : word.toCharArray()) {
                t.append(codes[c - 'a']);
            }
            s.add(t.toString());
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int uniqueMorseRepresentations(vector<string>& words) {
        vector<string> codes = {".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..", "--", "-.",
            "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.."};
        unordered_set<string> s;
        for (auto& word : words) {
            string t;
            for (char& c : word) t += codes[c - 'a'];
            s.insert(t);
        }
        return s.size();
    }
};
```

#### Go

```go
func uniqueMorseRepresentations(words []string) int {
	codes := []string{".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..", "--", "-.",
		"---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.."}
	s := make(map[string]bool)
	for _, word := range words {
		t := &strings.Builder{}
		for _, c := range word {
			t.WriteString(codes[c-'a'])
		}
		s[t.String()] = true
	}
	return len(s)
}
```

#### TypeScript

```ts
const codes = [
    '.-',
    '-...',
    '-.-.',
    '-..',
    '.',
    '..-.',
    '--.',
    '....',
    '..',
    '.---',
    '-.-',
    '.-..',
    '--',
    '-.',
    '---',
    '.--.',
    '--.-',
    '.-.',
    '...',
    '-',
    '..-',
    '...-',
    '.--',
    '-..-',
    '-.--',
    '--..',
];

function uniqueMorseRepresentations(words: string[]): number {
    return new Set(
        words.map(word => {
            return word
                .split('')
                .map(c => codes[c.charCodeAt(0) - 'a'.charCodeAt(0)])
                .join('');
        }),
    ).size;
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn unique_morse_representations(words: Vec<String>) -> i32 {
        const codes: [&str; 26] = [
            ".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..",
            "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-",
            "-.--", "--..",
        ];
        words
            .iter()
            .map(|word| {
                word.as_bytes()
                    .iter()
                    .map(|v| codes[(v - b'a') as usize])
                    .collect::<String>()
            })
            .collect::<HashSet<String>>()
            .len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
