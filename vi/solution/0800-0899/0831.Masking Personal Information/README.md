---
comments: true
difficulty: Medium
tags:
    - String
---

<!-- problem:start -->

# [831. Masking Personal Information](https://leetcode.com/problems/masking-personal-information)

[中文文档](/solution/0800-0899/0831.Masking%20Personal%20Information/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi thông tin cá nhân <code>s</code>, biểu diễn một <strong>địa chỉ email</strong> hoặc một <strong>số điện thoại</strong>. Hãy trả về thông tin cá nhân đã được <strong>che bớt</strong> theo các quy tắc dưới đây.</p>

<p><u><strong>Địa chỉ email:</strong></u></p>

<p>Địa chỉ email có dạng:</p>

<ul>
	<li><strong>Tên</strong> gồm <strong>ít nhất</strong> hai chữ cái tiếng Anh viết hoa hoặc viết thường, theo sau là</li>
	<li>Ký hiệu <code>&#39;@&#39;</code>, theo sau là</li>
	<li><strong>Tên miền</strong> gồm các chữ cái tiếng Anh viết hoa hoặc viết thường, có dấu chấm <code>&#39;.&#39;</code> ở giữa (không phải ký tự đầu tiên hay cuối cùng).</li>
</ul>

<p>Để che bớt email:</p>

<ul>
	<li>Các chữ cái viết hoa trong <strong>tên</strong> và <strong>tên miền</strong> phải được chuyển thành chữ thường.</li>
	<li>Các chữ cái ở giữa <strong>tên</strong> (tức tất cả trừ chữ cái đầu và cuối) phải được thay bằng 5 dấu sao <code>&quot;*****&quot;</code>.</li>
</ul>

<p><u><strong>Số điện thoại:</strong></u></p>

<p>Số điện thoại có định dạng như sau:</p>

<ul>
	<li>Số điện thoại gồm 10–13 chữ số.</li>
	<li>10 chữ số cuối tạo thành <strong>số nội hạt</strong>.</li>
	<li>0–3 chữ số ở đầu tạo thành <strong>mã quốc gia</strong>.</li>
	<li>Các <strong>ký tự phân tách</strong> thuộc tập <code>{&#39;+&#39;, &#39;-&#39;, &#39;(&#39;, &#39;)&#39;, &#39; &#39;}</code> có thể được dùng để phân cách các chữ số trên.</li>
</ul>

<p>Để che bớt số điện thoại:</p>

<ul>
	<li>Xóa tất cả <strong>ký tự phân tách</strong>.</li>
	<li>Số điện thoại sau khi che bớt phải có dạng:
	<ul>
		<li><code>&quot;***-***-XXXX&quot;</code> nếu mã quốc gia có 0 chữ số.</li>
		<li><code>&quot;+*-***-***-XXXX&quot;</code> nếu mã quốc gia có 1 chữ số.</li>
		<li><code>&quot;+**-***-***-XXXX&quot;</code> nếu mã quốc gia có 2 chữ số.</li>
		<li><code>&quot;+***-***-***-XXXX&quot;</code> nếu mã quốc gia có 3 chữ số.</li>
	</ul>
	</li>
	<li><code>&quot;XXXX&quot;</code> là 4 chữ số cuối của <strong>số nội hạt</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;LeetCode@LeetCode.com&quot;
<strong>Đầu ra:</strong> &quot;l*****e@leetcode.com&quot;
<strong>Giải thích:</strong> s là địa chỉ email.
Tên và tên miền được chuyển thành chữ thường, còn phần giữa tên được thay bằng 5 dấu sao.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;AB@qq.com&quot;
<strong>Đầu ra:</strong> &quot;a*****b@qq.com&quot;
<strong>Giải thích:</strong> s là địa chỉ email.
Tên và tên miền được chuyển thành chữ thường, còn phần giữa tên được thay bằng 5 dấu sao.
Lưu ý rằng dù &quot;ab&quot; chỉ có 2 ký tự, phần giữa vẫn phải được thay bằng 5 dấu sao.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1(234)567-890&quot;
<strong>Đầu ra:</strong> &quot;***-***-7890&quot;
<strong>Giải thích:</strong> s là số điện thoại.
Có 10 chữ số nên số nội hạt gồm 10 chữ số và mã quốc gia gồm 0 chữ số.
Do đó, số sau khi che bớt là &quot;***-***-7890&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>s</code> là email hoặc số điện thoại <strong>hợp lệ</strong>.</li>
	<li>Nếu <code>s</code> là email:
	<ul>
		<li><code>8 &lt;= s.length &lt;= 40</code></li>
		<li><code>s</code> gồm các chữ cái tiếng Anh viết hoa và viết thường, đúng một ký hiệu <code>&#39;@&#39;</code> và một ký hiệu <code>&#39;.&#39;</code>.</li>
	</ul>
	</li>
	<li>Nếu <code>s</code> là số điện thoại:
	<ul>
		<li><code>10 &lt;= s.length &lt;= 20</code></li>
		<li><code>s</code> gồm chữ số, dấu cách và các ký hiệu <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code>, <code>&#39;-&#39;</code> và <code>&#39;+&#39;</code>.</li>
	</ul>
	</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Đầu vào là email hoặc số điện thoại hợp lệ, nên chỉ cần kiểm tra ký tự đầu có phải chữ cái để phân loại. Không cần viết parser tổng quát.
>
> Với email, chuyển thành chữ thường rồi giữ lại chữ cái đầu và cuối của tên cùng tên miền. Với số điện thoại, chỉ giữ chữ số: thêm tiền tố mã quốc gia bằng dấu sao nếu cần, còn 4 chữ số cuối của số nội hạt được giữ nguyên.

<!-- thinking:end -->

Theo đề bài, trước tiên ta xác định chuỗi $s$ là email hay số điện thoại rồi xử lý tương ứng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maskPII(self, s: str) -> str:
        if s[0].isalpha():
            s = s.lower()
            return s[0] + '*****' + s[s.find('@') - 1 :]
        s = ''.join(c for c in s if c.isdigit())
        cnt = len(s) - 10
        suf = '***-***-' + s[-4:]
        return suf if cnt == 0 else f'+{"*" * cnt}-{suf}'
```

#### Java

```java
class Solution {
    public String maskPII(String s) {
        if (Character.isLetter(s.charAt(0))) {
            s = s.toLowerCase();
            int i = s.indexOf('@');
            return s.substring(0, 1) + "*****" + s.substring(i - 1);
        }
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) {
                sb.append(c);
            }
        }
        s = sb.toString();
        int cnt = s.length() - 10;
        String suf = "***-***-" + s.substring(s.length() - 4);
        return cnt == 0 ? suf
                        : "+"
                + "*".repeat(cnt) + "-" + suf;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string maskPII(string s) {
        int i = s.find('@');
        if (i != -1) {
            string ans;
            ans += tolower(s[0]);
            ans += "*****";
            for (int j = i - 1; j < s.size(); ++j) {
                ans += tolower(s[j]);
            }
            return ans;
        }
        string t;
        for (char c : s) {
            if (isdigit(c)) {
                t += c;
            }
        }
        int cnt = t.size() - 10;
        string suf = "***-***-" + t.substr(t.size() - 4);
        return cnt == 0 ? suf : "+" + string(cnt, '*') + "-" + suf;
    }
};
```

#### Go

```go
func maskPII(s string) string {
	i := strings.Index(s, "@")
	if i != -1 {
		s = strings.ToLower(s)
		return s[0:1] + "*****" + s[i-1:]
	}
	t := []rune{}
	for _, c := range s {
		if c >= '0' && c <= '9' {
			t = append(t, c)
		}
	}
	s = string(t)
	cnt := len(s) - 10
	suf := "***-***-" + s[len(s)-4:]
	if cnt == 0 {
		return suf
	}
	return "+" + strings.Repeat("*", cnt) + "-" + suf
}
```

#### TypeScript

```ts
function maskPII(s: string): string {
    const i = s.indexOf('@');
    if (i !== -1) {
        let ans = s[0].toLowerCase() + '*****';
        for (let j = i - 1; j < s.length; ++j) {
            ans += s.charAt(j).toLowerCase();
        }
        return ans;
    }
    let t = '';
    for (const c of s) {
        if (/\d/.test(c)) {
            t += c;
        }
    }
    const cnt = t.length - 10;
    const suf = `***-***-${t.substring(t.length - 4)}`;
    return cnt === 0 ? suf : `+${'*'.repeat(cnt)}-${suf}`;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
