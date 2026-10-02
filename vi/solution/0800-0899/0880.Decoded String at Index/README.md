---
comments: true
difficulty: Medium
tags:
    - Stack
    - String
---

<!-- problem:start -->

# [880. Decoded String at Index](https://leetcode.com/problems/decoded-string-at-index)

[中文文档](/solution/0800-0899/0880.Decoded%20String%20at%20Index/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi đã mã hóa <code>s</code>. Để giải mã chuỗi thành một dãy ký tự, đọc chuỗi đã mã hóa lần lượt từng ký tự và thực hiện các bước sau:</p>

<ul>
	<li>Nếu ký tự vừa đọc là chữ cái, ghi chữ cái đó vào dãy.</li>
	<li>Nếu ký tự vừa đọc là chữ số <code>d</code>, ghi thêm tổng cộng <code>d - 1</code> bản sao của toàn bộ dãy hiện tại.</li>
</ul>

<p>Cho số nguyên <code>k</code>, hãy trả về <em>chữ cái thứ </em><code>k<sup>th</sup></code><em> trong chuỗi đã giải mã (đánh số từ </em><strong>1</strong><em>)</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leet2code3&quot;, k = 10
<strong>Đầu ra:</strong> &quot;o&quot;
<strong>Giải thích:</strong> Chuỗi sau khi giải mã là &quot;leetleetcodeleetleetcodeleetleetcode&quot;.
Chữ cái thứ 10 trong chuỗi là &quot;o&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ha22&quot;, k = 5
<strong>Đầu ra:</strong> &quot;h&quot;
<strong>Giải thích:</strong> Chuỗi sau khi giải mã là &quot;hahahaha&quot;.
Chữ cái thứ 5 là &quot;h&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a2345678999999999999999&quot;, k = 1
<strong>Đầu ra:</strong> &quot;a&quot;
<strong>Giải thích:</strong> Chuỗi sau khi giải mã gồm ký tự &quot;a&quot; được lặp lại 8301530446056247680 lần.
Chữ cái thứ 1 là &quot;a&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm chữ cái tiếng Anh viết thường và các chữ số từ <code>2</code> đến <code>9</code>.</li>
	<li><code>s</code> bắt đầu bằng một chữ cái.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
	<li>Đảm bảo <code>k</code> nhỏ hơn hoặc bằng độ dài của chuỗi đã giải mã.</li>
	<li>Đảm bảo chuỗi đã giải mã có ít hơn <code>2<sup>63</sup></code> chữ cái.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tư duy ngược

<!-- thinking:start -->

> **Tư duy**
>
> Dãy sau giải mã được tạo từ các chữ cái và chữ số lặp, có thể dài hơn rất nhiều so với $10^{18}$; vì $k$ có thể lên đến $10^9$, ta không thể tạo toàn bộ dãy. Ký tự thứ $k$ được xác định bởi thao tác cuối cùng tạo ra đoạn chứa vị trí $k$.
>
> Duyệt xuôi để tính độ dài đã giải mã $m$, sau đó duyệt ngược: khi gặp chữ số, lấy $k$ modulo độ dài hiện tại; khi gặp chữ cái và $k\equiv 0$, đó là đáp án. Duyệt $s$ hai lượt là đủ.

<!-- thinking:end -->

Trước tiên, ta tính tổng độ dài $m$ của chuỗi đã giải mã, rồi duyệt chuỗi từ cuối về đầu. Ở mỗi bước, cập nhật $k$ thành $k \bmod m$. Khi $k$ bằng $0$ và ký tự hiện tại là chữ cái, trả về ký tự đó. Nếu ký tự hiện tại là chữ số, chia $m$ cho chữ số này; nếu là chữ cái, giảm $m$ đi $1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def decodeAtIndex(self, s: str, k: int) -> str:
        m = 0
        for c in s:
            if c.isdigit():
                m *= int(c)
            else:
                m += 1
        for c in s[::-1]:
            k %= m
            if k == 0 and c.isalpha():
                return c
            if c.isdigit():
                m //= int(c)
            else:
                m -= 1
```

#### Java

```java
class Solution {
    public String decodeAtIndex(String s, int k) {
        long m = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (Character.isDigit(s.charAt(i))) {
                m *= (s.charAt(i) - '0');
            } else {
                ++m;
            }
        }
        for (int i = s.length() - 1;; --i) {
            k %= m;
            if (k == 0 && !Character.isDigit(s.charAt(i))) {
                return String.valueOf(s.charAt(i));
            }
            if (Character.isDigit(s.charAt(i))) {
                m /= (s.charAt(i) - '0');
            } else {
                --m;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    string decodeAtIndex(string s, int k) {
        long long m = 0;
        for (char& c : s) {
            if (isdigit(c)) {
                m *= (c - '0');
            } else {
                ++m;
            }
        }
        for (int i = s.size() - 1;; --i) {
            k %= m;
            if (k == 0 && isalpha(s[i])) {
                return string(1, s[i]);
            }
            if (isdigit(s[i])) {
                m /= (s[i] - '0');
            } else {
                --m;
            }
        }
    }
};
```

#### Go

```go
func decodeAtIndex(s string, k int) string {
	m := 0
	for _, c := range s {
		if c >= '0' && c <= '9' {
			m *= int(c - '0')
		} else {
			m++
		}
	}
	for i := len(s) - 1; ; i-- {
		k %= m
		if k == 0 && s[i] >= 'a' && s[i] <= 'z' {
			return string(s[i])
		}
		if s[i] >= '0' && s[i] <= '9' {
			m /= int(s[i] - '0')
		} else {
			m--
		}
	}
}
```

#### TypeScript

```ts
function decodeAtIndex(s: string, k: number): string {
    let m = 0n;
    for (const c of s) {
        if (c >= '1' && c <= '9') {
            m *= BigInt(c);
        } else {
            ++m;
        }
    }
    for (let i = s.length - 1; ; --i) {
        if (k >= m) {
            k %= Number(m);
        }
        if (k === 0 && s[i] >= 'a' && s[i] <= 'z') {
            return s[i];
        }
        if (s[i] >= '1' && s[i] <= '9') {
            m = (m / BigInt(s[i])) | 0n;
        } else {
            --m;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
