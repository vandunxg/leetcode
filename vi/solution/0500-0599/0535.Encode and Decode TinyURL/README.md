---
comments: true
difficulty: Medium
tags:
    - Design
    - Hash Table
    - String
    - Hash Function
---

<!-- problem:start -->

# [535. Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl)

[中文文档](/solution/0500-0599/0535.Encode%20and%20Decode%20TinyURL/README.md)

## Mô tả

<!-- description:start -->

<blockquote>Lưu ý: Đây là bài bổ trợ cho bài toán <a href="https://leetcode.com/discuss/interview-question/system-design/" target="_blank">System Design</a>: <a href="https://leetcode.com/discuss/interview-question/124658/Design-a-URL-Shortener-(-TinyURL-)-System/" target="_blank">Design TinyURL</a>.</blockquote>

<p>TinyURL là dịch vụ rút gọn URL: bạn nhập một URL như <code>https://leetcode.com/problems/design-tinyurl</code> và dịch vụ trả về URL ngắn như <code>http://tinyurl.com/4e9iAk</code>. Hãy thiết kế một class để mã hóa URL và giải mã tiny URL.</p>

<p>Không có giới hạn về cách thuật toán encode/decode hoạt động. Bạn chỉ cần đảm bảo URL có thể được mã hóa thành tiny URL và tiny URL có thể được giải mã về URL ban đầu.</p>

<p>Hãy triển khai class <code>Solution</code>:</p>

<ul>
	<li><code>Solution()</code> Khởi tạo đối tượng của hệ thống.</li>
	<li><code>String encode(String longUrl)</code> Trả về tiny URL tương ứng với <code>longUrl</code>.</li>
	<li><code>String decode(String shortUrl)</code> Trả về URL dài ban đầu tương ứng với <code>shortUrl</code>. Đảm bảo rằng <code>shortUrl</code> đã được mã hóa bởi chính đối tượng này.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> url = &quot;https://leetcode.com/problems/design-tinyurl&quot;
<strong>Đầu ra:</strong> &quot;https://leetcode.com/problems/design-tinyurl&quot;

<strong>Giải thích:</strong>
Solution obj = new Solution();
string tiny = obj.encode(url); // returns the encoded tiny url.
string ans = obj.decode(tiny); // returns the original url after decoding it.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= url.length &lt;= 10<sup>4</sup></code></li>
	<li>Đảm bảo <code>url</code> là URL hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần một URL ngắn có thể giải mã ngược. Băm thành chuỗi cố định có thể xảy ra collision và cần bảng xử lý xung đột.
>
> Mã ngắn không cần khó đoán, nên chỉ cần một id tăng dần. Map lưu `id -> long URL`; URL ngắn gồm domain và id đó, còn decode đọc segment cuối cùng của path. Cả hai thao tác đều có độ phức tạp hằng số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Codec:
    def __init__(self):
        self.m = defaultdict()
        self.idx = 0
        self.domain = 'https://tinyurl.com/'

    def encode(self, longUrl: str) -> str:
        """Encodes a URL to a shortened URL."""
        self.idx += 1
        self.m[str(self.idx)] = longUrl
        return f'{self.domain}{self.idx}'

    def decode(self, shortUrl: str) -> str:
        """Decodes a shortened URL to its original URL."""
        idx = shortUrl.split('/')[-1]
        return self.m[idx]


# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.decode(codec.encode(url))
```

#### Java

```java
public class Codec {
    private Map<String, String> m = new HashMap<>();
    private int idx = 0;
    private String domain = "https://tinyurl.com/";

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        String v = String.valueOf(++idx);
        m.put(v, longUrl);
        return domain + v;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        int i = shortUrl.lastIndexOf('/') + 1;
        return m.get(shortUrl.substring(i));
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```

#### C++

```cpp
class Solution {
public:
    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        string v = to_string(++idx);
        m[v] = longUrl;
        return domain + v;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        int i = shortUrl.rfind('/') + 1;
        return m[shortUrl.substr(i, shortUrl.size() - i)];
    }

private:
    unordered_map<string, string> m;
    int idx = 0;
    string domain = "https://tinyurl.com/";
};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```

#### Go

```go
type Codec struct {
	m   map[int]string
	idx int
}

func Constructor() Codec {
	m := map[int]string{}
	return Codec{m, 0}
}

// Encodes a URL to a shortened URL.
func (this *Codec) encode(longUrl string) string {
	this.idx++
	this.m[this.idx] = longUrl
	return "https://tinyurl.com/" + strconv.Itoa(this.idx)
}

// Decodes a shortened URL to its original URL.
func (this *Codec) decode(shortUrl string) string {
	i := strings.LastIndexByte(shortUrl, '/')
	v, _ := strconv.Atoi(shortUrl[i+1:])
	return this.m[v]
}

/**
 * Your Codec object will be instantiated and called as such:
 * obj := Constructor();
 * url := obj.encode(longUrl);
 * ans := obj.decode(url);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
