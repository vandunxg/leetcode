---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [819. Most Common Word](https://leetcode.com/problems/most-common-word)

[中文文档](/solution/0800-0899/0819.Most%20Common%20Word/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>paragraph</code> và mảng chuỗi các từ bị cấm <code>banned</code>, hãy trả về <em>từ xuất hiện thường xuyên nhất mà không bị cấm</em>. Đảm bảo có <strong>ít nhất một từ</strong> không bị cấm và đáp án là <strong>duy nhất</strong>.</p>

<p>Không phân biệt chữ hoa, chữ thường trong các từ của <code>paragraph</code>; đáp án cần được trả về ở dạng <strong>chữ thường</strong>.</p>

<p><strong>Lưu ý</strong>, từ không chứa các ký hiệu dấu câu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> paragraph = &quot;Bob hit a ball, the hit BALL flew far after it was hit.&quot;, banned = [&quot;hit&quot;]
<strong>Đầu ra:</strong> &quot;ball&quot;
<strong>Giải thích:</strong> 
&quot;hit&quot; xuất hiện 3 lần nhưng là từ bị cấm.
&quot;ball&quot; xuất hiện hai lần (không có từ nào khác xuất hiện nhiều lần như vậy), nên đây là từ không bị cấm xuất hiện thường xuyên nhất trong đoạn văn. 
Lưu ý rằng không phân biệt chữ hoa, chữ thường trong các từ của đoạn văn,
dấu câu bị bỏ qua (kể cả khi đứng sát từ, chẳng hạn &quot;ball,&quot;), 
và &quot;hit&quot; không phải đáp án dù xuất hiện nhiều hơn vì đây là từ bị cấm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> paragraph = &quot;a.&quot;, banned = []
<strong>Đầu ra:</strong> &quot;a&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= paragraph.length &lt;= 1000</code></li>
	<li>paragraph chỉ gồm chữ cái tiếng Anh, dấu cách <code>&#39; &#39;</code> hoặc một trong các ký hiệu: <code>&quot;!?&#39;,;.&quot;</code>.</li>
	<li><code>0 &lt;= banned.length &lt;= 100</code></li>
	<li><code>1 &lt;= banned[i].length &lt;= 10</code></li>
	<li><code>banned[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm tần suất từ trong đoạn văn, bỏ qua dấu câu và các từ bị cấm. Đoạn văn dài tối đa $1000$ ký tự nên chỉ cần chuẩn hóa rồi đếm.
>
> Chuyển văn bản thành chữ thường, tách các từ chỉ gồm chữ cái, rồi trả về từ không bị cấm có tần suất cao nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        s = set(banned)
        p = Counter(re.findall('[a-z]+', paragraph.lower()))
        return next(word for word, _ in p.most_common() if word not in s)
```

#### Java

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

class Solution {
    private static Pattern pattern = Pattern.compile("[a-z]+");

    public String mostCommonWord(String paragraph, String[] banned) {
        Set<String> bannedWords = new HashSet<>();
        for (String word : banned) {
            bannedWords.add(word);
        }
        Map<String, Integer> counter = new HashMap<>();
        Matcher matcher = pattern.matcher(paragraph.toLowerCase());
        while (matcher.find()) {
            String word = matcher.group();
            if (bannedWords.contains(word)) {
                continue;
            }
            counter.put(word, counter.getOrDefault(word, 0) + 1);
        }
        int max = Integer.MIN_VALUE;
        String ans = null;
        for (Map.Entry<String, Integer> entry : counter.entrySet()) {
            if (entry.getValue() > max) {
                max = entry.getValue();
                ans = entry.getKey();
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
    string mostCommonWord(string paragraph, vector<string>& banned) {
        unordered_set<string> s(banned.begin(), banned.end());
        unordered_map<string, int> counter;
        string ans;
        for (int i = 0, mx = 0, n = paragraph.size(); i < n;) {
            if (!isalpha(paragraph[i]) && (++i > 0)) continue;
            int j = i;
            string word;
            while (j < n && isalpha(paragraph[j])) {
                word.push_back(tolower(paragraph[j]));
                ++j;
            }
            i = j + 1;
            if (s.count(word)) continue;
            ++counter[word];
            if (counter[word] > mx) {
                ans = word;
                mx = counter[word];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func mostCommonWord(paragraph string, banned []string) string {
	s := make(map[string]bool)
	for _, w := range banned {
		s[w] = true
	}
	counter := make(map[string]int)
	var ans string
	for i, mx, n := 0, 0, len(paragraph); i < n; {
		if !unicode.IsLetter(rune(paragraph[i])) {
			i++
			continue
		}
		j := i
		var word []byte
		for j < n && unicode.IsLetter(rune(paragraph[j])) {
			word = append(word, byte(unicode.ToLower(rune(paragraph[j]))))
			j++
		}
		i = j + 1
		t := string(word)
		if s[t] {
			continue
		}
		counter[t]++
		if counter[t] > mx {
			ans = t
			mx = counter[t]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function mostCommonWord(paragraph: string, banned: string[]): string {
    const s = paragraph.toLocaleLowerCase();
    const map = new Map<string, number>();
    const set = new Set<string>(banned);
    for (const word of s.split(/[^A-z]/)) {
        if (word === '' || set.has(word)) {
            continue;
        }
        map.set(word, (map.get(word) ?? 0) + 1);
    }
    return [...map.entries()].reduce((r, v) => (v[1] > r[1] ? v : r), ['', 0])[0];
}
```

#### Rust

```rust
use std::collections::{HashMap, HashSet};
impl Solution {
    pub fn most_common_word(mut paragraph: String, banned: Vec<String>) -> String {
        paragraph.make_ascii_lowercase();
        let banned: HashSet<&str> = banned.iter().map(String::as_str).collect();
        let mut map = HashMap::new();
        for word in paragraph.split(|c| !matches!(c, 'a'..='z')) {
            if word.is_empty() || banned.contains(word) {
                continue;
            }
            let val = map.get(&word).unwrap_or(&0) + 1;
            map.insert(word, val);
        }
        map.into_iter()
            .max_by_key(|&(_, v)| v)
            .unwrap()
            .0
            .to_string()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
