---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [824. Goat Latin](https://leetcode.com/problems/goat-latin)

[中文文档](/solution/0800-0899/0824.Goat%20Latin/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>sentence</code> gồm các từ được phân tách bằng dấu cách. Mỗi từ chỉ chứa chữ cái viết thường và viết hoa.</p>

<p>Ta muốn chuyển câu sang &quot;Goat Latin&quot; (một ngôn ngữ hư cấu tương tự Pig Latin). Quy tắc của Goat Latin như sau:</p>

<ul>
	<li>Nếu từ bắt đầu bằng một nguyên âm (<code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> hoặc <code>&#39;u&#39;</code>), thêm <code>&quot;ma&quot;</code> vào cuối từ.

    <ul>
    	<li>Ví dụ, từ <code>&quot;apple&quot;</code> trở thành <code>&quot;applema&quot;</code>.</li>
    </ul>
    </li>
    <li>Nếu từ bắt đầu bằng một phụ âm (tức không phải nguyên âm), chuyển chữ cái đầu tiên xuống cuối từ rồi thêm <code>&quot;ma&quot;</code>.
    <ul>
    	<li>Ví dụ, từ <code>&quot;goat&quot;</code> trở thành <code>&quot;oatgma&quot;</code>.</li>
    </ul>
    </li>
    <li>Thêm các chữ cái <code>&#39;a&#39;</code> vào cuối mỗi từ theo chỉ số của từ đó trong câu, bắt đầu từ <code>1</code>; số chữ cái được thêm bằng chỉ số này.
    <ul>
    	<li>Ví dụ, thêm <code>&quot;a&quot;</code> vào cuối từ thứ nhất, thêm <code>&quot;aa&quot;</code> vào cuối từ thứ hai, cứ tiếp tục như vậy.</li>
    </ul>
    </li>

</ul>

<p>Hãy trả về <em>câu cuối cùng sau khi chuyển sang Goat Latin</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> sentence = "I speak Goat Latin"
<strong>Đầu ra:</strong> "Imaa peaksmaaa oatGmaaaa atinLmaaaaa"
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> sentence = "The quick brown fox jumped over the lazy dog"
<strong>Đầu ra:</strong> "heTmaa uickqmaaa rownbmaaaa oxfmaaaaa umpedjmaaaaaa overmaaaaaaa hetmaaaaaaaa azylmaaaaaaaaa ogdmaaaaaaaaaa"
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 150</code></li>
	<li><code>sentence</code> chỉ gồm chữ cái tiếng Anh và dấu cách.</li>
	<li><code>sentence</code> không có dấu cách ở đầu hoặc cuối.</li>
	<li>Các từ trong <code>sentence</code> được phân tách bằng đúng một dấu cách.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Viết lại từng từ: nếu từ bắt đầu bằng phụ âm thì chuyển chữ cái đầu xuống cuối, sau đó thêm $\textit{ma}$ và một dãy chữ $a$ có độ dài bằng chỉ số của từ. Câu ngắn nên chỉ cần tách từ rồi chuyển đổi.
>
> Khi kiểm tra nguyên âm, xét chữ cái đầu sau khi chuyển về chữ thường. Từ thứ $i$ (đánh số từ 1) được thêm $i$ chữ $a$; sau đó nối các từ bằng dấu cách.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def toGoatLatin(self, sentence: str) -> str:
        ans = []
        for i, word in enumerate(sentence.split()):
            if word.lower()[0] not in ['a', 'e', 'i', 'o', 'u']:
                word = word[1:] + word[0]
            word += 'ma'
            word += 'a' * (i + 1)
            ans.append(word)
        return ' '.join(ans)
```

#### Java

```java
class Solution {
    public String toGoatLatin(String sentence) {
        List<String> ans = new ArrayList<>();
        Set<Character> vowels
            = new HashSet<>(Arrays.asList('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'));
        int i = 1;
        for (String word : sentence.split(" ")) {
            StringBuilder t = new StringBuilder();
            if (!vowels.contains(word.charAt(0))) {
                t.append(word.substring(1));
                t.append(word.charAt(0));
            } else {
                t.append(word);
            }
            t.append("ma");
            for (int j = 0; j < i; ++j) {
                t.append("a");
            }
            ++i;
            ans.add(t.toString());
        }
        return String.join(" ", ans);
    }
}
```

#### TypeScript

```ts
function toGoatLatin(sentence: string): string {
    return sentence
        .split(' ')
        .map((s, i) => {
            let startStr: string;
            if (/[aeiou]/i.test(s[0])) {
                startStr = s;
            } else {
                startStr = s.slice(1) + s[0];
            }
            return `${startStr}ma${'a'.repeat(i + 1)}`;
        })
        .join(' ');
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn to_goat_latin(sentence: String) -> String {
        let set: HashSet<&char> = ['a', 'e', 'i', 'o', 'u'].into_iter().collect();
        sentence
            .split_whitespace()
            .enumerate()
            .map(|(i, s)| {
                let first = char::from(s.as_bytes()[0]);
                let mut res = if set.contains(&first.to_ascii_lowercase()) {
                    s.to_string()
                } else {
                    s[1..].to_string() + &first.to_string()
                };
                res.push_str("ma");
                res.push_str(&"a".repeat(i + 1));
                res
            })
            .into_iter()
            .collect::<Vec<String>>()
            .join(" ")
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
