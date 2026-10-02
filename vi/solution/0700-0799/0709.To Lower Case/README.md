---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [709. To Lower Case](https://leetcode.com/problems/to-lower-case)

[中文文档](/solution/0700-0799/0709.To%20Lower%20Case/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về <em>chuỗi sau khi thay mỗi chữ cái viết hoa bằng chữ cái viết thường tương ứng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Hello&quot;
<strong>Đầu ra:</strong> &quot;hello&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;here&quot;
<strong>Đầu ra:</strong> &quot;here&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;LOVELY&quot;
<strong>Đầu ra:</strong> &quot;lovely&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các ký tự ASCII có thể in được.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuyển chữ cái viết hoa thành viết thường; $n \le 100$. Có thể dùng hàm thư viện hoặc duyệt các ký tự ASCII.
>
> Mã của mỗi chữ cái viết hoa nhỏ hơn mã chữ cái viết thường tương ứng $32$, tức khác nhau ở bit $5$. Thực hiện bitwise OR với $32$ sẽ chuyển chữ cái thành viết thường; các ký tự khác không đổi.
>
> Ánh xạ từng ký tự: nếu là chữ hoa, trả về $\operatorname{ord}(c)\,|\,32$. Độ phức tạp thời gian là $O(n)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def toLowerCase(self, s: str) -> str:
        return "".join([chr(ord(c) | 32) if c.isupper() else c for c in s])
```

#### Java

```java
class Solution {
    public String toLowerCase(String s) {
        char[] cs = s.toCharArray();
        for (int i = 0; i < cs.length; ++i) {
            if (cs[i] >= 'A' && cs[i] <= 'Z') {
                cs[i] |= 32;
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
    string toLowerCase(string s) {
        for (char& c : s) {
            if (c >= 'A' && c <= 'Z') {
                c |= 32;
            }
        }
        return s;
    }
};
```

#### Go

```go
func toLowerCase(s string) string {
	cs := []byte(s)
	for i, c := range cs {
		if c >= 'A' && c <= 'Z' {
			cs[i] |= 32
		}
	}
	return string(cs)
}
```

#### TypeScript

```ts
function toLowerCase(s: string): string {
    return s.toLowerCase();
}
```

#### Rust

```rust
impl Solution {
    pub fn to_lower_case(s: String) -> String {
        s.to_ascii_lowercase()
    }
}
```

#### C

```c
char* toLowerCase(char* s) {
    int n = strlen(s);
    for (int i = 0; i < n; i++) {
        if (s[i] >= 'A' && s[i] <= 'Z') {
            s[i] |= 32;
        }
    }
    return s;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng `isupper` để rẽ nhánh. Ký tự ASCII viết thường đã bật bit $5$, nên thực hiện OR với $32$ không làm thay đổi chúng; vì vậy ta có thể áp dụng phép này đồng loạt.
>
> Phần TypeScript thực hiện OR trên mọi ký tự; phần Rust vẫn kiểm tra khoảng $A$– $Z$ để giữ nguyên các ký tự không phải chữ cái. Cả hai phiên bản đều không gọi API chuyển chữ thường phụ thuộc locale.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function toLowerCase(s: string): string {
    return [...s].map(c => String.fromCharCode(c.charCodeAt(0) | 32)).join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn to_lower_case(s: String) -> String {
        s.as_bytes()
            .iter()
            .map(|&c| char::from(if c >= b'A' && c <= b'Z' { c | 32 } else { c }))
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
