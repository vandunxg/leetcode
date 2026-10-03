---
comments: true
difficulty: Easy
rating: 1268
source: Weekly Contest 300 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [2325. Decode the Message](https://leetcode.com/problems/decode-the-message)

[中文文档](/solution/2300-2399/2325.Decode%20the%20Message/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>key</code> và <code>message</code>, lần lượt biểu thị khóa mã hóa và thông điệp bí mật. Các bước để giải mã <code>message</code> như sau:</p>

<ol>
	<li>Dùng <strong>lần xuất hiện đầu tiên</strong> của tất cả 26 chữ cái tiếng Anh viết thường trong <code>key</code> làm <strong>thứ tự</strong> của bảng thay thế.</li>
	<li>Căn chỉnh bảng thay thế với bảng chữ cái tiếng Anh thông thường.</li>
	<li>Sau đó, mỗi chữ cái trong <code>message</code> được <strong>thay thế</strong> bằng cách sử dụng bảng này.</li>
	<li>Dấu cách <code>&#39; &#39;</code> được giữ nguyên.</li>
</ol>

<ul>
	<li>Ví dụ, với <code>key = &quot;<u><strong>hap</strong></u>p<u><strong>y</strong></u> <u><strong>bo</strong></u>y&quot;</code> (key thực tế sẽ có <strong>ít nhất một</strong> lần xuất hiện của mỗi chữ cái trong bảng chữ cái), ta có một phần của bảng thay thế là (<code>&#39;h&#39; -&gt; &#39;a&#39;</code>, <code>&#39;a&#39; -&gt; &#39;b&#39;</code>, <code>&#39;p&#39; -&gt; &#39;c&#39;</code>, <code>&#39;y&#39; -&gt; &#39;d&#39;</code>, <code>&#39;b&#39; -&gt; &#39;e&#39;</code>, <code>&#39;o&#39; -&gt; &#39;f&#39;</code>).</li>
</ul>

<p>Trả về <em>thông điệp đã giải mã</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2325.Decode%20the%20Message/images/ex1new4.jpg" style="width: 752px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> key = &quot;the quick brown fox jumps over the lazy dog&quot;, message = &quot;vkbs bs t suepuv&quot;
<strong>Đầu ra:</strong> &quot;this is a secret&quot;
<strong>Giải thích:</strong> Sơ đồ phía trên cho thấy bảng thay thế.
Bảng này được tạo ra bằng cách lấy lần xuất hiện đầu tiên của mỗi chữ cái trong &quot;<u><strong>the</strong></u> <u><strong>quick</strong></u> <u><strong>brown</strong></u> <u><strong>f</strong></u>o<u><strong>x</strong></u> <u><strong>j</strong></u>u<u><strong>mps</strong></u> o<u><strong>v</strong></u>er the <u><strong>lazy</strong></u> <u><strong>d</strong></u>o<u><strong>g</strong></u>&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2325.Decode%20the%20Message/images/ex2new.jpg" style="width: 754px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> key = &quot;eljuxhpwnyrdgtqkviszcfmabo&quot;, message = &quot;zwx hnfx lqantp mnoeius ycgk vcnjrdb&quot;
<strong>Đầu ra:</strong> &quot;the five boxing wizards jump quickly&quot;
<strong>Giải thích:</strong> Sơ đồ phía trên cho thấy bảng thay thế.
Bảng này được tạo ra bằng cách lấy lần xuất hiện đầu tiên của mỗi chữ cái trong &quot;<u><strong>eljuxhpwnyrdgtqkviszcfmabo</strong></u>&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>26 &lt;= key.length &lt;= 2000</code></li>
	<li><code>key</code> chỉ gồm các chữ cái tiếng Anh viết thường và <code>&#39; &#39;</code>.</li>
	<li><code>key</code> chứa mỗi chữ cái trong bảng chữ cái tiếng Anh (<code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code>) <strong>ít nhất một lần</strong>.</li>
	<li><code>1 &lt;= message.length &lt;= 2000</code></li>
	<li><code>message</code> chỉ gồm các chữ cái tiếng Anh viết thường và <code>&#39; &#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khóa ánh xạ các chữ cái, theo thứ tự xuất hiện đầu tiên, thành $a,b,c,\ldots$. Cả hai chuỗi đều có độ dài tối đa $2000$, nên chỉ cần một bảng thay thế là đủ.
>
> Ghi lại ký tự bản rõ được gán tại lần xuất hiện đầu tiên của mỗi chữ cái; giữ nguyên dấu cách. Giải mã $message$ bằng cách tra cứu trong bảng thay vì quét lại key.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def decodeMessage(self, key: str, message: str) -> str:
        d = {" ": " "}
        i = 0
        for c in key:
            if c not in d:
                d[c] = ascii_lowercase[i]
                i += 1
        return "".join(d[c] for c in message)
```

#### Java

```java
class Solution {
    public String decodeMessage(String key, String message) {
        char[] d = new char[128];
        d[' '] = ' ';
        for (int i = 0, j = 0; i < key.length(); ++i) {
            char c = key.charAt(i);
            if (d[c] == 0) {
                d[c] = (char) ('a' + j++);
            }
        }
        char[] ans = message.toCharArray();
        for (int i = 0; i < ans.length; ++i) {
            ans[i] = d[ans[i]];
        }
        return String.valueOf(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string decodeMessage(string key, string message) {
        char d[128]{};
        d[' '] = ' ';
        char i = 'a';
        for (char& c : key) {
            if (!d[c]) {
                d[c] = i++;
            }
        }
        for (char& c : message) {
            c = d[c];
        }
        return message;
    }
};
```

#### Go

```go
func decodeMessage(key string, message string) string {
	d := [128]byte{}
	d[' '] = ' '
	for i, j := 0, 0; i < len(key); i++ {
		if d[key[i]] == 0 {
			d[key[i]] = byte('a' + j)
			j++
		}
	}
	ans := []byte(message)
	for i, c := range ans {
		ans[i] = d[c]
	}
	return string(ans)
}
```

#### TypeScript

```ts
function decodeMessage(key: string, message: string): string {
    const d = new Map<string, string>();
    for (const c of key) {
        if (c === ' ' || d.has(c)) {
            continue;
        }
        d.set(c, String.fromCharCode('a'.charCodeAt(0) + d.size));
    }
    d.set(' ', ' ');
    return [...message].map(v => d.get(v)).join('');
}
```

#### Rust

```rust
use std::collections::HashMap;
impl Solution {
    pub fn decode_message(key: String, message: String) -> String {
        let mut d = HashMap::new();
        for c in key.as_bytes() {
            if *c == b' ' || d.contains_key(c) {
                continue;
            }
            d.insert(c, char::from((97 + d.len()) as u8));
        }
        message
            .as_bytes()
            .iter()
            .map(|c| d.get(c).unwrap_or(&' '))
            .collect()
    }
}
```

#### C

```c
char* decodeMessage(char* key, char* message) {
    int m = strlen(key);
    int n = strlen(message);
    char d[26];
    memset(d, ' ', 26);
    for (int i = 0, j = 0; i < m; i++) {
        if (key[i] == ' ' || d[key[i] - 'a'] != ' ') {
            continue;
        }
        d[key[i] - 'a'] = 'a' + j++;
    }
    char* ans = malloc(n + 1);
    for (int i = 0; i < n; i++) {
        ans[i] = message[i] == ' ' ? ' ' : d[message[i] - 'a'];
    }
    ans[n] = '\0';
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
