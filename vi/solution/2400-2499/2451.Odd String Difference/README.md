---
comments: true
difficulty: Easy
rating: 1406
source: Biweekly Contest 90 Q1
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [2451. Odd String Difference](https://leetcode.com/problems/odd-string-difference)

[中文文档](/solution/2400-2499/2451.Odd%20String%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng các chuỗi có cùng độ dài <code>words</code>. Gọi độ dài của mỗi chuỗi là <code>n</code>.</p>

<p>Mỗi chuỗi <code>words[i]</code> có thể được chuyển thành một <strong>mảng số nguyên hiệu</strong> <code>difference[i]</code> có độ dài <code>n - 1</code>, trong đó <code>difference[i][j] = words[i][j+1] - words[i][j]</code> với <code>0 &lt;= j &lt;= n - 2</code>. Lưu ý rằng hiệu giữa hai chữ cái là hiệu giữa <strong>vị trí</strong> của chúng trong bảng chữ cái, tức là vị trí của <code>&#39;a&#39;</code> là <code>0</code>, <code>&#39;b&#39;</code> là <code>1</code> và <code>&#39;z&#39;</code> là <code>25</code>.</p>

<ul>
	<li>Ví dụ, với chuỗi <code>&quot;acb&quot;</code>, mảng số nguyên hiệu là <code>[2 - 0, 1 - 2] = [2, -1]</code>.</li>
</ul>

<p>Tất cả các chuỗi trong words đều có cùng mảng số nguyên hiệu, <strong>ngoại trừ một chuỗi</strong>. Hãy tìm chuỗi đó.</p>

<p>Trả về<em> chuỗi trong </em><code>words</code><em> có <strong>mảng số nguyên hiệu</strong> khác với các chuỗi còn lại.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;adc&quot;,&quot;wzy&quot;,&quot;abc&quot;]
<strong>Đầu ra:</strong> &quot;abc&quot;
<strong>Giải thích:</strong>
- Mảng số nguyên hiệu của &quot;adc&quot; là [3 - 0, 2 - 3] = [3, -1].
- Mảng số nguyên hiệu của &quot;wzy&quot; là [25 - 22, 24 - 25]= [3, -1].
- Mảng số nguyên hiệu của &quot;abc&quot; là [1 - 0, 2 - 1] = [1, 1].
Mảng khác biệt là [1, 1], vì vậy ta trả về chuỗi tương ứng là &quot;abc&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;aaa&quot;,&quot;bob&quot;,&quot;ccc&quot;,&quot;ddd&quot;]
<strong>Đầu ra:</strong> &quot;bob&quot;
<strong>Giải thích:</strong> Tất cả các mảng số nguyên đều là [0, 0], ngoại trừ &quot;bob&quot;, tương ứng với [13, -13].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= words.length &lt;= 100</code></li>
	<li><code>n == words[i].length</code></li>
	<li><code>2 &lt;= n &lt;= 20</code></li>
	<li><code>words[i]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng bằng Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mảng hiệu chính là các khoảng cách ASCII giữa những ký tự kề nhau. Tất cả trừ một chuỗi đều có cùng các khoảng cách này. Ta ánh xạ mỗi tuple hiệu với các chuỗi tương ứng; danh sách có độ dài một chính là đáp án.

<!-- thinking:end -->

Ta sử dụng một hash table $d$ để duy trì ánh xạ giữa mảng hiệu của chuỗi và chính chuỗi đó, trong đó mảng hiệu được tạo thành từ hiệu của các ký tự kề nhau trong chuỗi. Vì đề bài đảm bảo rằng mảng hiệu của tất cả các chuỗi trừ một chuỗi đều giống nhau, ta chỉ cần tìm chuỗi có mảng hiệu khác biệt.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(m + n)$. Ở đây, $m$ và $n$ lần lượt là độ dài của chuỗi và số lượng chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def oddString(self, words: List[str]) -> str:
        d = defaultdict(list)
        for s in words:
            t = tuple(ord(b) - ord(a) for a, b in pairwise(s))
            d[t].append(s)
        return next(ss[0] for ss in d.values() if len(ss) == 1)
```

#### Java

```java
class Solution {
    public String oddString(String[] words) {
        var d = new HashMap<String, List<String>>();
        for (var s : words) {
            int m = s.length();
            var cs = new char[m - 1];
            for (int i = 0; i < m - 1; ++i) {
                cs[i] = (char) (s.charAt(i + 1) - s.charAt(i));
            }
            var t = String.valueOf(cs);
            d.putIfAbsent(t, new ArrayList<>());
            d.get(t).add(s);
        }
        for (var ss : d.values()) {
            if (ss.size() == 1) {
                return ss.get(0);
            }
        }
        return "";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string oddString(vector<string>& words) {
        unordered_map<string, vector<string>> cnt;
        for (auto& w : words) {
            string d;
            for (int i = 0; i < w.size() - 1; ++i) {
                d += (char) (w[i + 1] - w[i]);
                d += ',';
            }
            cnt[d].emplace_back(w);
        }
        for (auto& [_, v] : cnt) {
            if (v.size() == 1) {
                return v[0];
            }
        }
        return "";
    }
};
```

#### Go

```go
func oddString(words []string) string {
	d := map[string][]string{}
	for _, s := range words {
		m := len(s)
		cs := make([]byte, m-1)
		for i := 0; i < m-1; i++ {
			cs[i] = s[i+1] - s[i]
		}
		t := string(cs)
		d[t] = append(d[t], s)
	}
	for _, ss := range d {
		if len(ss) == 1 {
			return ss[0]
		}
	}
	return ""
}
```

#### TypeScript

```ts
function oddString(words: string[]): string {
    const d: Map<string, string[]> = new Map();
    for (const s of words) {
        const cs: number[] = [];
        for (let i = 0; i < s.length - 1; ++i) {
            cs.push(s[i + 1].charCodeAt(0) - s[i].charCodeAt(0));
        }
        const t = cs.join(',');
        if (!d.has(t)) {
            d.set(t, []);
        }
        d.get(t)!.push(s);
    }
    for (const [_, ss] of d) {
        if (ss.length === 1) {
            return ss[0];
        }
    }
    return '';
}
```

#### Rust

```rust
use std::collections::HashMap;
impl Solution {
    pub fn odd_string(words: Vec<String>) -> String {
        let n = words[0].len();
        let mut map: HashMap<String, (bool, usize)> = HashMap::new();
        for (i, word) in words.iter().enumerate() {
            let mut k = String::new();
            for j in 1..n {
                k.push_str(&(word.as_bytes()[j] - word.as_bytes()[j - 1]).to_string());
                k.push(',');
            }
            let new_is_only = !map.contains_key(&k);
            map.insert(k, (new_is_only, i));
        }
        for (is_only, i) in map.values() {
            if *is_only {
                return words[*i].clone();
            }
        }
        String::new()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã sử dụng một tuple làm khóa. Việc mã hóa các khoảng cách thành một chuỗi và băm chuỗi đó cũng tạo ra cùng một nhóm, chỉ thay đổi cách biểu diễn khóa.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn odd_string(words: Vec<String>) -> String {
        let mut h = HashMap::new();

        for w in words {
            let bytes: Vec<i32> = w
                .bytes()
                .zip(w.bytes().skip(1))
                .map(|(current, next)| (next - current) as i32)
                .collect();

            let s: String = bytes.iter().map(|&b| char::from(b as u8)).collect();

            h.entry(s).or_insert(vec![]).push(w);
        }

        for strs in h.values() {
            if strs.len() == 1 {
                return strs[0].clone();
            }
        }

        String::from("")
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
