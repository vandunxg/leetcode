---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [966. Vowel Spellchecker](https://leetcode.com/problems/vowel-spellchecker)

[中文文档](/solution/0900-0999/0966.Vowel%20Spellchecker/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>wordlist</code>, hãy triển khai spellchecker để chuyển một từ được query thành từ đúng.</p>

<p>Với một từ <code>query</code>, spellchecker xử lý hai loại lỗi chính tả:</p>

<ul>
	<li>Viết hoa/thường: Nếu query khớp với một từ trong wordlist theo cách <strong>không phân biệt chữ hoa chữ thường</strong>, trả về từ trong wordlist với cách viết hoa/thường như ban đầu.

    <ul>
    	<li>Ví dụ: <code>wordlist = [&quot;yellow&quot;]</code>, <code>query = &quot;YellOw&quot;</code>: <code>correct = &quot;yellow&quot;</code></li>
    	<li>Ví dụ: <code>wordlist = [&quot;Yellow&quot;]</code>, <code>query = &quot;yellow&quot;</code>: <code>correct = &quot;Yellow&quot;</code></li>
    	<li>Ví dụ: <code>wordlist = [&quot;yellow&quot;]</code>, <code>query = &quot;yellow&quot;</code>: <code>correct = &quot;yellow&quot;</code></li>
    </ul>
    </li>
    <li>Lỗi nguyên âm: Nếu thay từng nguyên âm <code>(&#39;a&#39;, &#39;e&#39;, &#39;i&#39;, &#39;o&#39;, &#39;u&#39;)</code> trong query bằng một nguyên âm bất kỳ mà từ thu được khớp với một từ trong wordlist theo cách <strong>không phân biệt chữ hoa chữ thường</strong>, thì trả về từ khớp trong wordlist với cách viết hoa/thường ban đầu.
    <ul>
    	<li>Ví dụ: <code>wordlist = [&quot;YellOw&quot;]</code>, <code>query = &quot;yollow&quot;</code>: <code>correct = &quot;YellOw&quot;</code></li>
    	<li>Ví dụ: <code>wordlist = [&quot;YellOw&quot;]</code>, <code>query = &quot;yeellow&quot;</code>: <code>correct = &quot;&quot;</code> (không khớp)</li>
    	<li>Ví dụ: <code>wordlist = [&quot;YellOw&quot;]</code>, <code>query = &quot;yllw&quot;</code>: <code>correct = &quot;&quot;</code> (không khớp)</li>
    </ul>
    </li>

</ul>

<p>Ngoài ra, spellchecker áp dụng các quy tắc ưu tiên sau:</p>

<ul>
	<li>Nếu query khớp chính xác với một từ trong wordlist (<strong>có phân biệt chữ hoa chữ thường</strong>), hãy trả về chính từ đó.</li>
	<li>Nếu query chỉ khác cách viết hoa/thường so với từ trong wordlist, hãy trả về từ khớp đầu tiên.</li>
	<li>Nếu query chỉ khác lỗi nguyên âm so với từ trong wordlist, hãy trả về từ khớp đầu tiên.</li>
	<li>Nếu query không khớp với từ nào trong wordlist, hãy trả về chuỗi rỗng.</li>
</ul>

<p>Cho một số <code>queries</code>, hãy trả về danh sách từ <code>answer</code>, trong đó <code>answer[i]</code> là từ đúng tương ứng với <code>query = queries[i]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> wordlist = ["KiTe","kite","hare","Hare"], queries = ["kite","Kite","KiTe","Hare","HARE","Hear","hear","keti","keet","keto"]
<strong>Output:</strong> ["kite","KiTe","KiTe","Hare","hare","","","KiTe","","KiTe"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> wordlist = ["yellow"], queries = ["YellOw"]
<strong>Output:</strong> ["yellow"]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= wordlist.length, queries.length &lt;= 5000</code></li>
	<li><code>1 &lt;= wordlist[i].length, queries[i].length &lt;= 7</code></li>
	<li><code>wordlist[i]</code> và <code>queries[i]</code> chỉ gồm các chữ cái tiếng Anh.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Đối chiếu query với wordlist theo thứ tự: khớp chính xác, khớp không phân biệt hoa/thường, rồi khớp không phân biệt nguyên âm; ở mỗi bước chọn từ xuất hiện đầu tiên. Tạo ba bảng: set các từ gốc, từ đầu tiên ứng với dạng chữ thường, và từ đầu tiên ứng với dạng thay nguyên âm bằng `*`. Mỗi query lần lượt tra các bảng theo thứ tự đó.

<!-- thinking:end -->

Ta duyệt $\textit{wordlist}$ và lưu các từ vào hai hash table $\textit{low}$ và $\textit{pat}$ theo quy tắc lần lượt không phân biệt hoa/thường và không phân biệt nguyên âm. Key của $\textit{low}$ là dạng chữ thường của từ; key của $\textit{pat}$ là chuỗi thu được khi thay các nguyên âm của từ bằng `*`, còn value là từ ban đầu. Hash table $\textit{s}$ lưu các từ trong $\textit{wordlist}$.

Ta duyệt $\textit{queries}$. Với mỗi từ $\textit{q}$, nếu $\textit{q}$ có trong $\textit{s}$ thì từ đó khớp chính xác với $\textit{wordlist}$, nên ta thêm ngay $\textit{q}$ vào mảng kết quả $\textit{ans}$. Nếu không, mà dạng chữ thường của $\textit{q}$ có trong $\textit{low}$, thì ta thêm $\textit{low}[q.\text{lower}()]$ vào $\textit{ans}$. Nếu vẫn không khớp, mà chuỗi thu được khi thay các nguyên âm của $\textit{q}$ bằng `*` có trong $\textit{pat}$, ta thêm $\textit{pat}[f(q)]$ vào $\textit{ans}$. Nếu không trường hợp nào khớp, ta thêm chuỗi rỗng vào $\textit{ans}$.

Cuối cùng, trả về mảng kết quả $\textit{ans}$.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là số lượng phần tử trong $\textit{wordlist}$ và $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def spellchecker(self, wordlist: List[str], queries: List[str]) -> List[str]:
        def f(w):
            t = []
            for c in w:
                t.append("*" if c in "aeiou" else c)
            return "".join(t)

        s = set(wordlist)
        low, pat = {}, {}
        for w in wordlist:
            t = w.lower()
            low.setdefault(t, w)
            pat.setdefault(f(t), w)

        ans = []
        for q in queries:
            if q in s:
                ans.append(q)
                continue
            q = q.lower()
            if q in low:
                ans.append(low[q])
                continue
            q = f(q)
            if q in pat:
                ans.append(pat[q])
                continue
            ans.append("")
        return ans
```

#### Java

```java
class Solution {
    public String[] spellchecker(String[] wordlist, String[] queries) {
        Set<String> s = new HashSet<>();
        Map<String, String> low = new HashMap<>();
        Map<String, String> pat = new HashMap<>();
        for (String w : wordlist) {
            s.add(w);
            String t = w.toLowerCase();
            low.putIfAbsent(t, w);
            pat.putIfAbsent(f(t), w);
        }
        int m = queries.length;
        String[] ans = new String[m];
        for (int i = 0; i < m; ++i) {
            String q = queries[i];
            if (s.contains(q)) {
                ans[i] = q;
                continue;
            }
            q = q.toLowerCase();
            if (low.containsKey(q)) {
                ans[i] = low.get(q);
                continue;
            }
            q = f(q);
            if (pat.containsKey(q)) {
                ans[i] = pat.get(q);
                continue;
            }
            ans[i] = "";
        }
        return ans;
    }

    private String f(String w) {
        char[] cs = w.toCharArray();
        for (int i = 0; i < cs.length; ++i) {
            char c = cs[i];
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                cs[i] = '*';
            }
        }
        return String.valueOf(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) {
        unordered_set<string> s(wordlist.begin(), wordlist.end());
        unordered_map<string, string> low;
        unordered_map<string, string> pat;
        auto f = [](string& w) {
            string res;
            for (char& c : w) {
                if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                    res += '*';
                } else {
                    res += c;
                }
            }
            return res;
        };
        for (const auto& w : wordlist) {
            string t = w;
            transform(t.begin(), t.end(), t.begin(), ::tolower);
            if (!low.contains(t)) {
                low[t] = w;
            }
            t = f(t);
            if (!pat.contains(t)) {
                pat[t] = w;
            }
        }
        vector<string> ans;
        for (auto& q : queries) {
            if (s.contains(q)) {
                ans.emplace_back(q);
                continue;
            }
            transform(q.begin(), q.end(), q.begin(), ::tolower);
            if (low.contains(q)) {
                ans.emplace_back(low[q]);
                continue;
            }
            q = f(q);
            if (pat.contains(q)) {
                ans.emplace_back(pat[q]);
                continue;
            }
            ans.emplace_back("");
        }
        return ans;
    }
};
```

#### Go

```go
func spellchecker(wordlist []string, queries []string) (ans []string) {
	s := map[string]bool{}
	low := map[string]string{}
	pat := map[string]string{}
	f := func(w string) string {
		res := []byte(w)
		for i := range res {
			if res[i] == 'a' || res[i] == 'e' || res[i] == 'i' || res[i] == 'o' || res[i] == 'u' {
				res[i] = '*'
			}
		}
		return string(res)
	}
	for _, w := range wordlist {
		s[w] = true
		t := strings.ToLower(w)
		if _, ok := low[t]; !ok {
			low[t] = w
		}
		if _, ok := pat[f(t)]; !ok {
			pat[f(t)] = w
		}
	}
	for _, q := range queries {
		if s[q] {
			ans = append(ans, q)
			continue
		}
		q = strings.ToLower(q)
		if s, ok := low[q]; ok {
			ans = append(ans, s)
			continue
		}
		q = f(q)
		if s, ok := pat[q]; ok {
			ans = append(ans, s)
			continue
		}
		ans = append(ans, "")
	}
	return
}
```

#### TypeScript

```ts
function spellchecker(wordlist: string[], queries: string[]): string[] {
    const s = new Set(wordlist);
    const low = new Map<string, string>();
    const pat = new Map<string, string>();

    const f = (w: string): string => {
        let res = '';
        for (const c of w) {
            if ('aeiou'.includes(c)) {
                res += '*';
            } else {
                res += c;
            }
        }
        return res;
    };

    for (const w of wordlist) {
        let t = w.toLowerCase();
        if (!low.has(t)) {
            low.set(t, w);
        }
        t = f(t);
        if (!pat.has(t)) {
            pat.set(t, w);
        }
    }

    const ans: string[] = [];
    for (let q of queries) {
        if (s.has(q)) {
            ans.push(q);
            continue;
        }
        q = q.toLowerCase();
        if (low.has(q)) {
            ans.push(low.get(q)!);
            continue;
        }
        q = f(q);
        if (pat.has(q)) {
            ans.push(pat.get(q)!);
            continue;
        }
        ans.push('');
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::{HashSet, HashMap};

impl Solution {
    pub fn spellchecker(wordlist: Vec<String>, queries: Vec<String>) -> Vec<String> {
        let s: HashSet<String> = wordlist.iter().cloned().collect();
        let mut low: HashMap<String, String> = HashMap::new();
        let mut pat: HashMap<String, String> = HashMap::new();

        let f = |w: &str| -> String {
            w.chars()
                .map(|c| match c {
                    'a' | 'e' | 'i' | 'o' | 'u' => '*',
                    _ => c,
                })
                .collect()
        };

        for w in &wordlist {
            let mut t = w.to_lowercase();
            if !low.contains_key(&t) {
                low.insert(t.clone(), w.clone());
            }
            t = f(&t);
            if !pat.contains_key(&t) {
                pat.insert(t.clone(), w.clone());
            }
        }

        let mut ans: Vec<String> = Vec::new();
        for query in queries {
            if s.contains(&query) {
                ans.push(query);
                continue;
            }
            let mut q = query.to_lowercase();
            if let Some(v) = low.get(&q) {
                ans.push(v.clone());
                continue;
            }
            q = f(&q);
            if let Some(v) = pat.get(&q) {
                ans.push(v.clone());
                continue;
            }
            ans.push("".to_string());
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
