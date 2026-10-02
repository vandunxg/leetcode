---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [890. Find and Replace Pattern](https://leetcode.com/problems/find-and-replace-pattern)

[中文文档](/solution/0800-0899/0890.Find%20and%20Replace%20Pattern/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách chuỗi <code>words</code> và chuỗi <code>pattern</code>, hãy trả về <em>danh sách các</em> <code>words[i]</code> <em>khớp với</em> <code>pattern</code>. Có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Một từ khớp với pattern nếu tồn tại một phép hoán vị chữ cái <code>p</code> sao cho khi thay mỗi chữ cái <code>x</code> trong pattern bằng <code>p(x)</code>, ta thu được từ đó.</p>

<p>Phép hoán vị chữ cái là một ánh xạ song ánh giữa các chữ cái: mỗi chữ cái được ánh xạ sang một chữ cái khác và không có hai chữ cái nào cùng ánh xạ đến một chữ cái.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;abc&quot;,&quot;deq&quot;,&quot;mee&quot;,&quot;aqq&quot;,&quot;dkd&quot;,&quot;ccc&quot;], pattern = &quot;abb&quot;
<strong>Output:</strong> [&quot;mee&quot;,&quot;aqq&quot;]
<strong>Giải thích:</strong> &quot;mee&quot; khớp với pattern vì tồn tại phép hoán vị {a -&gt; m, b -&gt; e, ...}. 
&quot;ccc&quot; không khớp với pattern vì {a -&gt; c, b -&gt; c, ...} không phải phép hoán vị: a và b cùng ánh xạ đến một chữ cái.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;], pattern = &quot;a&quot;
<strong>Output:</strong> [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length &lt;= 20</code></li>
	<li><code>1 &lt;= words.length &lt;= 50</code></li>
	<li><code>words[i].length == pattern.length</code></li>
	<li><code>pattern</code> và <code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một từ khớp với pattern khi và chỉ khi tồn tại song ánh giữa các chữ cái. Số lượng từ ít và độ dài ngắn, nên chỉ cần kiểm tra ánh xạ một lần cho mỗi từ.
>
> Duyệt hai chuỗi và lưu chỉ số xuất hiện đầu tiên của mỗi chữ cái vào hai mảng; nếu hai chỉ số khác nhau thì các ánh xạ không tương ứng. Lưu ánh xạ theo cả hai chiều giúp loại trường hợp nhiều chữ cái ánh xạ vào cùng một chữ cái.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findAndReplacePattern(self, words: List[str], pattern: str) -> List[str]:
        def match(s, t):
            m1, m2 = [0] * 128, [0] * 128
            for i, (a, b) in enumerate(zip(s, t), 1):
                if m1[ord(a)] != m2[ord(b)]:
                    return False
                m1[ord(a)] = m2[ord(b)] = i
            return True

        return [word for word in words if match(word, pattern)]
```

#### Java

```java
class Solution {
    public List<String> findAndReplacePattern(String[] words, String pattern) {
        List<String> ans = new ArrayList<>();
        for (String word : words) {
            if (match(word, pattern)) {
                ans.add(word);
            }
        }
        return ans;
    }

    private boolean match(String s, String t) {
        int[] m1 = new int[128];
        int[] m2 = new int[128];
        for (int i = 0; i < s.length(); ++i) {
            char c1 = s.charAt(i);
            char c2 = t.charAt(i);
            if (m1[c1] != m2[c2]) {
                return false;
            }
            m1[c1] = i + 1;
            m2[c2] = i + 1;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> findAndReplacePattern(vector<string>& words, string pattern) {
        vector<string> ans;
        auto match = [](string& s, string& t) {
            int m1[128] = {0};
            int m2[128] = {0};
            for (int i = 0; i < s.size(); ++i) {
                if (m1[s[i]] != m2[t[i]]) return 0;
                m1[s[i]] = i + 1;
                m2[t[i]] = i + 1;
            }
            return 1;
        };
        for (auto& word : words)
            if (match(word, pattern)) ans.emplace_back(word);
        return ans;
    }
};
```

#### Go

```go
func findAndReplacePattern(words []string, pattern string) []string {
	match := func(s, t string) bool {
		m1, m2 := make([]int, 128), make([]int, 128)
		for i := 0; i < len(s); i++ {
			if m1[s[i]] != m2[t[i]] {
				return false
			}
			m1[s[i]] = i + 1
			m2[t[i]] = i + 1
		}
		return true
	}
	var ans []string
	for _, word := range words {
		if match(word, pattern) {
			ans = append(ans, word)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findAndReplacePattern(words: string[], pattern: string): string[] {
    return words.filter(word => {
        const map1 = new Map<string, number>();
        const map2 = new Map<string, number>();
        for (let i = 0; i < word.length; i++) {
            if (map1.get(word[i]) !== map2.get(pattern[i])) {
                return false;
            }
            map1.set(word[i], i);
            map2.set(pattern[i], i);
        }
        return true;
    });
}
```

#### Rust

```rust
use std::collections::HashMap;
impl Solution {
    pub fn find_and_replace_pattern(words: Vec<String>, pattern: String) -> Vec<String> {
        let pattern = pattern.as_bytes();
        let n = pattern.len();
        words
            .into_iter()
            .filter(|word| {
                let word = word.as_bytes();
                let mut map1 = HashMap::new();
                let mut map2 = HashMap::new();
                for i in 0..n {
                    if map1.get(&word[i]).unwrap_or(&n) != map2.get(&pattern[i]).unwrap_or(&n) {
                        return false;
                    }
                    map1.insert(word[i], i);
                    map2.insert(pattern[i], i);
                }
                true
            })
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
