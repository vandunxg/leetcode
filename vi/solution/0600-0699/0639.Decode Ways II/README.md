---
comments: true
difficulty: Hard
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [639. Decode Ways II](https://leetcode.com/problems/decode-ways-ii)

[中文文档](/solution/0600-0699/0639.Decode%20Ways%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Một thông điệp gồm các chữ cái từ <code>A-Z</code> có thể được <strong>mã hóa</strong> thành số theo ánh xạ sau:</p>

<pre>
&#39;A&#39; -&gt; &quot;1&quot;
&#39;B&#39; -&gt; &quot;2&quot;
...
&#39;Z&#39; -&gt; &quot;26&quot;
</pre>

<p>Để <strong>giải mã</strong> một thông điệp đã mã hóa, ta nhóm các chữ số rồi ánh xạ ngược lại thành chữ cái (có thể có nhiều cách). Ví dụ, <code>&quot;11106&quot;</code> có thể được ánh xạ thành:</p>

<ul>
	<li><code>&quot;AAJF&quot;</code> với cách nhóm <code>(1 1 10 6)</code></li>
	<li><code>&quot;KJF&quot;</code> với cách nhóm <code>(11 10 6)</code></li>
</ul>

<p>Lưu ý, cách nhóm <code>(1 11 06)</code> không hợp lệ vì <code>&quot;06&quot;</code> không thể ánh xạ thành <code>&#39;F&#39;</code>, do <code>&quot;6&quot;</code> khác với <code>&quot;06&quot;</code>.</p>

<p><strong>Ngoài</strong> ánh xạ trên, thông điệp đã mã hóa còn có thể chứa ký tự <code>&#39;*&#39;</code>, đại diện cho bất kỳ chữ số nào từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code> (không bao gồm <code>&#39;0&#39;</code>). Ví dụ, thông điệp đã mã hóa <code>&quot;1*&quot;</code> có thể đại diện cho một trong các thông điệp <code>&quot;11&quot;</code>, <code>&quot;12&quot;</code>, <code>&quot;13&quot;</code>, <code>&quot;14&quot;</code>, <code>&quot;15&quot;</code>, <code>&quot;16&quot;</code>, <code>&quot;17&quot;</code>, <code>&quot;18&quot;</code> hoặc <code>&quot;19&quot;</code>. Giải mã <code>&quot;1*&quot;</code> tương đương với giải mã <strong>bất kỳ</strong> thông điệp nào mà nó có thể đại diện.</p>

<p>Cho chuỗi <code>s</code> gồm các chữ số và ký tự <code>&#39;*&#39;</code>, hãy trả về <em><strong>số cách</strong> có thể <strong>giải mã</strong> chuỗi đó</em>.</p>

<p>Vì kết quả có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;*&quot;
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Thông điệp đã mã hóa có thể đại diện cho bất kỳ thông điệp nào trong số &quot;1&quot;, &quot;2&quot;, &quot;3&quot;, &quot;4&quot;, &quot;5&quot;, &quot;6&quot;, &quot;7&quot;, &quot;8&quot; hoặc &quot;9&quot;.
Mỗi thông điệp này lần lượt có thể được giải mã thành các chuỗi &quot;A&quot;, &quot;B&quot;, &quot;C&quot;, &quot;D&quot;, &quot;E&quot;, &quot;F&quot;, &quot;G&quot;, &quot;H&quot; và &quot;I&quot;.
Vì vậy, có tổng cộng 9 cách giải mã &quot;*&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1*&quot;
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Thông điệp đã mã hóa có thể đại diện cho bất kỳ thông điệp nào trong số &quot;11&quot;, &quot;12&quot;, &quot;13&quot;, &quot;14&quot;, &quot;15&quot;, &quot;16&quot;, &quot;17&quot;, &quot;18&quot; hoặc &quot;19&quot;.
Mỗi thông điệp đã mã hóa này có 2 cách giải mã (ví dụ, &quot;11&quot; có thể giải mã thành &quot;AA&quot; hoặc &quot;K&quot;).
Vì vậy, có tổng cộng 9 * 2 = 18 cách giải mã &quot;1*&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;2*&quot;
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Thông điệp đã mã hóa có thể đại diện cho bất kỳ thông điệp nào trong số &quot;21&quot;, &quot;22&quot;, &quot;23&quot;, &quot;24&quot;, &quot;25&quot;, &quot;26&quot;, &quot;27&quot;, &quot;28&quot; hoặc &quot;29&quot;.
&quot;21&quot;, &quot;22&quot;, &quot;23&quot;, &quot;24&quot;, &quot;25&quot; và &quot;26&quot; có 2 cách giải mã, còn &quot;27&quot;, &quot;28&quot; và &quot;29&quot; chỉ có 1 cách.
Vì vậy, có tổng cộng (6 * 2) + (3 * 1) = 12 + 3 = 15 cách giải mã &quot;2*&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là một chữ số hoặc <code>&#39;*&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> `*` có thể tạo ra các cách giải mã một chữ số hoặc hai chữ số. Với độ dài $10^5$, không thể dùng backtracking.
>
> Có thể dùng DP tuyến tính như trước: số cách phụ thuộc vào hai trạng thái trước đó. Xét các trường hợp `*` và chữ số để cộng số cách giải mã một hoặc hai chữ số; chỉ cần ba biến luân phiên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        mod = int(1e9 + 7)
        n = len(s)

        # dp[i - 2], dp[i - 1], dp[i]
        a, b, c = 0, 1, 0
        for i in range(1, n + 1):
            # 1 digit
            if s[i - 1] == "*":
                c = 9 * b % mod
            elif s[i - 1] != "0":
                c = b
            else:
                c = 0

            # 2 digits
            if i > 1:
                if s[i - 2] == "*" and s[i - 1] == "*":
                    c = (c + 15 * a) % mod
                elif s[i - 2] == "*":
                    if s[i - 1] > "6":
                        c = (c + a) % mod
                    else:
                        c = (c + 2 * a) % mod
                elif s[i - 1] == "*":
                    if s[i - 2] == "1":
                        c = (c + 9 * a) % mod
                    elif s[i - 2] == "2":
                        c = (c + 6 * a) % mod
                elif (
                    s[i - 2] != "0"
                    and (ord(s[i - 2]) - ord("0")) * 10 + ord(s[i - 1]) - ord("0") <= 26
                ):
                    c = (c + a) % mod

            a, b = b, c

        return c
```

#### Java

```java
class Solution {

    private static final int MOD = 1000000007;

    public int numDecodings(String s) {
        int n = s.length();
        char[] cs = s.toCharArray();

        // dp[i - 2], dp[i - 1], dp[i]
        long a = 0, b = 1, c = 0;
        for (int i = 1; i <= n; i++) {
            // 1 digit
            if (cs[i - 1] == '*') {
                c = 9 * b % MOD;
            } else if (cs[i - 1] != '0') {
                c = b;
            } else {
                c = 0;
            }

            // 2 digits
            if (i > 1) {
                if (cs[i - 2] == '*' && cs[i - 1] == '*') {
                    c = (c + 15 * a) % MOD;
                } else if (cs[i - 2] == '*') {
                    if (cs[i - 1] > '6') {
                        c = (c + a) % MOD;
                    } else {
                        c = (c + 2 * a) % MOD;
                    }
                } else if (cs[i - 1] == '*') {
                    if (cs[i - 2] == '1') {
                        c = (c + 9 * a) % MOD;
                    } else if (cs[i - 2] == '2') {
                        c = (c + 6 * a) % MOD;
                    }
                } else if (cs[i - 2] != '0' && (cs[i - 2] - '0') * 10 + cs[i - 1] - '0' <= 26) {
                    c = (c + a) % MOD;
                }
            }

            a = b;
            b = c;
        }

        return (int) c;
    }
}
```

#### Go

```go
const mod int = 1e9 + 7

func numDecodings(s string) int {
	n := len(s)

	// dp[i - 2], dp[i - 1], dp[i]
	a, b, c := 0, 1, 0
	for i := 1; i <= n; i++ {
		// 1 digit
		if s[i-1] == '*' {
			c = 9 * b % mod
		} else if s[i-1] != '0' {
			c = b
		} else {
			c = 0
		}

		// 2 digits
		if i > 1 {
			if s[i-2] == '*' && s[i-1] == '*' {
				c = (c + 15*a) % mod
			} else if s[i-2] == '*' {
				if s[i-1] > '6' {
					c = (c + a) % mod
				} else {
					c = (c + 2*a) % mod
				}
			} else if s[i-1] == '*' {
				if s[i-2] == '1' {
					c = (c + 9*a) % mod
				} else if s[i-2] == '2' {
					c = (c + 6*a) % mod
				}
			} else if s[i-2] != '0' && (s[i-2]-'0')*10+s[i-1]-'0' <= 26 {
				c = (c + a) % mod
			}
		}

		a, b = b, c
	}
	return c
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
