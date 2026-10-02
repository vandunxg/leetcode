---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [929. Unique Email Addresses](https://leetcode.com/problems/unique-email-addresses)

[中文文档](/solution/0900-0999/0929.Unique%20Email%20Addresses/README.md)

## Mô tả

<!-- description:start -->

<p>Mỗi <strong>email hợp lệ</strong> gồm <strong>local name</strong> và <strong>domain name</strong>, ngăn cách bởi ký tự <code>&#39;@&#39;</code>. Ngoài chữ cái viết thường, email có thể chứa một hoặc nhiều ký tự <code>&#39;.&#39;</code> hoặc <code>&#39;+&#39;</code>.</p>

<ul>
	<li>Ví dụ, trong <code>&quot;alice@leetcode.com&quot;</code>, <code>&quot;alice&quot;</code> là <strong>local name</strong>, còn <code>&quot;leetcode.com&quot;</code> là <strong>domain name</strong>.</li>
</ul>

<p>Nếu thêm dấu chấm <code>&#39;.&#39;</code> giữa một số ký tự trong phần <strong>local name</strong> của email, thư gửi đến địa chỉ đó sẽ được chuyển tiếp như khi local name không có dấu chấm. Quy tắc này <strong>không áp dụng</strong> cho <strong>domain name</strong>.</p>

<ul>
	<li>Ví dụ, <code>&quot;alice.z@leetcode.com&quot;</code> và <code>&quot;alicez@leetcode.com&quot;</code> được chuyển tiếp đến cùng một địa chỉ email.</li>
</ul>

<p>Nếu thêm dấu cộng <code>&#39;+&#39;</code> vào <strong>local name</strong>, mọi thứ sau dấu cộng đầu tiên sẽ <strong>bị bỏ qua</strong>. Cách này cho phép lọc một số email. Quy tắc này <strong>không áp dụng</strong> cho <strong>domain name</strong>.</p>

<ul>
	<li>Ví dụ, <code>&quot;m.y+name@email.com&quot;</code> sẽ được chuyển tiếp đến <code>&quot;my@email.com&quot;</code>.</li>
</ul>

<p>Có thể áp dụng đồng thời cả hai quy tắc.</p>

<p>Cho mảng chuỗi <code>emails</code>, trong đó ta gửi một email đến mỗi địa chỉ <code>emails[i]</code>. Trả về <em>số địa chỉ khác nhau thực sự nhận được thư</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> emails = [&quot;test.email+alex@leetcode.com&quot;,&quot;test.e.mail+bob.cathy@leetcode.com&quot;,&quot;testemail+david@lee.tcode.com&quot;]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> &quot;testemail@leetcode.com&quot; và &quot;testemail@lee.tcode.com&quot; là các địa chỉ thực sự nhận thư.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> emails = [&quot;a@leetcode.com&quot;,&quot;b@leetcode.com&quot;,&quot;c@leetcode.com&quot;]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= emails.length &lt;= 100</code></li>
	<li><code>1 &lt;= emails[i].length &lt;= 100</code></li>
	<li><code>emails[i]</code> chỉ gồm chữ cái tiếng Anh viết thường và các ký tự <code>&#39;+&#39;</code>, <code>&#39;.&#39;</code>, <code>&#39;@&#39;</code>.</li>
	<li>Mỗi <code>emails[i]</code> chứa đúng một ký tự <code>&#39;@&#39;</code>.</li>
	<li>Local name và domain name đều không rỗng.</li>
	<li>Local name không bắt đầu bằng ký tự <code>&#39;+&#39;</code>.</li>
	<li>Domain name kết thúc bằng hậu tố <code>&quot;.com&quot;</code>.</li>
	<li>Domain name phải có ít nhất một ký tự trước hậu tố <code>&quot;.com&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ local part được chuẩn hóa: bỏ dấu chấm và bỏ qua mọi ký tự từ dấu `+` đầu tiên trở đi. Các địa chỉ trở nên giống nhau sau khi áp dụng quy tắc này chỉ được tính một lần. Chuẩn hóa từng email rồi lưu vào set; kích thước set là đáp án.

<!-- thinking:end -->

Ta có thể dùng hash table $s$ để lưu các địa chỉ email duy nhất. Sau đó, duyệt mảng $\textit{emails}$. Với mỗi email, tách thành local part và domain part. Xử lý local part bằng cách bỏ mọi dấu chấm và bỏ qua các ký tự sau dấu cộng đầu tiên. Cuối cùng, nối local part đã xử lý với domain part rồi thêm địa chỉ thu được vào hash table $s$.

Cuối cùng, trả về kích thước của hash table $s$.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(L)$, với $L$ là tổng độ dài của tất cả địa chỉ email.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numUniqueEmails(self, emails: List[str]) -> int:
        s = set()
        for email in emails:
            local, domain = email.split("@")
            t = []
            for c in local:
                if c == ".":
                    continue
                if c == "+":
                    break
                t.append(c)
            s.add("".join(t) + "@" + domain)
        return len(s)
```

#### Java

```java
class Solution {
    public int numUniqueEmails(String[] emails) {
        Set<String> s = new HashSet<>();
        for (String email : emails) {
            String[] parts = email.split("@");
            String local = parts[0];
            String domain = parts[1];
            StringBuilder t = new StringBuilder();
            for (char c : local.toCharArray()) {
                if (c == '.') {
                    continue;
                }
                if (c == '+') {
                    break;
                }
                t.append(c);
            }
            s.add(t.toString() + "@" + domain);
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> s;
        for (const string& email : emails) {
            size_t atPos = email.find('@');
            string local = email.substr(0, atPos);
            string domain = email.substr(atPos + 1);
            string t;
            for (char c : local) {
                if (c == '.') {
                    continue;
                }
                if (c == '+') {
                    break;
                }
                t.push_back(c);
            }
            s.insert(t + "@" + domain);
        }
        return s.size();
    }
};
```

#### Go

```go
func numUniqueEmails(emails []string) int {
	s := make(map[string]struct{})
	for _, email := range emails {
		parts := strings.Split(email, "@")
		local := parts[0]
		domain := parts[1]
		var t strings.Builder
		for _, c := range local {
			if c == '.' {
				continue
			}
			if c == '+' {
				break
			}
			t.WriteByte(byte(c))
		}
		s[t.String()+"@"+domain] = struct{}{}
	}
	return len(s)
}
```

#### TypeScript

```ts
function numUniqueEmails(emails: string[]): number {
    const s = new Set<string>();
    for (const email of emails) {
        const [local, domain] = email.split('@');
        let t = '';
        for (const c of local) {
            if (c === '.') {
                continue;
            }
            if (c === '+') {
                break;
            }
            t += c;
        }
        s.add(t + '@' + domain);
    }
    return s.size;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn num_unique_emails(emails: Vec<String>) -> i32 {
        let mut s = HashSet::new();

        for email in emails {
            let parts: Vec<&str> = email.split('@').collect();
            let local = parts[0];
            let domain = parts[1];
            let mut t = String::new();
            for c in local.chars() {
                if c == '.' {
                    continue;
                }
                if c == '+' {
                    break;
                }
                t.push(c);
            }
            s.insert(format!("{}@{}", t, domain));
        }

        s.len() as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {string[]} emails
 * @return {number}
 */
var numUniqueEmails = function (emails) {
    const s = new Set();
    for (const email of emails) {
        const [local, domain] = email.split('@');
        let t = '';
        for (const c of local) {
            if (c === '.') {
                continue;
            }
            if (c === '+') {
                break;
            }
            t += c;
        }
        s.add(t + '@' + domain);
    }
    return s.size;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
