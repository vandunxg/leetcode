---
comments: true
difficulty: Easy
rating: 1257
source: Weekly Contest 170 Q1
tags:
    - String
---

<!-- problem:start -->

# [1309. Decrypt String from Alphabet to Integer Mapping](https://leetcode.com/problems/decrypt-string-from-alphabet-to-integer-mapping)

[中文文档](/solution/1300-1399/1309.Decrypt%20String%20from%20Alphabet%20to%20Integer%20Mapping/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho chuỗi <code>s</code> gồm các chữ số và ký tự <code>&#39;#&#39;</code>. Ta ánh xạ <code>s</code> sang các chữ cái tiếng Anh thường như sau:</p>

<ul>
	<li>Các chữ cái từ <code>&#39;a&#39;</code> đến <code>&#39;i&#39;</code> lần lượt được biểu diễn bằng các số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code>.</li>
	<li>Các chữ cái từ <code>&#39;j&#39;</code> đến <code>&#39;z&#39;</code> lần lượt được biểu diễn bằng các mã từ <code>&#39;10#&#39;</code> đến <code>&#39;26#&#39;</code>.</li>
</ul>

<p>Hãy trả về <em>chuỗi thu được sau khi ánh xạ</em>.</p>

<p>Dữ liệu kiểm thử được tạo sao cho luôn tồn tại duy nhất một cách ánh xạ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;10#11#12&quot;
<strong>Đầu ra:</strong> &quot;jkab&quot;
<strong>Giải thích:</strong> &quot;j&quot; -&gt; &quot;10#&quot; , &quot;k&quot; -&gt; &quot;11#&quot; , &quot;a&quot; -&gt; &quot;1&quot; , &quot;b&quot; -&gt; &quot;2&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1326#&quot;
<strong>Đầu ra:</strong> &quot;acz&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ số và ký tự <code>&#39;#&#39;</code>.</li>
	<li><code>s</code> luôn là chuỗi hợp lệ, có thể ánh xạ được.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các mã ánh xạ có hai độ dài: một chữ số cho $1$– $9$, và ba ký tự kèm `#` cho $10$– $26$. Nếu đọc từng ký tự như một chữ số thì mã `10#` sẽ bị tách sai. Ở mỗi chỉ số, ta nhìn trước hai vị trí: nếu gặp `#` thì lấy hai chữ số, nếu không thì lấy một chữ số; con trỏ tăng lần lượt $3$ hoặc $1$.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quá trình này.

Duyệt chuỗi $s$. Với chỉ số hiện tại $i$, nếu $i + 2 < n$ và $s[i + 2]$ là `#`, hãy chuyển chuỗi con gồm $s[i]$ và $s[i + 1]$ thành số nguyên, cộng với mã ASCII của `a` trừ 1, chuyển kết quả thành ký tự rồi thêm vào mảng kết quả; sau đó tăng $i$ thêm 3. Nếu không, chuyển $s[i]$ thành số nguyên, cộng với mã ASCII của `a` trừ 1, chuyển kết quả thành ký tự rồi thêm vào mảng kết quả; sau đó tăng $i$ thêm 1.

Cuối cùng, chuyển mảng kết quả thành chuỗi rồi trả về.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def freqAlphabets(self, s: str) -> str:
        ans = []
        i, n = 0, len(s)
        while i < n:
            if i + 2 < n and s[i + 2] == "#":
                ans.append(chr(int(s[i : i + 2]) + ord("a") - 1))
                i += 3
            else:
                ans.append(chr(int(s[i]) + ord("a") - 1))
                i += 1
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String freqAlphabets(String s) {
        int i = 0, n = s.length();
        StringBuilder ans = new StringBuilder();
        while (i < n) {
            if (i + 2 < n && s.charAt(i + 2) == '#') {
                ans.append((char) ('a' + Integer.parseInt(s.substring(i, i + 2)) - 1));
                i += 3;
            } else {
                ans.append((char) ('a' + Integer.parseInt(s.substring(i, i + 1)) - 1));
                i++;
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
    string freqAlphabets(string s) {
        string ans = "";
        int i = 0, n = s.size();
        while (i < n) {
            if (i + 2 < n && s[i + 2] == '#') {
                ans += char(stoi(s.substr(i, 2)) + 'a' - 1);
                i += 3;
            } else {
                ans += char(s[i] - '0' + 'a' - 1);
                i += 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func freqAlphabets(s string) string {
	var ans []byte
	for i, n := 0, len(s); i < n; {
		if i+2 < n && s[i+2] == '#' {
			num := (int(s[i])-'0')*10 + int(s[i+1]) - '0'
			ans = append(ans, byte(num+int('a')-1))
			i += 3
		} else {
			num := int(s[i]) - '0'
			ans = append(ans, byte(num+int('a')-1))
			i += 1
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function freqAlphabets(s: string): string {
    const ans: string[] = [];
    for (let i = 0, n = s.length; i < n;) {
        if (i + 2 < n && s[i + 2] === '#') {
            ans.push(String.fromCharCode(96 + +s.slice(i, i + 2)));
            i += 3;
        } else {
            ans.push(String.fromCharCode(96 + +s[i]));
            i++;
        }
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn freq_alphabets(s: String) -> String {
        let s = s.as_bytes();
        let mut ans = String::new();
        let mut i = 0;
        let n = s.len();
        while i < n {
            if i + 2 < n && s[i + 2] == b'#' {
                let num = (s[i] - b'0') * 10 + (s[i + 1] - b'0');
                ans.push((96 + num) as char);
                i += 3;
            } else {
                let num = s[i] - b'0';
                ans.push((96 + num) as char);
                i += 1;
            }
        }
        ans
    }
}
```

#### C

```c
char* freqAlphabets(char* s) {
    int n = strlen(s);
    int i = 0;
    int j = 0;
    char* ans = malloc(sizeof(s) * n);
    while (i < n) {
        int t;
        if (i + 2 < n && s[i + 2] == '#') {
            t = (s[i] - '0') * 10 + s[i + 1];
            i += 3;
        } else {
            t = s[i];
            i += 1;
        }
        ans[j++] = 'a' + t - '1';
    }
    ans[j] = '\0';
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
