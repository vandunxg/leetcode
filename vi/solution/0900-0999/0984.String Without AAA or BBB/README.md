---
comments: true
difficulty: Medium
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [984. String Without AAA or BBB](https://leetcode.com/problems/string-without-aaa-or-bbb)

[中文文档](/solution/0900-0999/0984.String%20Without%20AAA%20or%20BBB/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>a</code> và <code>b</code>, hãy trả về <strong>một</strong> chuỗi <code>s</code> bất kỳ thỏa mãn:</p>

<ul>
	<li><code>s</code> có độ dài <code>a + b</code>, chứa đúng <code>a</code> ký tự <code>&#39;a&#39;</code> và đúng <code>b</code> ký tự <code>&#39;b&#39;</code>,</li>
	<li>Chuỗi con <code>&#39;aaa&#39;</code> không xuất hiện trong <code>s</code>, và</li>
	<li>Chuỗi con <code>&#39;bbb&#39;</code> không xuất hiện trong <code>s</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 1, b = 2
<strong>Đầu ra:</strong> &quot;abb&quot;
<strong>Giải thích:</strong> &quot;abb&quot;, &quot;bab&quot; và &quot;bba&quot; đều là đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 4, b = 1
<strong>Đầu ra:</strong> &quot;aabaa&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= a, b &lt;= 100</code></li>
	<li>Đảm bảo luôn tồn tại chuỗi <code>s</code> như vậy với <code>a</code> và <code>b</code> đã cho.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tạo chuỗi gồm $a$ ký tự `'a'` và $b$ ký tự `'b'`, không chứa `aaa` hay `bbb`. Nên thêm ký tự xuất hiện nhiều hơn theo từng cặp, ngăn cách bằng ký tự ít hơn để tránh tạo thành một dãy quá dài. Thêm `aab` khi $a>b$, `bba` khi $b>a$, `ab` khi hai giá trị bằng nhau, rồi thêm các ký tự còn lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def strWithout3a3b(self, a: int, b: int) -> str:
        ans = []
        while a and b:
            if a > b:
                ans.append('aab')
                a, b = a - 2, b - 1
            elif a < b:
                ans.append('bba')
                a, b = a - 1, b - 2
            else:
                ans.append('ab')
                a, b = a - 1, b - 1
        if a:
            ans.append('a' * a)
        if b:
            ans.append('b' * b)
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String strWithout3a3b(int a, int b) {
        StringBuilder ans = new StringBuilder();
        while (a > 0 && b > 0) {
            if (a > b) {
                ans.append("aab");
                a -= 2;
                b -= 1;
            } else if (a < b) {
                ans.append("bba");
                a -= 1;
                b -= 2;
            } else {
                ans.append("ab");
                --a;
                --b;
            }
        }
        if (a > 0) {
            ans.append("a".repeat(a));
        }
        if (b > 0) {
            ans.append("b".repeat(b));
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string strWithout3a3b(int a, int b) {
        string ans;
        while (a && b) {
            if (a > b) {
                ans += "aab";
                a -= 2;
                b -= 1;
            } else if (a < b) {
                ans += "bba";
                a -= 1;
                b -= 2;
            } else {
                ans += "ab";
                --a;
                --b;
            }
        }
        if (a) ans += string(a, 'a');
        if (b) ans += string(b, 'b');
        return ans;
    }
};
```

#### Go

```go
func strWithout3a3b(a int, b int) string {
	var ans strings.Builder
	for a > 0 && b > 0 {
		if a > b {
			ans.WriteString("aab")
			a -= 2
			b -= 1
		} else if a < b {
			ans.WriteString("bba")
			a -= 1
			b -= 2
		} else {
			ans.WriteString("ab")
			a--
			b--
		}
	}
	if a > 0 {
		ans.WriteString(strings.Repeat("a", a))
	}
	if b > 0 {
		ans.WriteString(strings.Repeat("b", b))
	}
	return ans.String()
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
