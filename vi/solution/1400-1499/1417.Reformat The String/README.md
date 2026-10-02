---
comments: true
difficulty: Easy
rating: 1241
source: Weekly Contest 185 Q1
tags:
    - String
---

<!-- problem:start -->

# [1417. Reformat The String](https://leetcode.com/problems/reformat-the-string)

[中文文档](/solution/1400-1499/1417.Reformat%20The%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi gồm chữ cái và chữ số <code>s</code>. (<strong>Chuỗi gồm chữ cái và chữ số</strong> là chuỗi chỉ chứa các chữ cái tiếng Anh viết thường và chữ số).</p>

<p>Bạn cần tìm một hoán vị của chuỗi sao cho không có chữ cái nào đứng ngay sau một chữ cái khác và không có chữ số nào đứng ngay sau một chữ số khác. Nói cách khác, hai ký tự liền kề bất kỳ không được cùng loại.</p>

<p>Trả về <em>chuỗi sau khi định dạng lại</em> hoặc trả về <strong>chuỗi rỗng</strong> nếu không thể định dạng lại chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a0b1c2&quot;
<strong>Đầu ra:</strong> &quot;0a1b2c&quot;
<strong>Giải thích:</strong> Không có hai ký tự liền kề nào cùng loại trong &quot;0a1b2c&quot;. &quot;a0b1c2&quot;, &quot;0a1b2c&quot;, &quot;0c2a1b&quot; cũng là các hoán vị hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> &quot;leetcode&quot; chỉ có các ký tự, nên không thể tách chúng bằng chữ số.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1229857369&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> &quot;1229857369&quot; chỉ có các chữ số, nên không thể tách chúng bằng các ký tự.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và/hoặc chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chữ cái và chữ số phải xen kẽ. Tách hai loại này; nếu số lượng của chúng chênh lệch quá $1$ thì không tồn tại cách sắp xếp.
>
> Đặt loại có nhiều phần tử hơn lên trước, ghép xen kẽ hai danh sách, rồi thêm ký tự còn lại. Vì $n\le 500$, duyệt tuyến tính là đủ.

<!-- thinking:end -->

Ta phân loại tất cả ký tự trong chuỗi $s$ thành hai nhóm: "chữ số" và "chữ cái", rồi đưa chúng lần lượt vào hai mảng $a$ và $b$.

So sánh độ dài của $a$ và $b$. Nếu độ dài của $a$ nhỏ hơn $b$, hoán đổi $a$ và $b$. Sau đó kiểm tra độ chênh lệch độ dài; nếu lớn hơn $1$, trả về chuỗi rỗng.

Tiếp theo, duyệt đồng thời hai mảng, lần lượt thêm các ký tự từ $a$ và $b$ vào kết quả. Sau khi duyệt xong, nếu $a$ dài hơn $b$, thêm ký tự cuối cùng của $a$ vào kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reformat(self, s: str) -> str:
        a = [c for c in s if c.islower()]
        b = [c for c in s if c.isdigit()]
        if abs(len(a) - len(b)) > 1:
            return ''
        if len(a) < len(b):
            a, b = b, a
        ans = []
        for x, y in zip(a, b):
            ans.append(x + y)
        if len(a) > len(b):
            ans.append(a[-1])
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String reformat(String s) {
        StringBuilder a = new StringBuilder();
        StringBuilder b = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) {
                a.append(c);
            } else {
                b.append(c);
            }
        }
        int m = a.length(), n = b.length();
        if (Math.abs(m - n) > 1) {
            return "";
        }
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < Math.min(m, n); ++i) {
            if (m > n) {
                ans.append(a.charAt(i));
                ans.append(b.charAt(i));
            } else {
                ans.append(b.charAt(i));
                ans.append(a.charAt(i));
            }
        }
        if (m > n) {
            ans.append(a.charAt(m - 1));
        }
        if (m < n) {
            ans.append(b.charAt(n - 1));
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reformat(string s) {
        string a = "", b = "";
        for (char c : s) {
            if (isdigit(c))
                a += c;
            else
                b += c;
        }
        int m = a.size(), n = b.size();
        if (abs(m - n) > 1) return "";
        string ans = "";
        for (int i = 0; i < min(m, n); ++i) {
            if (m > n) {
                ans += a[i];
                ans += b[i];
            } else {
                ans += b[i];
                ans += a[i];
            }
        }
        if (m > n) ans += a[m - 1];
        if (m < n) ans += b[n - 1];
        return ans;
    }
};
```

#### Go

```go
func reformat(s string) string {
	a := []byte{}
	b := []byte{}
	for _, c := range s {
		if unicode.IsLetter(c) {
			a = append(a, byte(c))
		} else {
			b = append(b, byte(c))
		}
	}
	if len(a) < len(b) {
		a, b = b, a
	}
	if len(a)-len(b) > 1 {
		return ""
	}
	var ans strings.Builder
	for i := range b {
		ans.WriteByte(a[i])
		ans.WriteByte(b[i])
	}
	if len(a) > len(b) {
		ans.WriteByte(a[len(a)-1])
	}
	return ans.String()
}
```

#### TypeScript

```ts
function reformat(s: string): string {
    let a: string[] = [];
    let b: string[] = [];

    for (const c of s) {
        if (c >= 'a' && c <= 'z') a.push(c);
        else if (c >= '0' && c <= '9') b.push(c);
    }

    if (Math.abs(a.length - b.length) > 1) {
        return '';
    }

    if (a.length < b.length) {
        [a, b] = [b, a];
    }

    const ans: string[] = [];

    for (let i = 0; i < b.length; i++) {
        ans.push(a[i] + b[i]);
    }

    if (a.length > b.length) {
        ans.push(a[a.length - 1]);
    }

    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn reformat(s: String) -> String {
        let mut a: Vec<char> = Vec::new();
        let mut b: Vec<char> = Vec::new();

        for c in s.chars() {
            if c.is_ascii_lowercase() {
                a.push(c);
            } else if c.is_ascii_digit() {
                b.push(c);
            }
        }

        if (a.len() as i32 - b.len() as i32).abs() > 1 {
            return String::new();
        }

        if a.len() < b.len() {
            std::mem::swap(&mut a, &mut b);
        }

        let mut ans = String::new();

        for i in 0..b.len() {
            ans.push(a[i]);
            ans.push(b[i]);
        }

        if a.len() > b.len() {
            ans.push(a[a.len() - 1]);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
