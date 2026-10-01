---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [290. Word Pattern](https://leetcode.com/problems/word-pattern)

[中文文档](/solution/0200-0299/0290.Word%20Pattern/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>pattern</code> và chuỗi <code>s</code>, hãy kiểm tra xem <code>s</code> có khớp với pattern đó hay không.</p>

<p>Ở đây, <b>khớp</b> nghĩa là khớp hoàn toàn, tức tồn tại một song ánh giữa mỗi chữ cái trong <code>pattern</code> và một từ <b>không rỗng</b> trong <code>s</code>. Cụ thể:</p>

<ul>
	<li>Mỗi chữ cái trong <code>pattern</code> ánh xạ tới <strong>duy nhất</strong> một từ trong <code>s</code>.</li>
	<li>Mỗi từ phân biệt trong <code>s</code> ánh xạ tới <strong>duy nhất</strong> một chữ cái trong <code>pattern</code>.</li>
	<li>Không có hai chữ cái nào ánh xạ tới cùng một từ, và không có hai từ nào ánh xạ tới cùng một chữ cái.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">pattern = &quot;abba&quot;, s = &quot;dog cat cat dog&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thiết lập song ánh như sau:</p>

<ul>
	<li><code>&#39;a&#39;</code> ánh xạ tới <code>&quot;dog&quot;</code>.</li>
	<li><code>&#39;b&#39;</code> ánh xạ tới <code>&quot;cat&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">pattern = &quot;abba&quot;, s = &quot;dog cat cat fish&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">pattern = &quot;aaaa&quot;, s = &quot;dog cat cat dog&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length &lt;= 300</code></li>
	<li><code>pattern</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= s.length &lt;= 3000</code></li>
	<li><code>s</code> chỉ chứa chữ cái tiếng Anh viết thường và dấu cách <code>&#39; &#39;</code>.</li>
	<li><code>s</code> <strong>không chứa</strong> dấu cách ở đầu hoặc cuối.</li>
	<li>Các từ trong <code>s</code> được phân tách bằng <strong>một dấu cách duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Pattern và các từ phải tạo thành một song ánh; nếu độ dài không bằng nhau thì không thể khớp. Tương tự bài toán chuỗi đẳng cấu, ta dùng hai map để lưu ánh xạ từ ký tự sang từ và từ từ sang ký tự.
>
> Nếu ánh xạ hiện tại mâu thuẫn với ánh xạ đã lưu thì trả về false.

<!-- thinking:end -->

Trước tiên, ta tách chuỗi $s$ theo dấu cách thành mảng từ $ws$. Nếu độ dài của $pattern$ và $ws$ khác nhau, trả về `false`. Nếu bằng nhau, ta dùng hai hash table $d_1$ và $d_2$ để lưu ánh xạ giữa từng ký tự và từ trong $pattern$ và $ws$.

Tiếp theo, ta duyệt $pattern$ và $ws$. Với mỗi ký tự $a$ và từ $b$, nếu $a$ đã có ánh xạ trong $d_1$ nhưng từ được ánh xạ không phải $b$, hoặc $b$ đã có ánh xạ trong $d_2$ nhưng ký tự được ánh xạ không phải $a$, thì trả về `false`. Nếu không, lần lượt thêm ánh xạ của $a$ và $b$ vào $d_1$ và $d_2$.

Sau khi duyệt xong, trả về `true`.

Độ phức tạp thời gian là $O(m + n)$ và độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của $pattern$ và chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wordPattern(self, pattern: str, s: str) -> bool:
        ws = s.split()
        if len(pattern) != len(ws):
            return False
        d1 = {}
        d2 = {}
        for a, b in zip(pattern, ws):
            if (a in d1 and d1[a] != b) or (b in d2 and d2[b] != a):
                return False
            d1[a] = b
            d2[b] = a
        return True
```

#### Java

```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        String[] ws = s.split(" ");
        if (pattern.length() != ws.length) {
            return false;
        }
        Map<Character, String> d1 = new HashMap<>();
        Map<String, Character> d2 = new HashMap<>();
        for (int i = 0; i < ws.length; ++i) {
            char a = pattern.charAt(i);
            String b = ws[i];
            if (!d1.getOrDefault(a, b).equals(b) || d2.getOrDefault(b, a) != a) {
                return false;
            }
            d1.put(a, b);
            d2.put(b, a);
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool wordPattern(string pattern, string s) {
        istringstream is(s);
        vector<string> ws;
        while (is >> s) {
            ws.push_back(s);
        }
        if (pattern.size() != ws.size()) {
            return false;
        }
        unordered_map<char, string> d1;
        unordered_map<string, char> d2;
        for (int i = 0; i < ws.size(); ++i) {
            char a = pattern[i];
            string b = ws[i];
            if ((d1.count(a) && d1[a] != b) || (d2.count(b) && d2[b] != a)) {
                return false;
            }
            d1[a] = b;
            d2[b] = a;
        }
        return true;
    }
};
```

#### Go

```go
func wordPattern(pattern string, s string) bool {
	ws := strings.Split(s, " ")
	if len(ws) != len(pattern) {
		return false
	}
	d1 := map[rune]string{}
	d2 := map[string]rune{}
	for i, a := range pattern {
		b := ws[i]
		if v, ok := d1[a]; ok && v != b {
			return false
		}
		if v, ok := d2[b]; ok && v != a {
			return false
		}
		d1[a] = b
		d2[b] = a
	}
	return true
}
```

#### TypeScript

```ts
function wordPattern(pattern: string, s: string): boolean {
    const ws = s.split(' ');
    if (pattern.length !== ws.length) {
        return false;
    }
    const d1 = new Map<string, string>();
    const d2 = new Map<string, string>();
    for (let i = 0; i < pattern.length; ++i) {
        const a = pattern[i];
        const b = ws[i];
        if (d1.has(a) && d1.get(a) !== b) {
            return false;
        }
        if (d2.has(b) && d2.get(b) !== a) {
            return false;
        }
        d1.set(a, b);
        d2.set(b, a);
    }
    return true;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn word_pattern(pattern: String, s: String) -> bool {
        let cs1: Vec<char> = pattern.chars().collect();
        let cs2: Vec<&str> = s.split_whitespace().collect();
        let n = cs1.len();
        if n != cs2.len() {
            return false;
        }
        let mut map1 = HashMap::new();
        let mut map2 = HashMap::new();
        for i in 0..n {
            let c = cs1[i];
            let s = cs2[i];
            if !map1.contains_key(&c) {
                map1.insert(c, i);
            }
            if !map2.contains_key(&s) {
                map2.insert(s, i);
            }
            if map1.get(&c) != map2.get(&s) {
                return false;
            }
        }
        true
    }
}
```

#### C#

```cs
public class Solution {
    public bool WordPattern(string pattern, string s) {
        var ws = s.Split(' ');
        if (pattern.Length != ws.Length) {
            return false;
        }
        var d1 = new Dictionary<char, string>();
        var d2 = new Dictionary<string, char>();
        for (int i = 0; i < ws.Length; ++i) {
            var a = pattern[i];
            var b = ws[i];
            if (d1.ContainsKey(a) && d1[a] != b) {
                return false;
            }
            if (d2.ContainsKey(b) && d2[b] != a) {
                return false;
            }
            d1[a] = b;
            d2[b] = a;
        }
        return true;
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
> Có thể thay hai map bằng cách kiểm tra số ký tự phân biệt trong pattern có bằng số từ phân biệt hay không, rồi chỉ cần lưu một map một chiều.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function wordPattern(pattern: string, s: string): boolean {
    const hash: Record<string, string> = Object.create(null);
    const arr = s.split(/\s+/);

    if (pattern.length !== arr.length || new Set(pattern).size !== new Set(arr).size) {
        return false;
    }

    for (let i = 0; i < pattern.length; i++) {
        hash[pattern[i]] ??= arr[i];
        if (hash[pattern[i]] !== arr[i]) {
            return false;
        }
    }

    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
