---
comments: true
difficulty: Medium
rating: 1481
source: Weekly Contest 234 Q3
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [1807. Evaluate the Bracket Pairs of a String](https://leetcode.com/problems/evaluate-the-bracket-pairs-of-a-string)

[中文文档](/solution/1800-1899/1807.Evaluate%20the%20Bracket%20Pairs%20of%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chứa một số cặp ngoặc, mỗi cặp chứa một key <strong>không rỗng</strong>.</p>

<ul>
	<li>Ví dụ, trong chuỗi <code>&quot;(name)is(age)yearsold&quot;</code>, có <strong>hai</strong> cặp ngoặc chứa các key <code>&quot;name&quot;</code> và <code>&quot;age&quot;</code>.</li>
</ul>

<p>Ta biết giá trị của nhiều key. Thông tin này được biểu diễn bởi mảng chuỗi 2D <code>knowledge</code>, trong đó mỗi <code>knowledge[i] = [key<sub>i</sub>, value<sub>i</sub>]</code> cho biết key <code>key<sub>i</sub></code> có giá trị <code>value<sub>i</sub></code>.</p>

<p>Nhiệm vụ là đánh giá <strong>tất cả</strong> các cặp ngoặc. Khi đánh giá một cặp ngoặc chứa key <code>key<sub>i</sub></code>, ta sẽ:</p>

<ul>
	<li>Thay <code>key<sub>i</sub></code> và cặp ngoặc bằng <code>value<sub>i</sub></code> tương ứng với key đó.</li>
	<li>Nếu không biết giá trị của key, thay <code>key<sub>i</sub></code> và cặp ngoặc bằng dấu hỏi <code>&quot;?&quot;</code> (không bao gồm dấu ngoặc kép).</li>
</ul>

<p>Mỗi key xuất hiện nhiều nhất một lần trong <code>knowledge</code>. Trong <code>s</code> không có ngoặc lồng nhau.</p>

<p>Hãy trả về <em>chuỗi kết quả sau khi đánh giá <strong>tất cả</strong> các cặp ngoặc.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(name)is(age)yearsold&quot;, knowledge = [[&quot;name&quot;,&quot;bob&quot;],[&quot;age&quot;,&quot;two&quot;]]
<strong>Đầu ra:</strong> &quot;bobistwoyearsold&quot;
<strong>Giải thích:</strong>
Key &quot;name&quot; có giá trị &quot;bob&quot;, nên thay &quot;(name)&quot; bằng &quot;bob&quot;.
Key &quot;age&quot; có giá trị &quot;two&quot;, nên thay &quot;(age)&quot; bằng &quot;two&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;hi(name)&quot;, knowledge = [[&quot;a&quot;,&quot;b&quot;]]
<strong>Đầu ra:</strong> &quot;hi?&quot;
<strong>Giải thích:</strong> Vì không biết giá trị của key &quot;name&quot;, ta thay &quot;(name)&quot; bằng &quot;?&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(a)(a)(a)aaa&quot;, knowledge = [[&quot;a&quot;,&quot;yes&quot;]]
<strong>Đầu ra:</strong> &quot;yesyesyesaaa&quot;
<strong>Giải thích:</strong> Cùng một key có thể xuất hiện nhiều lần.
Key &quot;a&quot; có giá trị &quot;yes&quot;, nên thay tất cả các lần xuất hiện của &quot;(a)&quot; bằng &quot;yes&quot;.
Lưu ý rằng các chữ &quot;a&quot; không nằm trong cặp ngoặc sẽ không được đánh giá.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= knowledge.length &lt;= 10<sup>5</sup></code></li>
	<li><code>knowledge[i].length == 2</code></li>
	<li><code>1 &lt;= key<sub>i</sub>.length, value<sub>i</sub>.length &lt;= 10</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và dấu ngoặc tròn <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code>.</li>
	<li>Mỗi ngoặc mở <code>&#39;(&#39;</code> trong <code>s</code> đều có ngoặc đóng <code>&#39;)&#39;</code> tương ứng.</li>
	<li>Key trong mỗi cặp ngoặc của <code>s</code> không rỗng.</li>
	<li>Trong <code>s</code> không có cặp ngoặc lồng nhau.</li>
	<li><code>key<sub>i</sub></code> và <code>value<sub>i</sub></code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Mỗi <code>key<sub>i</sub></code> trong <code>knowledge</code> là duy nhất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi key trong ngoặc phải được thay bằng giá trị tương ứng trong knowledge, hoặc bằng `'?'` nếu không tồn tại. Việc duyệt $\textit{knowledge}$ cho từng cặp ngoặc sẽ quá chậm khi cả chuỗi và từ điển đều có thể dài tới $10^5$.
>
> Xây dựng hash map từ $\textit{knowledge}$, sau đó duyệt $s$ từ trái sang phải. Khi gặp `'('`, tìm `')'` tương ứng, tra key và thêm chuỗi thay thế; các ký tự khác được sao chép nguyên trạng. Mỗi ký tự chỉ được xét một số lần không đổi.

<!-- thinking:end -->

Trước tiên, ta dùng hash table $d$ để ghi nhận các cặp key-value trong `knowledge`.

Sau đó, ta duyệt chuỗi $s$. Nếu ký tự hiện tại là ngoặc mở `'('`, ta bắt đầu duyệt từ vị trí hiện tại cho đến khi gặp ngoặc đóng `')'`. Khi đó, chuỗi nằm trong cặp ngoặc là key. Ta tìm giá trị tương ứng của key trong hash table $d$. Nếu tìm thấy, thay phần nằm trong ngoặc bằng giá trị đó; nếu không, thay bằng `'?'`.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(L)$. Trong đó, $n$ và $m$ lần lượt là độ dài chuỗi $s$ và số phần tử trong danh sách `knowledge`, còn $L$ là tổng độ dài của tất cả chuỗi trong `knowledge`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def evaluate(self, s: str, knowledge: List[List[str]]) -> str:
        d = {a: b for a, b in knowledge}
        i, n = 0, len(s)
        ans = []
        while i < n:
            if s[i] == '(':
                j = s.find(')', i + 1)
                ans.append(d.get(s[i + 1 : j], '?'))
                i = j
            else:
                ans.append(s[i])
            i += 1
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String evaluate(String s, List<List<String>> knowledge) {
        Map<String, String> d = new HashMap<>(knowledge.size());
        for (var e : knowledge) {
            d.put(e.get(0), e.get(1));
        }
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == '(') {
                int j = s.indexOf(')', i + 1);
                ans.append(d.getOrDefault(s.substring(i + 1, j), "?"));
                i = j;
            } else {
                ans.append(s.charAt(i));
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string evaluate(string s, vector<vector<string>>& knowledge) {
        unordered_map<string, string> d;
        for (auto& e : knowledge) {
            d[e[0]] = e[1];
        }
        string ans;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == '(') {
                int j = s.find(")", i + 1);
                auto t = s.substr(i + 1, j - i - 1);
                ans += d.count(t) ? d[t] : "?";
                i = j;
            } else {
                ans += s[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func evaluate(s string, knowledge [][]string) string {
	d := map[string]string{}
	for _, v := range knowledge {
		d[v[0]] = v[1]
	}
	var ans strings.Builder
	for i := 0; i < len(s); i++ {
		if s[i] == '(' {
			j := i + 1
			for s[j] != ')' {
				j++
			}
			if v, ok := d[s[i+1:j]]; ok {
				ans.WriteString(v)
			} else {
				ans.WriteByte('?')
			}
			i = j
		} else {
			ans.WriteByte(s[i])
		}
	}
	return ans.String()
}
```

#### TypeScript

```ts
function evaluate(s: string, knowledge: string[][]): string {
    const n = s.length;
    const map = new Map();
    for (const [k, v] of knowledge) {
        map.set(k, v);
    }
    const ans = [];
    let i = 0;
    while (i < n) {
        if (s[i] === '(') {
            const j = s.indexOf(')', i + 1);
            ans.push(map.get(s.slice(i + 1, j)) ?? '?');
            i = j;
        } else {
            ans.push(s[i]);
        }
        i++;
    }
    return ans.join('');
}
```

#### Rust

```rust
use std::collections::HashMap;
impl Solution {
    pub fn evaluate(s: String, knowledge: Vec<Vec<String>>) -> String {
        let s = s.as_bytes();
        let n = s.len();
        let mut map = HashMap::new();
        for v in knowledge.iter() {
            map.insert(&v[0], &v[1]);
        }
        let mut ans = String::new();
        let mut i = 0;
        while i < n {
            if s[i] == b'(' {
                i += 1;
                let mut j = i;
                let mut key = String::new();
                while s[j] != b')' {
                    key.push(s[j] as char);
                    j += 1;
                }
                ans.push_str(map.get(&key).unwrap_or(&&'?'.to_string()));
                i = j;
            } else {
                ans.push(s[i] as char);
            }
            i += 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
