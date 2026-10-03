---
comments: true
difficulty: Medium
rating: 1460
source: Biweekly Contest 55 Q2
tags:
    - Stack
    - String
    - Simulation
---

<!-- problem:start -->

# [1910. Remove All Occurrences of a Substring](https://leetcode.com/problems/remove-all-occurrences-of-a-substring)

[中文文档](/solution/1900-1999/1910.Remove%20All%20Occurrences%20of%20a%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>part</code>, thực hiện thao tác sau trên <code>s</code> cho đến khi <strong>tất cả</strong> các lần xuất hiện của chuỗi con <code>part</code> được xóa:</p>

<ul>
	<li>Tìm lần xuất hiện <strong>ngoài cùng bên trái</strong> của chuỗi con <code>part</code> và <strong>xóa</strong> nó khỏi <code>s</code>.</li>
</ul>

<p>Trả về <code>s</code><em> sau khi đã xóa tất cả các lần xuất hiện của </em><code>part</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;daabcbaabcbc&quot;, part = &quot;abc&quot;
<strong>Đầu ra:</strong> &quot;dab&quot;
<strong>Giải thích</strong>: Thực hiện các thao tác sau:
- s = &quot;da<strong><u>abc</u></strong>baabcbc&quot;, xóa &quot;abc&quot; bắt đầu từ chỉ số 2, nên s = &quot;dabaabcbc&quot;.
- s = &quot;daba<strong><u>abc</u></strong>bc&quot;, xóa &quot;abc&quot; bắt đầu từ chỉ số 4, nên s = &quot;dababc&quot;.
- s = &quot;dab<strong><u>abc</u></strong>&quot;, xóa &quot;abc&quot; bắt đầu từ chỉ số 3, nên s = &quot;dab&quot;.
Lúc này s không còn lần xuất hiện nào của &quot;abc&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;axxxxyyyyb&quot;, part = &quot;xy&quot;
<strong>Đầu ra:</strong> &quot;ab&quot;
<strong>Giải thích</strong>: Thực hiện các thao tác sau:
- s = &quot;axxx<strong><u>xy</u></strong>yyyb&quot;, xóa &quot;xy&quot; bắt đầu từ chỉ số 4, nên s = &quot;axxxyyyb&quot;.
- s = &quot;axx<strong><u>xy</u></strong>yyb&quot;, xóa &quot;xy&quot; bắt đầu từ chỉ số 3, nên s = &quot;axxyyb&quot;.
- s = &quot;ax<strong><u>xy</u></strong>yb&quot;, xóa &quot;xy&quot; bắt đầu từ chỉ số 2, nên s = &quot;axyb&quot;.
- s = &quot;a<strong><u>xy</u></strong>b&quot;, xóa &quot;xy&quot; bắt đầu từ chỉ số 1, nên s = &quot;ab&quot;.
Lúc này s không còn lần xuất hiện nào của &quot;xy&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>1 &lt;= part.length &lt;= 1000</code></li>
	<li><code>s</code>​​​​​​ và <code>part</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Brute Force

<!-- thinking:start -->

> **Tư duy**
>
> Vì cả $s$ và $\textit{part}$ đều có độ dài không quá $10^3$, việc liên tục tìm và xóa một lần xuất hiện là chấp nhận được.
>
> Mỗi bước thay thế lần xuất hiện ngoài cùng bên trái của $\textit{part}$. Việc nối các phần còn lại sau đó có thể tạo lại $\textit{part}$, nên vòng lặp tiếp tục cho đến khi không còn lần xuất hiện nào.
>
> Mỗi lần thay thế đều làm $s$ ngắn đi, nên quá trình chắc chắn kết thúc và tuân theo thứ tự xóa từ trái sang phải yêu cầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeOccurrences(self, s: str, part: str) -> str:
        while part in s:
            s = s.replace(part, '', 1)
        return s
```

#### Java

```java
class Solution {
    public String removeOccurrences(String s, String part) {
        while (s.contains(part)) {
            s = s.replaceFirst(part, "");
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeOccurrences(string s, string part) {
        int m = part.size();
        while (s.find(part) != -1) {
            s = s.erase(s.find(part), m);
        }
        return s;
    }
};
```

#### Go

```go
func removeOccurrences(s string, part string) string {
	for strings.Contains(s, part) {
		s = strings.Replace(s, part, "", 1)
	}
	return s
}
```

#### TypeScript

```ts
function removeOccurrences(s: string, part: string): string {
    while (s.includes(part)) {
        s = s.replace(part, '');
    }
    return s;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 quét lại toàn bộ chuỗi sau mỗi lần xóa. Mỗi lần xóa làm $s$ ngắn đi ít nhất một ký tự, nên có thể có $O(n)$ vòng lặp và tổng thời gian là $O(n^2)$.
>
> Khi đọc từ trái sang phải, mọi lần xuất hiện còn lại của $\textit{part}$ mà nằm ngoài cùng bên trái phải kết thúc tại ký tự vừa đọc. Một kết quả khớp ở trước đó đã được xóa.
>
> Lưu các ký tự còn lại vào một stack. Sau mỗi lần push, pop $m$ ký tự cuối nếu chúng bằng $\textit{part}$. Một lượt duyệt sẽ thực hiện mọi lần xóa ngoài cùng bên trái.

<!-- thinking:end -->

Quét $s$ từ trái sang phải và lưu các ký tự chưa bị xóa vào một chuỗi $st$. Thêm ký tự hiện tại vào cuối chuỗi. Nếu độ dài của $st$ ít nhất là $m = |\textit{part}|$ và $m$ ký tự cuối cùng của nó là $\textit{part}$, xóa $m$ ký tự đó. Sau khi quét xong, $st$ là đáp án.

Điều này tương đương với Lời giải 1. Ở mọi thời điểm, $st$ không chứa lần xuất hiện nào của $\textit{part}$, nên kết quả khớp tiếp theo phải kết thúc tại ký tự vừa thêm, và đó là lần xuất hiện ngoài cùng bên trái trong phần còn lại.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là độ dài của $s$ và $\textit{part}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeOccurrences(self, s: str, part: str) -> str:
        m = len(part)
        st = []
        for c in s:
            st.append(c)
            if len(st) >= m and ''.join(st[-m:]) == part:
                del st[-m:]
        return ''.join(st)
```

#### Java

```java
class Solution {
    public String removeOccurrences(String s, String part) {
        int m = part.length();
        StringBuilder st = new StringBuilder();
        for (int i = 0; i < s.length(); ++i) {
            st.append(s.charAt(i));
            if (st.length() >= m && st.substring(st.length() - m).equals(part)) {
                st.setLength(st.length() - m);
            }
        }
        return st.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeOccurrences(string s, string part) {
        int m = part.size();
        string st;
        for (char c : s) {
            st.push_back(c);
            if ((int) st.size() >= m && st.compare(st.size() - m, m, part) == 0) {
                st.erase(st.size() - m);
            }
        }
        return st;
    }
};
```

#### Go

```go
func removeOccurrences(s string, part string) string {
	m := len(part)
	st := make([]byte, 0, len(s))
	for i := 0; i < len(s); i++ {
		st = append(st, s[i])
		if len(st) >= m && string(st[len(st)-m:]) == part {
			st = st[:len(st)-m]
		}
	}
	return string(st)
}
```

#### TypeScript

```ts
function removeOccurrences(s: string, part: string): string {
    const m = part.length;
    const st: string[] = [];
    for (const c of s) {
        st.push(c);
        if (st.length >= m && st.slice(-m).join('') === part) {
            st.length -= m;
        }
    }
    return st.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
