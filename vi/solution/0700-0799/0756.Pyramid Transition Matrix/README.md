---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Backtracking
---

<!-- problem:start -->

# [756. Pyramid Transition Matrix](https://leetcode.com/problems/pyramid-transition-matrix)

[中文文档](/solution/0700-0799/0756.Pyramid%20Transition%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn xếp các khối để tạo thành một kim tự tháp. Mỗi khối có một màu được biểu diễn bằng một chữ cái. Mỗi hàng có <strong>ít hơn một khối</strong> so với hàng ngay bên dưới và được đặt chính giữa phía trên.</p>

<p>Để kim tự tháp trông đẹp mắt, chỉ cho phép một số <strong>mẫu hình tam giác</strong> nhất định. Mỗi mẫu gồm <strong>một khối</strong> đặt trên <strong>hai khối</strong>. Các mẫu được cho dưới dạng danh sách chuỗi ba ký tự <code>allowed</code>; hai ký tự đầu lần lượt biểu thị khối dưới bên trái và bên phải, còn ký tự thứ ba là khối phía trên.</p>

<ul>
	<li>Ví dụ, <code>&quot;ABC&quot;</code> biểu diễn mẫu tam giác có khối <code>&#39;C&#39;</code> đặt trên khối <code>&#39;A&#39;</code> (bên trái) và khối <code>&#39;B&#39;</code> (bên phải). Mẫu này khác với <code>&quot;BAC&quot;</code>, trong đó <code>&#39;B&#39;</code> ở dưới bên trái còn <code>&#39;A&#39;</code> ở dưới bên phải.</li>
</ul>

<p>Bạn bắt đầu với hàng khối dưới cùng <code>bottom</code>, được cho dưới dạng một chuỗi, và <strong>bắt buộc</strong> phải dùng hàng này làm đáy kim tự tháp.</p>

<p>Cho <code>bottom</code> và <code>allowed</code>, trả về <code>true</code><em> nếu có thể xây kim tự tháp lên đến đỉnh sao cho <strong>mọi mẫu tam giác</strong> trong kim tự tháp đều có trong </em><code>allowed</code><em>; nếu không thì trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0756.Pyramid%20Transition%20Matrix/images/pyramid1-grid.jpg" style="width: 600px; height: 232px;" />
<pre>
<strong>Đầu vào:</strong> bottom = &quot;BCD&quot;, allowed = [&quot;BCC&quot;,&quot;CDE&quot;,&quot;CEA&quot;,&quot;FFF&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các mẫu tam giác được phép được minh họa ở bên phải.
Bắt đầu từ đáy (level 3), ta có thể tạo &quot;CE&quot; ở level 2 rồi tạo &quot;A&quot; ở level 1.
Kim tự tháp có ba mẫu tam giác là &quot;BCC&quot;, &quot;CDE&quot; và &quot;CEA&quot;. Cả ba đều được phép.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0756.Pyramid%20Transition%20Matrix/images/pyramid2-grid.jpg" style="width: 600px; height: 359px;" />
<pre>
<strong>Đầu vào:</strong> bottom = &quot;AAAA&quot;, allowed = [&quot;AAB&quot;,&quot;AAC&quot;,&quot;BCD&quot;,&quot;BBE&quot;,&quot;DEF&quot;]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Các mẫu tam giác được phép được minh họa ở bên phải.
Bắt đầu từ đáy (level 4), có nhiều cách tạo level 3, nhưng dù thử mọi khả năng, ta luôn bị kẹt trước khi tạo được level 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= bottom.length &lt;= 6</code></li>
	<li><code>0 &lt;= allowed.length &lt;= 216</code></li>
	<li><code>allowed[i].length == 3</code></li>
	<li>Các chữ cái trong mọi chuỗi đầu vào thuộc tập <code>{&#39;A&#39;, &#39;B&#39;, &#39;C&#39;, &#39;D&#39;, &#39;E&#39;, &#39;F&#39;}</code>.</li>
	<li>Tất cả giá trị trong <code>allowed</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Hàng đáy có độ dài tối đa $6$. Khi xây từng level, ta có thể gặp lại cùng một chuỗi cần xử lý.
>
> Với mỗi cặp ký tự liền kề, ta có danh sách các khối phía trên được phép; tích Descartes của các danh sách này tạo ra mọi hàng kế tiếp có thể có.
>
> Dùng memoization cho $dfs(s)$: trả về thành công khi độ dài bằng $1$; thất bại nếu có cặp ký tự không có lựa chọn; còn lại thì đệ quy với từng chuỗi tạo từ tích Descartes.

<!-- thinking:end -->

Ta dùng hash table $d$ để lưu các mẫu tam giác được phép. Key là một cặp ký tự, còn value là danh sách ký tự tương ứng, cho biết hai ký tự đó có thể tạo thành mẫu tam giác với mỗi ký tự trong danh sách làm đỉnh.

Bắt đầu từ hàng đáy, với mỗi cặp ký tự liền kề ở từng level, nếu chúng tạo được một mẫu tam giác thì ta thêm ký tự phía trên vào vị trí tương ứng trong danh sách ký tự của level kế tiếp, rồi xử lý đệ quy level đó.

Khi đệ quy đến một ký tự duy nhất, ta đã xây thành công đến đỉnh kim tự tháp và trả về $\textit{true}$. Nếu ở một level nào đó có hai ký tự liền kề không tạo được mẫu tam giác, ta trả về $\textit{false}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pyramidTransition(self, bottom: str, allowed: List[str]) -> bool:
        @cache
        def dfs(s: str) -> bool:
            if len(s) == 1:
                return True
            t = []
            for a, b in pairwise(s):
                cs = d[a, b]
                if not cs:
                    return False
                t.append(cs)
            return any(dfs("".join(nxt)) for nxt in product(*t))

        d = defaultdict(list)
        for a, b, c in allowed:
            d[a, b].append(c)
        return dfs(bottom)
```

#### Java

```java
class Solution {
    private final int[][] d = new int[7][7];
    private final Map<String, Boolean> f = new HashMap<>();

    public boolean pyramidTransition(String bottom, List<String> allowed) {
        for (String s : allowed) {
            int a = s.charAt(0) - 'A', b = s.charAt(1) - 'A';
            d[a][b] |= 1 << (s.charAt(2) - 'A');
        }
        return dfs(bottom, new StringBuilder());
    }

    private boolean dfs(String s, StringBuilder t) {
        if (s.length() == 1) {
            return true;
        }
        if (t.length() + 1 == s.length()) {
            return dfs(t.toString(), new StringBuilder());
        }
        String k = s + "." + t.toString();
        Boolean res = f.get(k);
        if (res != null) {
            return res;
        }
        int a = s.charAt(t.length()) - 'A', b = s.charAt(t.length() + 1) - 'A';
        int cs = d[a][b];
        for (int i = 0; i < 7; ++i) {
            if (((cs >> i) & 1) == 1) {
                t.append((char) ('A' + i));
                if (dfs(s, t)) {
                    f.put(k, true);
                    return true;
                }
                t.setLength(t.length() - 1);
            }
        }
        f.put(k, false);
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int d[7][7];
    unordered_map<string, bool> f;

    bool pyramidTransition(string bottom, vector<string>& allowed) {
        memset(d, 0, sizeof(d));
        for (auto& s : allowed) {
            int a = s[0] - 'A', b = s[1] - 'A';
            d[a][b] |= 1 << (s[2] - 'A');
        }
        return dfs(bottom, "");
    }

    bool dfs(string& s, string t) {
        if (s.size() == 1) {
            return true;
        }
        if (t.size() + 1 == s.size()) {
            return dfs(t, "");
        }
        string k = s + "." + t;
        if (f.contains(k)) {
            return f[k];
        }
        int a = s[t.size()] - 'A', b = s[t.size() + 1] - 'A';
        int cs = d[a][b];
        for (int i = 0; i < 7; ++i) {
            if (cs >> i & 1) {
                if (dfs(s, t + (char) (i + 'A'))) {
                    f[k] = true;
                    return true;
                }
            }
        }
        f[k] = false;
        return false;
    }
};
```

#### Go

```go
func pyramidTransition(bottom string, allowed []string) bool {
	d := make([][]int, 7)
	for i := 0; i < 7; i++ {
		d[i] = make([]int, 7)
	}

	for _, s := range allowed {
		a := int(s[0] - 'A')
		b := int(s[1] - 'A')
		c := int(s[2] - 'A')
		d[a][b] |= 1 << c
	}

	f := make(map[string]bool)

	var dfs func(s string, t []byte) bool
	dfs = func(s string, t []byte) bool {
		if len(s) == 1 {
			return true
		}
		if len(t)+1 == len(s) {
			return dfs(string(t), []byte{})
		}

		key := s + "." + string(t)
		if v, ok := f[key]; ok {
			return v
		}

		i := len(t)
		a := int(s[i] - 'A')
		b := int(s[i+1] - 'A')
		cs := d[a][b]

		for c := 0; c < 7; c++ {
			if (cs>>c)&1 == 1 {
				t = append(t, byte('A'+c))
				if dfs(s, t) {
					f[key] = true
					return true
				}
				t = t[:len(t)-1]
			}
		}

		f[key] = false
		return false
	}

	return dfs(bottom, []byte{})
}
```

#### TypeScript

```ts
function pyramidTransition(bottom: string, allowed: string[]): boolean {
    const d: number[][] = Array.from({ length: 7 }, () => Array(7).fill(0));
    for (const s of allowed) {
        const a = s.charCodeAt(0) - 65;
        const b = s.charCodeAt(1) - 65;
        const c = s.charCodeAt(2) - 65;
        d[a][b] |= 1 << c;
    }

    const f = new Map<string, boolean>();

    const dfs = (s: string, t: string[]): boolean => {
        if (s.length === 1) return true;
        if (t.length + 1 === s.length) {
            return dfs(t.join(''), []);
        }

        const key = s + '.' + t.join('');
        if (f.has(key)) return f.get(key)!;

        const i = t.length;
        const a = s.charCodeAt(i) - 65;
        const b = s.charCodeAt(i + 1) - 65;
        let cs = d[a][b];

        for (let c = 0; c < 7; c++) {
            if ((cs >> c) & 1) {
                t.push(String.fromCharCode(65 + c));
                if (dfs(s, t)) {
                    f.set(key, true);
                    return true;
                }
                t.pop();
            }
        }

        f.set(key, false);
        return false;
    };

    return dfs(bottom, []);
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn pyramid_transition(bottom: String, allowed: Vec<String>) -> bool {
        let mut d = vec![vec![0; 7]; 7];
        for s in allowed {
            let a = (s.as_bytes()[0] - b'A') as usize;
            let b = (s.as_bytes()[1] - b'A') as usize;
            let c = (s.as_bytes()[2] - b'A') as usize;
            d[a][b] |= 1 << c;
        }

        let mut f = HashMap::<String, bool>::new();

        fn dfs(s: &str, t: &mut Vec<u8>, d: &Vec<Vec<i32>>, f: &mut HashMap<String, bool>) -> bool {
            if s.len() == 1 {
                return true;
            }

            if t.len() + 1 == s.len() {
                let next = String::from_utf8_lossy(t).to_string();
                let mut nt = Vec::new();
                return dfs(&next, &mut nt, d, f);
            }

            let key = format!("{}.{}", s, String::from_utf8_lossy(t));
            if let Some(&res) = f.get(&key) {
                return res;
            }

            let i = t.len();
            let a = (s.as_bytes()[i] - b'A') as usize;
            let b = (s.as_bytes()[i + 1] - b'A') as usize;
            let mut cs = d[a][b];

            for c in 0..7 {
                if (cs >> c) & 1 == 1 {
                    t.push(b'A' + c as u8);
                    if dfs(s, t, d, f) {
                        f.insert(key.clone(), true);
                        t.pop();
                        return true;
                    }
                    t.pop();
                }
            }

            f.insert(key, false);
            false
        }

        let mut t = Vec::new();
        dfs(&bottom, &mut t, &d, &mut f)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
