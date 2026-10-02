---
comments: true
difficulty: Medium
tags:
    - String
---

<!-- problem:start -->

# [468. Validate IP Address](https://leetcode.com/problems/validate-ip-address)

[中文文档](/solution/0400-0499/0468.Validate%20IP%20Address/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>queryIP</code>, trả về <code>&quot;IPv4&quot;</code> nếu IP là địa chỉ IPv4 hợp lệ, <code>&quot;IPv6&quot;</code> nếu IP là địa chỉ IPv6 hợp lệ, hoặc <code>&quot;Neither&quot;</code> nếu IP không phải địa chỉ hợp lệ thuộc loại nào.</p>

<p>Địa chỉ <strong>IPv4 hợp lệ</strong> có dạng <code>&quot;x<sub>1</sub>.x<sub>2</sub>.x<sub>3</sub>.x<sub>4</sub>&quot;</code>, trong đó <code>0 &lt;= x<sub>i</sub> &lt;= 255</code> và <code>x<sub>i</sub></code> <strong>không được có</strong> số 0 ở đầu. Ví dụ, <code>&quot;192.168.1.1&quot;</code> và <code>&quot;192.168.1.0&quot;</code> là địa chỉ IPv4 hợp lệ, còn <code>&quot;192.168.01.1&quot;</code>, <code>&quot;192.168.1.00&quot;</code> và <code>&quot;192.168@1.1&quot;</code> là địa chỉ IPv4 không hợp lệ.</p>

<p>Địa chỉ <strong>IPv6 hợp lệ</strong> có dạng <code>&quot;x<sub>1</sub>:x<sub>2</sub>:x<sub>3</sub>:x<sub>4</sub>:x<sub>5</sub>:x<sub>6</sub>:x<sub>7</sub>:x<sub>8</sub>&quot;</code>, trong đó:</p>

<ul>
	<li><code>1 &lt;= x<sub>i</sub>.length &lt;= 4</code></li>
	<li><code>x<sub>i</sub></code> là <strong>chuỗi thập lục phân</strong>, có thể gồm chữ số, chữ cái tiếng Anh viết thường (<code>&#39;a&#39;</code> đến <code>&#39;f&#39;</code>) và chữ cái tiếng Anh viết hoa (<code>&#39;A&#39;</code> đến <code>&#39;F&#39;</code>).</li>
	<li><code>x<sub>i</sub></code> được phép có số 0 ở đầu.</li>
</ul>

<p>Ví dụ, &quot;<code>2001:0db8:85a3:0000:0000:8a2e:0370:7334&quot;</code> và &quot;<code>2001:db8:85a3:0:0:8A2E:0370:7334&quot;</code> là địa chỉ IPv6 hợp lệ, còn &quot;<code>2001:0db8:85a3::8A2E:037j:7334&quot;</code> và &quot;<code>02001:0db8:85a3:0000:0000:8a2e:0370:7334&quot;</code> là địa chỉ IPv6 không hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> queryIP = &quot;172.16.254.1&quot;
<strong>Đầu ra:</strong> &quot;IPv4&quot;
<strong>Giải thích:</strong> Đây là địa chỉ IPv4 hợp lệ, trả về &quot;IPv4&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> queryIP = &quot;2001:0db8:85a3:0:0:8A2E:0370:7334&quot;
<strong>Đầu ra:</strong> &quot;IPv6&quot;
<strong>Giải thích:</strong> Đây là địa chỉ IPv6 hợp lệ, trả về &quot;IPv6&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> queryIP = &quot;256.256.256.256&quot;
<strong>Đầu ra:</strong> &quot;Neither&quot;
<strong>Giải thích:</strong> Đây không phải địa chỉ IPv4 hay IPv6 hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>queryIP</code> chỉ gồm chữ cái tiếng Anh, chữ số và các ký tự <code>&#39;.&#39;</code>, <code>&#39;:&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Phân loại thành IPv4, IPv6 hoặc không thuộc loại nào. Có thể dùng regex, nhưng dễ xử lý số 0 ở đầu, các phần rỗng và tập ký tự hơn bằng cách tách chuỗi rõ ràng.
>
> IPv4: gồm bốn phần tách bởi `.`, không có số 0 ở đầu và mỗi phần là số trong khoảng $0$– $255$. IPv6: gồm tám phần tách bởi `:`, mỗi phần dài từ $1$ đến $4$ ký tự thập lục phân. Các trường hợp còn lại trả về $\texttt{Neither}$.
>
> Kiểm tra IPv4 rồi đến IPv6; hai loại dùng dấu phân tách khác nhau nên không thể cùng hợp lệ. Phần rỗng sẽ không đạt điều kiện về độ dài hoặc ký tự.

<!-- thinking:end -->

Ta có thể định nghĩa hai hàm `isIPv4` và `isIPv6` để xác định một chuỗi có phải địa chỉ IPv4 hoặc IPv6 hợp lệ hay không.

Hàm `isIPv4` được triển khai như sau:

1. Trước tiên, kiểm tra chuỗi `s` có kết thúc bằng `.` không. Nếu có, `s` không phải địa chỉ IPv4 hợp lệ nên trả về `false` ngay.
1. Tách chuỗi `s` theo dấu `.` thành mảng chuỗi `ss`. Nếu `ss` không có đúng `4` phần tử thì `s` không phải địa chỉ IPv4 hợp lệ nên trả về `false` ngay.
1. Với mỗi chuỗi `t` trong mảng `ss`, kiểm tra:
    - Nếu `t` dài hơn `1` ký tự và ký tự đầu tiên của `t` là `0`, `t` không phải địa chỉ IPv4 hợp lệ nên trả về `false` ngay.
    - Nếu `t` không phải số hoặc `t` nằm ngoài khoảng từ `0` đến `255`, `t` không phải địa chỉ IPv4 hợp lệ nên trả về `false` ngay.
1. Nếu không điều kiện nào ở trên đúng, `s` là địa chỉ IPv4 hợp lệ và ta trả về `true`.

Hàm `isIPv6` được triển khai như sau:

1. Trước tiên, kiểm tra chuỗi `s` có kết thúc bằng `:` không. Nếu có, `s` không phải địa chỉ IPv6 hợp lệ nên trả về `false` ngay.
1. Tách chuỗi `s` theo dấu `:` thành mảng chuỗi `ss`. Nếu `ss` không có đúng `8` phần tử thì `s` không phải địa chỉ IPv6 hợp lệ nên trả về `false` ngay.
1. Với mỗi chuỗi `t` trong mảng `ss`, kiểm tra:
    - Nếu độ dài của `t` nhỏ hơn `1` hoặc lớn hơn `4`, `t` không phải địa chỉ IPv6 hợp lệ nên trả về `false` ngay.
    - Nếu các ký tự trong `t` không thuộc các khoảng `0`–`9` hoặc `a`–`f` (không phân biệt chữ hoa, chữ thường), `t` không phải địa chỉ IPv6 hợp lệ nên trả về `false` ngay.
1. Nếu không điều kiện nào ở trên đúng, `s` là địa chỉ IPv6 hợp lệ và ta trả về `true`.

Cuối cùng, gọi `isIPv4` và `isIPv6` để xác định `queryIP` có phải địa chỉ IPv4 hay IPv6 hợp lệ không. Nếu không thuộc loại nào, trả về `Neither`.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài chuỗi `queryIP`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validIPAddress(self, queryIP: str) -> str:
        def is_ipv4(s: str) -> bool:
            ss = s.split(".")
            if len(ss) != 4:
                return False
            for t in ss:
                if len(t) > 1 and t[0] == "0":
                    return False
                if not t.isdigit() or not 0 <= int(t) <= 255:
                    return False
            return True

        def is_ipv6(s: str) -> bool:
            ss = s.split(":")
            if len(ss) != 8:
                return False
            for t in ss:
                if not 1 <= len(t) <= 4:
                    return False
                if not all(c in "0123456789abcdefABCDEF" for c in t):
                    return False
            return True

        if is_ipv4(queryIP):
            return "IPv4"
        if is_ipv6(queryIP):
            return "IPv6"
        return "Neither"
```

#### Java

```java
class Solution {
    public String validIPAddress(String queryIP) {
        if (isIPv4(queryIP)) {
            return "IPv4";
        }
        if (isIPv6(queryIP)) {
            return "IPv6";
        }
        return "Neither";
    }

    private boolean isIPv4(String s) {
        if (s.endsWith(".")) {
            return false;
        }
        String[] ss = s.split("\\.");
        if (ss.length != 4) {
            return false;
        }
        for (String t : ss) {
            if (t.length() == 0 || t.length() > 1 && t.charAt(0) == '0') {
                return false;
            }
            int x = convert(t);
            if (x < 0 || x > 255) {
                return false;
            }
        }
        return true;
    }

    private boolean isIPv6(String s) {
        if (s.endsWith(":")) {
            return false;
        }
        String[] ss = s.split(":");
        if (ss.length != 8) {
            return false;
        }
        for (String t : ss) {
            if (t.length() < 1 || t.length() > 4) {
                return false;
            }
            for (char c : t.toCharArray()) {
                if (!Character.isDigit(c)
                    && !"0123456789abcdefABCDEF".contains(String.valueOf(c))) {
                    return false;
                }
            }
        }
        return true;
    }

    private int convert(String s) {
        int x = 0;
        for (char c : s.toCharArray()) {
            if (!Character.isDigit(c)) {
                return -1;
            }
            x = x * 10 + (c - '0');
            if (x > 255) {
                return x;
            }
        }
        return x;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string validIPAddress(string queryIP) {
        if (isIPv4(queryIP)) {
            return "IPv4";
        }
        if (isIPv6(queryIP)) {
            return "IPv6";
        }
        return "Neither";
    }

private:
    bool isIPv4(const string& s) {
        if (s.empty() || s.back() == '.') {
            return false;
        }
        vector<string> ss = split(s, '.');
        if (ss.size() != 4) {
            return false;
        }
        for (const string& t : ss) {
            if (t.empty() || (t.size() > 1 && t[0] == '0')) {
                return false;
            }
            int x = convert(t);
            if (x < 0 || x > 255) {
                return false;
            }
        }
        return true;
    }

    bool isIPv6(const string& s) {
        if (s.empty() || s.back() == ':') {
            return false;
        }
        vector<string> ss = split(s, ':');
        if (ss.size() != 8) {
            return false;
        }
        for (const string& t : ss) {
            if (t.size() < 1 || t.size() > 4) {
                return false;
            }
            for (char c : t) {
                if (!isxdigit(c)) {
                    return false;
                }
            }
        }
        return true;
    }

    int convert(const string& s) {
        int x = 0;
        for (char c : s) {
            if (!isdigit(c)) {
                return -1;
            }
            x = x * 10 + (c - '0');
            if (x > 255) {
                return x;
            }
        }
        return x;
    }

    vector<string> split(const string& s, char delimiter) {
        vector<string> tokens;
        string token;
        istringstream iss(s);
        while (getline(iss, token, delimiter)) {
            tokens.push_back(token);
        }
        return tokens;
    }
};
```

#### Go

```go
func validIPAddress(queryIP string) string {
	if isIPv4(queryIP) {
		return "IPv4"
	}
	if isIPv6(queryIP) {
		return "IPv6"
	}
	return "Neither"
}

func isIPv4(s string) bool {
	if strings.HasSuffix(s, ".") {
		return false
	}
	ss := strings.Split(s, ".")
	if len(ss) != 4 {
		return false
	}
	for _, t := range ss {
		if len(t) == 0 || (len(t) > 1 && t[0] == '0') {
			return false
		}
		x := convert(t)
		if x < 0 || x > 255 {
			return false
		}
	}
	return true
}

func isIPv6(s string) bool {
	if strings.HasSuffix(s, ":") {
		return false
	}
	ss := strings.Split(s, ":")
	if len(ss) != 8 {
		return false
	}
	for _, t := range ss {
		if len(t) < 1 || len(t) > 4 {
			return false
		}
		for _, c := range t {
			if !unicode.IsDigit(c) && !strings.ContainsRune("0123456789abcdefABCDEF", c) {
				return false
			}
		}
	}
	return true
}

func convert(s string) int {
	x := 0
	for _, c := range s {
		if !unicode.IsDigit(c) {
			return -1
		}
		x = x*10 + int(c-'0')
		if x > 255 {
			return x
		}
	}
	return x
}
```

#### TypeScript

```ts
function validIPAddress(queryIP: string): string {
    if (isIPv4(queryIP)) {
        return 'IPv4';
    }
    if (isIPv6(queryIP)) {
        return 'IPv6';
    }
    return 'Neither';
}

function isIPv4(s: string): boolean {
    if (s.endsWith('.')) {
        return false;
    }
    const ss = s.split('.');
    if (ss.length !== 4) {
        return false;
    }
    for (const t of ss) {
        if (t.length === 0 || (t.length > 1 && t[0] === '0')) {
            return false;
        }
        const x = convert(t);
        if (x < 0 || x > 255) {
            return false;
        }
    }
    return true;
}

function isIPv6(s: string): boolean {
    if (s.endsWith(':')) {
        return false;
    }
    const ss = s.split(':');
    if (ss.length !== 8) {
        return false;
    }
    for (const t of ss) {
        if (t.length < 1 || t.length > 4) {
            return false;
        }
        for (const c of t) {
            if (!isHexDigit(c)) {
                return false;
            }
        }
    }
    return true;
}

function convert(s: string): number {
    let x = 0;
    for (const c of s) {
        if (!isDigit(c)) {
            return -1;
        }
        x = x * 10 + (c.charCodeAt(0) - '0'.charCodeAt(0));
        if (x > 255) {
            return x;
        }
    }
    return x;
}

function isDigit(c: string): boolean {
    return c >= '0' && c <= '9';
}

function isHexDigit(c: string): boolean {
    return (c >= '0' && c <= '9') || (c >= 'a' && c <= 'f') || (c >= 'A' && c <= 'F');
}
```

#### Rust

```rust
impl Solution {
    pub fn valid_ip_address(query_ip: String) -> String {
        if Self::is_ipv4(&query_ip) {
            return "IPv4".to_string();
        }
        if Self::is_ipv6(&query_ip) {
            return "IPv6".to_string();
        }
        "Neither".to_string()
    }

    fn is_ipv4(s: &str) -> bool {
        if s.ends_with('.') {
            return false;
        }
        let ss: Vec<&str> = s.split('.').collect();
        if ss.len() != 4 {
            return false;
        }
        for t in ss {
            if t.is_empty() || (t.len() > 1 && t.starts_with('0')) {
                return false;
            }
            match Self::convert(t) {
                Some(x) if x <= 255 => {
                    continue;
                }
                _ => {
                    return false;
                }
            }
        }
        true
    }

    fn is_ipv6(s: &str) -> bool {
        if s.ends_with(':') {
            return false;
        }
        let ss: Vec<&str> = s.split(':').collect();
        if ss.len() != 8 {
            return false;
        }
        for t in ss {
            if t.len() < 1 || t.len() > 4 {
                return false;
            }
            if !t.chars().all(|c| c.is_digit(16)) {
                return false;
            }
        }
        true
    }

    fn convert(s: &str) -> Option<i32> {
        let mut x = 0;
        for c in s.chars() {
            if !c.is_digit(10) {
                return None;
            }
            x = x * 10 + (c.to_digit(10).unwrap() as i32);
            if x > 255 {
                return Some(x);
            }
        }
        Some(x)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
