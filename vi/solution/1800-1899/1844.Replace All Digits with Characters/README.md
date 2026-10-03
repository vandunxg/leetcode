---
comments: true
difficulty: Easy
rating: 1300
source: Biweekly Contest 51 Q1
tags:
    - String
---

<!-- problem:start -->

# [1844. Replace All Digits with Characters](https://leetcode.com/problems/replace-all-digits-with-characters)

[中文文档](/solution/1800-1899/1844.Replace%20All%20Digits%20with%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>được đánh chỉ số từ 0</strong> có tên <code>s</code>, trong đó các chỉ số <strong>chẵn</strong> chứa chữ cái tiếng Anh viết thường và các chỉ số <strong>lẻ</strong> chứa chữ số.</p>

<p>Ta cần thực hiện phép toán <code>shift(c, x)</code>, trong đó <code>c</code> là một ký tự và <code>x</code> là một chữ số, trả về ký tự thứ <code>x<sup>th</sup></code> sau <code>c</code>.</p>

<ul>
	<li>Ví dụ, <code>shift(&#39;a&#39;, 5) = &#39;f&#39;</code> và <code>shift(&#39;x&#39;, 0) = &#39;x&#39;</code>.</li>
</ul>

<p>Với mỗi chỉ số <strong>lẻ</strong> <code>i</code>, ta cần thay chữ số <code>s[i]</code> bằng kết quả của phép toán <code>shift(s[i-1], s[i])</code>.</p>

<p>Trả về <code>s</code><em> </em>sau khi thay thế tất cả chữ số. <strong>Đảm bảo</strong> rằng<em> </em><code>shift(s[i-1], s[i])</code><em> </em>sẽ không bao giờ vượt quá<em> </em><code>&#39;z&#39;</code>.</p>

<p><strong>Lưu ý</strong> rằng <code>shift(c, x)</code> <strong>không phải</strong> là hàm được cung cấp sẵn, mà là một phép toán <em>cần được cài đặt</em> trong lời giải.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a1c1e1&quot;
<strong>Đầu ra:</strong> &quot;abcdef&quot;
<strong>Giải thích: </strong>Các chữ số được thay thế như sau:
- s[1] -&gt; shift(&#39;a&#39;,1) = &#39;b&#39;
- s[3] -&gt; shift(&#39;c&#39;,1) = &#39;d&#39;
- s[5] -&gt; shift(&#39;e&#39;,1) = &#39;f&#39;</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a1b2c3d4e&quot;
<strong>Đầu ra:</strong> &quot;abbdcfdhe&quot;
<strong>Giải thích: </strong>Các chữ số được thay thế như sau:
- s[1] -&gt; shift(&#39;a&#39;,1) = &#39;b&#39;
- s[3] -&gt; shift(&#39;b&#39;,2) = &#39;d&#39;
- s[5] -&gt; shift(&#39;c&#39;,3) = &#39;f&#39;
- s[7] -&gt; shift(&#39;d&#39;,4) = &#39;h&#39;</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và chữ số.</li>
	<li><code>shift(s[i-1], s[i]) &lt;= &#39;z&#39;</code> với mọi chỉ số <strong>lẻ</strong> <code>i</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chữ số ở chỉ số lẻ dịch chữ cái ngay trước nó. Các phép thay thế độc lập với nhau.
>
> Duyệt các chỉ số lẻ với bước nhảy $2$ và ghi $\textit{chr}(\textit{ord}(s[i-1])+\textit{digit})$.

<!-- thinking:end -->

Duyệt chuỗi, với các ký tự ở chỉ số lẻ, thay chúng bằng ký tự nằm sau ký tự trước đó một số vị trí tương ứng.

Cuối cùng, trả về chuỗi sau khi thay thế.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Không tính phần bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def replaceDigits(self, s: str) -> str:
        s = list(s)
        for i in range(1, len(s), 2):
            s[i] = chr(ord(s[i - 1]) + int(s[i]))
        return ''.join(s)
```

#### Java

```java
class Solution {
    public String replaceDigits(String s) {
        char[] cs = s.toCharArray();
        for (int i = 1; i < cs.length; i += 2) {
            cs[i] = (char) (cs[i - 1] + (cs[i] - '0'));
        }
        return String.valueOf(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string replaceDigits(string s) {
        int n = s.size();
        for (int i = 1; i < n; i += 2) {
            s[i] = s[i - 1] + s[i] - '0';
        }
        return s;
    }
};
```

#### Go

```go
func replaceDigits(s string) string {
	cs := []byte(s)
	for i := 1; i < len(s); i += 2 {
		cs[i] = cs[i-1] + cs[i] - '0'
	}
	return string(cs)
}
```

#### TypeScript

```ts
function replaceDigits(s: string): string {
    const n = s.length;
    const ans = [...s];
    for (let i = 1; i < n; i += 2) {
        ans[i] = String.fromCharCode(ans[i - 1].charCodeAt(0) + Number(ans[i]));
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn replace_digits(s: String) -> String {
        let n = s.len();
        let mut ans = s.into_bytes();
        let mut i = 1;
        while i < n {
            ans[i] = ans[i - 1] + (ans[i] - b'0');
            i += 2;
        }
        ans.into_iter().map(char::from).collect()
    }
}
```

#### C

```c
char* replaceDigits(char* s) {
    int n = strlen(s);
    for (int i = 1; i < n; i += 2) {
        s[i] = s[i - 1] + s[i] - '0';
    }
    return s;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
