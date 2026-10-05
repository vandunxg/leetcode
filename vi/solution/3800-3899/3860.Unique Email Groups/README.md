---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [3860. Unique Email Groups 🔒](https://leetcode.com/problems/unique-email-groups)

[中文文档](/solution/3800-3899/3860.Unique%20Email%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>emails</code>, trong đó mỗi chuỗi là một địa chỉ email hợp lệ.</p>

<p>Hai địa chỉ email thuộc cùng một nhóm nếu <strong>cả</strong> tên cục bộ <strong>đã chuẩn hóa</strong> và tên miền <strong>đã chuẩn hóa</strong> của chúng <strong>giống hệt nhau</strong>.</p>

<p>Các quy tắc chuẩn hóa như sau:</p>

<ul>
	<li>Tên cục bộ là phần nằm <strong>trước</strong> ký hiệu <code>&#39;@&#39;</code>.

    <ul>
    	<li>Bỏ qua mọi dấu chấm <code>&#39;.&#39;</code>.</li>
    	<li>Bỏ qua mọi thứ sau dấu <code>&#39;+&#39;</code> đầu tiên, nếu có.</li>
    	<li>Chuyển thành chữ thường.</li>
    </ul>
    </li>
    <li>Tên miền là phần nằm <strong>sau</strong> ký hiệu <code>&#39;@&#39;</code>.
    <ul>
    	<li>Chuyển thành chữ thường.</li>
    </ul>
    </li>

</ul>

<p>Trả về một số nguyên biểu thị số lượng nhóm email <strong>duy nhất</strong> sau khi chuẩn hóa.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">emails = [&quot;test.email+alex@leetcode.com&quot;, &quot;test.e.mail+bob.cathy@leetcode.com&quot;, &quot;testemail+david@lee.tcode.com&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>
</div>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Email</th>
			<th style="border: 1px solid black;">Tên cục bộ</th>
			<th style="border: 1px solid black;">Tên cục bộ đã chuẩn hóa</th>
			<th style="border: 1px solid black;">Tên miền</th>
			<th style="border: 1px solid black;">Tên miền đã chuẩn hóa</th>
			<th style="border: 1px solid black;">Email cuối cùng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">test.email+alex@leetcode.com</td>
			<td style="border: 1px solid black;">test.email+alex</td>
			<td style="border: 1px solid black;">testemail</td>
			<td style="border: 1px solid black;">leetcode.com</td>
			<td style="border: 1px solid black;">leetcode.com</td>
			<td style="border: 1px solid black;">testemail@leetcode.com</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">test.e.mail+bob.cathy@leetcode.com</td>
			<td style="border: 1px solid black;">test.e.mail+bob.cathy</td>
			<td style="border: 1px solid black;">testemail</td>
			<td style="border: 1px solid black;">leetcode.com</td>
			<td style="border: 1px solid black;">leetcode.com</td>
			<td style="border: 1px solid black;">testemail@leetcode.com</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">testemail+david@lee.tcode.com</td>
			<td style="border: 1px solid black;">testemail+david</td>
			<td style="border: 1px solid black;">testemail</td>
			<td style="border: 1px solid black;">lee.tcode.com</td>
			<td style="border: 1px solid black;">lee.tcode.com</td>
			<td style="border: 1px solid black;">testemail@lee.tcode.com</td>
		</tr>
	</tbody>
</table>

<p>Các email duy nhất là [<code>&quot;testemail@leetcode.com&quot;</code>, <code>&quot;testemail@lee.tcode.com&quot;</code>]. Vì vậy, đáp án là 2.</p>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">emails = [&quot;A@B.com&quot;, &quot;a@b.com&quot;, &quot;ab+xy@b.com&quot;, &quot;a.b@b.com&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Email</th>
			<th style="border: 1px solid black;">Tên cục bộ</th>
			<th style="border: 1px solid black;">Tên cục bộ đã chuẩn hóa</th>
			<th style="border: 1px solid black;">Tên miền</th>
			<th style="border: 1px solid black;">Tên miền đã chuẩn hóa</th>
			<th style="border: 1px solid black;">Email cuối cùng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">A@B.com</td>
			<td style="border: 1px solid black;">A</td>
			<td style="border: 1px solid black;">a</td>
			<td style="border: 1px solid black;">B.com</td>
			<td style="border: 1px solid black;">b.com</td>
			<td style="border: 1px solid black;">a@b.com</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">a@b.com</td>
			<td style="border: 1px solid black;">a</td>
			<td style="border: 1px solid black;">a</td>
			<td style="border: 1px solid black;">b.com</td>
			<td style="border: 1px solid black;">b.com</td>
			<td style="border: 1px solid black;">a@b.com</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">ab+xy@b.com</td>
			<td style="border: 1px solid black;">ab+xy</td>
			<td style="border: 1px solid black;">ab</td>
			<td style="border: 1px solid black;">b.com</td>
			<td style="border: 1px solid black;">b.com</td>
			<td style="border: 1px solid black;">ab@b.com</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">a.b@b.com</td>
			<td style="border: 1px solid black;">a.b</td>
			<td style="border: 1px solid black;">ab</td>
			<td style="border: 1px solid black;">b.com</td>
			<td style="border: 1px solid black;">b.com</td>
			<td style="border: 1px solid black;">ab@b.com</td>
		</tr>
	</tbody>
</table>

<p>Các email duy nhất là [<code>&quot;a@b.com&quot;</code>, <code>&quot;ab@b.com&quot;</code>]. Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">emails = [&quot;a.b+c.d+e@DoMain.com&quot;, &quot;ab+xyz@domain.com&quot;, &quot;ab@domain.com&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Email</th>
			<th style="border: 1px solid black;">Tên cục bộ</th>
			<th style="border: 1px solid black;">Tên cục bộ đã chuẩn hóa</th>
			<th style="border: 1px solid black;">Tên miền</th>
			<th style="border: 1px solid black;">Tên miền đã chuẩn hóa</th>
			<th style="border: 1px solid black;">Email cuối cùng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">a.b+c.d+e@DoMain.com</td>
			<td style="border: 1px solid black;">a.b+c.d+e</td>
			<td style="border: 1px solid black;">ab</td>
			<td style="border: 1px solid black;">DoMain.com</td>
			<td style="border: 1px solid black;">domain.com</td>
			<td style="border: 1px solid black;">ab@domain.com</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">ab+xyz@domain.com</td>
			<td style="border: 1px solid black;">ab+xyz</td>
			<td style="border: 1px solid black;">ab</td>
			<td style="border: 1px solid black;">domain.com</td>
			<td style="border: 1px solid black;">domain.com</td>
			<td style="border: 1px solid black;">ab@domain.com</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">ab@domain.com</td>
			<td style="border: 1px solid black;">ab</td>
			<td style="border: 1px solid black;">ab</td>
			<td style="border: 1px solid black;">domain.com</td>
			<td style="border: 1px solid black;">domain.com</td>
			<td style="border: 1px solid black;">ab@domain.com</td>
		</tr>
	</tbody>
</table>

<p>Tất cả email đều được chuẩn hóa thành <code>&quot;ab@domain.com&quot;</code>. Vì vậy, đáp án là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= emails.length &lt;= 1000</code></li>
	<li><code>1 &lt;= emails[i].length &lt;= 100</code></li>
	<li><code>emails[i]</code> gồm các chữ cái tiếng Anh viết thường và viết hoa, chữ số, cùng các ký tự <code>&#39;.&#39;</code>, <code>&#39;+&#39;</code> và <code>&#39;@&#39;</code>.</li>
	<li>Mỗi <code>emails[i]</code> chứa <strong>chính xác</strong> một ký tự <code>&#39;@&#39;</code>.</li>
	<li>Tất cả tên cục bộ và tên miền đều không rỗng; tên cục bộ không bắt đầu bằng <code>&#39;+&#39;</code>.</li>
	<li>Tên miền kết thúc bằng hậu tố <code>&quot;.com&quot;</code> và chứa ít nhất một ký tự trước <code>&quot;.com&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các email được chuẩn hóa thành cùng một chuỗi sẽ tạo thành một nhóm. Có nhiều nhất $1000$ địa chỉ, nên ta chuẩn hóa từng địa chỉ rồi thêm vào một set.
>
> Xóa dấu chấm khỏi phần cục bộ, cắt tại dấu $+$ đầu tiên, rồi chuyển cả phần cục bộ và tên miền thành chữ thường.
>
> Kích thước của set chính là số nhóm khác nhau.
>
> Mỗi địa chỉ được xử lý đúng một lần, phù hợp với các quy tắc đã cho.

<!-- thinking:end -->

Ta có thể dùng một hash set $\textit{st}$ để lưu kết quả chuẩn hóa của mỗi địa chỉ email. Với mỗi địa chỉ email, ta chuẩn hóa theo yêu cầu của đề bài:

- Tách địa chỉ email thành tên cục bộ và tên miền.
- Với tên cục bộ, xóa mọi dấu chấm `.`, và nếu có dấu cộng `+` thì xóa dấu cộng cùng mọi ký tự phía sau nó. Sau đó chuyển tên cục bộ thành chữ thường.
- Với tên miền, chuyển thành chữ thường.
- Nối tên cục bộ và tên miền đã chuẩn hóa để tạo địa chỉ email đã chuẩn hóa, rồi thêm địa chỉ này vào hash set $\textit{st}$.

Cuối cùng, số phần tử trong hash set $\textit{st}$ chính là số nhóm email duy nhất.

Độ phức tạp thời gian là $O(n \cdot m)$, trong đó $n$ và $m$ lần lượt là số lượng địa chỉ email và độ dài trung bình của mỗi địa chỉ email. Độ phức tạp không gian là $O(n \cdot m)$ trong trường hợp xấu nhất khi tất cả địa chỉ email đều khác nhau.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uniqueEmailGroups(self, emails: list[str]) -> int:
        st = set()
        for email in emails:
            local, domain = email.split("@")
            local = local.split("+")[0].replace(".", "").lower()
            domain = domain.lower()
            normalized = local + domain
            st.add(normalized)
        return len(st)
```

#### Java

```java
class Solution {
    public int uniqueEmailGroups(String[] emails) {
        Set<String> st = new HashSet<>();

        for (String email : emails) {
            String[] parts = email.split("@");
            String local = parts[0];
            String domain = parts[1];

            int plusIndex = local.indexOf('+');
            if (plusIndex != -1) {
                local = local.substring(0, plusIndex);
            }

            local = local.replace(".", "").toLowerCase();
            domain = domain.toLowerCase();

            String normalized = local + domain;
            st.add(normalized);
        }

        return st.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int uniqueEmailGroups(vector<string>& emails) {
        unordered_set<string> st;

        for (auto& email : emails) {
            int atPos = email.find('@');
            string local = email.substr(0, atPos);
            string domain = email.substr(atPos + 1);

            int plusPos = local.find('+');
            if (plusPos != string::npos) {
                local = local.substr(0, plusPos);
            }

            string cleaned;
            for (char c : local) {
                if (c != '.') {
                    cleaned += tolower(c);
                }
            }

            for (char& c : domain) {
                c = tolower(c);
            }

            st.insert(cleaned + domain);
        }

        return st.size();
    }
};
```

#### Go

```go
func uniqueEmailGroups(emails []string) int {
	st := make(map[string]struct{})

	for _, email := range emails {
		parts := strings.Split(email, "@")
		local := parts[0]
		domain := parts[1]

		if idx := strings.Index(local, "+"); idx != -1 {
			local = local[:idx]
		}

		local = strings.ReplaceAll(local, ".", "")
		local = strings.ToLower(local)
		domain = strings.ToLower(domain)

		normalized := local + domain
		st[normalized] = struct{}{}
	}

	return len(st)
}
```

#### TypeScript

```ts
function uniqueEmailGroups(emails: string[]): number {
    const st = new Set<string>();

    for (const email of emails) {
        let [local, domain] = email.split('@');
        local = local.split('+')[0].replace(/\./g, '').toLowerCase();
        domain = domain.toLowerCase();

        const normalized = local + domain;
        st.add(normalized);
    }

    return st.size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
