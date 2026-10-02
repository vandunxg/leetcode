---
comments: true
difficulty: Medium
rating: 1405
source: Weekly Contest 184 Q3
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1410. HTML Entity Parser](https://leetcode.com/problems/html-entity-parser)

[中文文档](/solution/1400-1499/1410.HTML%20Entity%20Parser/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Trình phân tích HTML entity</strong> là trình phân tích nhận mã HTML làm đầu vào và thay thế tất cả entity của các ký tự đặc biệt bằng chính các ký tự đó.</p>

<p>Các ký tự đặc biệt và entity tương ứng trong HTML là:</p>

<ul>
	<li><strong>Dấu ngoặc kép:</strong> entity là <code>&amp;quot;</code> và ký tự biểu tượng là <code>&quot;</code>.</li>
	<li><strong>Dấu nháy đơn:</strong> entity là <code>&amp;apos;</code> và ký tự biểu tượng là <code>&#39;</code>.</li>
	<li><strong>Dấu và:</strong> entity là <code>&amp;amp;</code> và ký tự biểu tượng là <code>&amp;</code>.</li>
	<li><strong>Dấu lớn hơn:</strong> entity là <code>&amp;gt;</code> và ký tự biểu tượng là <code>&gt;</code>.</li>
	<li><strong>Dấu nhỏ hơn:</strong> entity là <code>&amp;lt;</code> và ký tự biểu tượng là <code>&lt;</code>.</li>
	<li><strong>Dấu gạch chéo:</strong> entity là <code>&amp;frasl;</code> và ký tự biểu tượng là <code>/</code>.</li>
</ul>

<p>Cho chuỗi <code>text</code> làm đầu vào cho trình phân tích HTML, hãy cài đặt trình phân tích entity.</p>

<p>Trả về <em>chuỗi sau khi thay thế các entity bằng các ký tự đặc biệt</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;&amp;amp; is an HTML entity but &amp;ambassador; is not.&quot;
<strong>Đầu ra:</strong> &quot;&amp; is an HTML entity but &amp;ambassador; is not.&quot;
<strong>Giải thích:</strong> Trình phân tích sẽ thay thế entity &amp;amp; bằng &amp;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;and I quote: &amp;quot;...&amp;quot;&quot;
<strong>Đầu ra:</strong> &quot;and I quote: \&quot;...\&quot;&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 10<sup>5</sup></code></li>
	<li>Chuỗi có thể chứa bất kỳ ký tự nào trong toàn bộ 256 ký tự ASCII.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Entity có nhiều nhất $7$ ký tự và đến từ một tập cố định. Với $n\le 10^5$, chỉ cần thử các tiền tố có độ dài từ $1$ đến $7$ tại mỗi chỉ số.
>
> Ánh xạ mỗi entity tới ký tự tương ứng. Khi tìm thấy entity, thêm ký tự thay thế vào kết quả và bỏ qua entity đó; nếu không thì thêm ký tự hiện tại. Cách này cũng xử lý được các trường hợp chồng lấn như `&amp;gt;`.

<!-- thinking:end -->

Ta có thể dùng một hash table để lưu ký tự tương ứng với mỗi entity ký tự. Sau đó, duyệt chuỗi, và khi gặp một entity ký tự, thay thế entity đó bằng ký tự tương ứng.

Độ phức tạp thời gian là $O(n \times l)$, độ phức tạp không gian là $O(l)$. Trong đó, $n$ là độ dài chuỗi và $l$ là tổng độ dài của các entity ký tự.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def entityParser(self, text: str) -> str:
        d = {
            '&quot;': '"',
            '&apos;': "'",
            '&amp;': "&",
            "&gt;": '>',
            "&lt;": '<',
            "&frasl;": '/',
        }
        i, n = 0, len(text)
        ans = []
        while i < n:
            for l in range(1, 8):
                j = i + l
                if text[i:j] in d:
                    ans.append(d[text[i:j]])
                    i = j
                    break
            else:
                ans.append(text[i])
                i += 1
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String entityParser(String text) {
        Map<String, String> d = new HashMap<>();
        d.put("&quot;", "\"");
        d.put("&apos;", "'");
        d.put("&amp;", "&");
        d.put("&gt;", ">");
        d.put("&lt;", "<");
        d.put("&frasl;", "/");
        StringBuilder ans = new StringBuilder();
        int i = 0;
        int n = text.length();
        while (i < n) {
            boolean found = false;
            for (int l = 1; l < 8; ++l) {
                int j = i + l;
                if (j <= n) {
                    String t = text.substring(i, j);
                    if (d.containsKey(t)) {
                        ans.append(d.get(t));
                        i = j;
                        found = true;
                        break;
                    }
                }
            }
            if (!found) {
                ans.append(text.charAt(i++));
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
    string entityParser(string text) {
        unordered_map<string, string> d = {
            {"&quot;", "\""},
            {"&apos;", "'"},
            {"&amp;", "&"},
            {"&gt;", ">"},
            {"&lt;", "<"},
            {"&frasl;", "/"},
        };
        string ans = "";
        int i = 0, n = text.size();
        while (i < n) {
            bool found = false;
            for (int l = 1; l < 8; ++l) {
                int j = i + l;
                if (j <= n) {
                    string t = text.substr(i, l);
                    if (d.count(t)) {
                        ans += d[t];
                        i = j;
                        found = true;
                        break;
                    }
                }
            }
            if (!found) ans += text[i++];
        }
        return ans;
    }
};
```

#### Go

```go
func entityParser(text string) string {
	d := map[string]string{
		"&quot;":  "\"",
		"&apos;":  "'",
		"&amp;":   "&",
		"&gt;":    ">",
		"&lt;":    "<",
		"&frasl;": "/",
	}
	var ans strings.Builder
	i, n := 0, len(text)

	for i < n {
		found := false
		for l := 1; l < 8; l++ {
			j := i + l
			if j <= n {
				t := text[i:j]
				if val, ok := d[t]; ok {
					ans.WriteString(val)
					i = j
					found = true
					break
				}
			}
		}
		if !found {
			ans.WriteByte(text[i])
			i++
		}
	}

	return ans.String()
}
```

#### TypeScript

```ts
function entityParser(text: string): string {
    const d: Record<string, string> = {
        '&quot;': '"',
        '&apos;': "'",
        '&amp;': '&',
        '&gt;': '>',
        '&lt;': '<',
        '&frasl;': '/',
    };

    let ans: string = '';
    let i: number = 0;
    const n: number = text.length;

    while (i < n) {
        let found: boolean = false;
        for (let l: number = 1; l < 8; ++l) {
            const j: number = i + l;
            if (j <= n) {
                const t: string = text.substring(i, j);
                if (d.hasOwnProperty(t)) {
                    ans += d[t];
                    i = j;
                    found = true;
                    break;
                }
            }
        }

        if (!found) {
            ans += text[i++];
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thay thế bằng Regular Expression

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 tự duyệt qua các độ dài tiền tố. Tập entity rất nhỏ, nên ta có thể nối các key thành một regular expression và gọi `replace` để thực hiện cùng phép ánh xạ với ít code hơn.

<!-- thinking:end -->

Lưu ánh xạ entity-ký tự trong một hash table, sau đó xây dựng một regular expression và thay thế tất cả entity trong một lượt.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(l)$, trong đó $n$ là độ dài chuỗi và $l$ là tổng độ dài của các entity.

<!-- tabs:start -->

#### TypeScript

```ts
function entityParser(text: string): string {
    const d: { [key: string]: string } = {
        '&quot;': '"',
        '&apos;': "'",
        '&amp;': '&',
        '&gt;': '>',
        '&lt;': '<',
        '&frasl;': '/',
    };

    const pattern = new RegExp(Object.keys(d).join('|'), 'g');
    return text.replace(pattern, match => d[match]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
