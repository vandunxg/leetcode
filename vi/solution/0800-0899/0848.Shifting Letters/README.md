---
comments: true
difficulty: Medium
tags:
    - Array
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [848. Shifting Letters](https://leetcode.com/problems/shifting-letters)

[中文文档](/solution/0800-0899/0848.Shifting%20Letters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường và mảng số nguyên <code>shifts</code> có cùng độ dài.</p>

<p>Gọi <code>shift()</code> của một chữ cái là chữ cái tiếp theo trong bảng chữ cái (quay vòng để <code>&#39;z&#39;</code> trở thành <code>&#39;a&#39;</code>).</p>

<ul>
	<li>Ví dụ, <code>shift(&#39;a&#39;) = &#39;b&#39;</code>, <code>shift(&#39;t&#39;) = &#39;u&#39;</code> và <code>shift(&#39;z&#39;) = &#39;a&#39;</code>.</li>
</ul>

<p>Với mỗi <code>shifts[i] = x</code>, ta dịch chuyển <code>i + 1</code> chữ cái đầu tiên của <code>s</code> đi <code>x</code> lần.</p>

<p>Trả về <em>chuỗi cuối cùng sau khi áp dụng tất cả các lần dịch chuyển lên s</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abc&quot;, shifts = [3,5,9]
<strong>Output:</strong> &quot;rpl&quot;
<strong>Giải thích:</strong> Ban đầu, ta có &quot;abc&quot;.
Sau khi dịch chuyển chữ cái đầu tiên của s đi 3 lần, ta được &quot;dbc&quot;.
Sau khi dịch chuyển 2 chữ cái đầu tiên của s đi 5 lần, ta được &quot;igc&quot;.
Sau khi dịch chuyển 3 chữ cái đầu tiên của s đi 9 lần, ta được &quot;rpl&quot;, đây là đáp án.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aaa&quot;, shifts = [1,2,3]
<strong>Output:</strong> &quot;gfd&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>shifts.length == s.length</code></li>
	<li><code>0 &lt;= shifts[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng hậu tố

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{shifts}[i]$ dịch chuyển prefix có độ dài $i+1$. Thực hiện lần lượt từng thao tác sẽ quá chậm khi $n\le 10^5$ và giá trị dịch chuyển lớn. Chữ cái ở vị trí $i$ được dịch chuyển theo tổng hậu tố $\textit{shifts}[i:]$.
>
> Tính dồn tổng hậu tố từ phải sang trái, lấy modulo $26$ rồi cập nhật ký tự. Một lượt duyệt ngược là đủ để xử lý mọi lần dịch chuyển.

<!-- thinking:end -->

Với mỗi ký tự trong chuỗi $s$, ta cần tính tổng số lần dịch chuyển cuối cùng của nó, bằng tổng $\textit{shifts}[i]$, $\textit{shifts}[i + 1]$, $\textit{shifts}[i + 2]$, v.v. Ta dùng tổng hậu tố: duyệt $\textit{shifts}$ từ cuối về đầu, tính số lần dịch chuyển cuối cùng cho mỗi ký tự rồi lấy modulo $26$ để xác định ký tự cuối.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài chuỗi $s$. Không tính bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shiftingLetters(self, s: str, shifts: List[int]) -> str:
        n, t = len(s), 0
        s = list(s)
        for i in range(n - 1, -1, -1):
            t += shifts[i]
            j = (ord(s[i]) - ord('a') + t) % 26
            s[i] = ascii_lowercase[j]
        return ''.join(s)
```

#### Java

```java
class Solution {
    public String shiftingLetters(String s, int[] shifts) {
        char[] cs = s.toCharArray();
        int n = cs.length;
        long t = 0;
        for (int i = n - 1; i >= 0; --i) {
            t += shifts[i];
            int j = (int) ((cs[i] - 'a' + t) % 26);
            cs[i] = (char) ('a' + j);
        }
        return String.valueOf(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string shiftingLetters(string s, vector<int>& shifts) {
        long long t = 0;
        int n = s.size();
        for (int i = n - 1; ~i; --i) {
            t += shifts[i];
            int j = (s[i] - 'a' + t) % 26;
            s[i] = 'a' + j;
        }
        return s;
    }
};
```

#### Go

```go
func shiftingLetters(s string, shifts []int) string {
	t := 0
	n := len(s)
	cs := []byte(s)
	for i := n - 1; i >= 0; i-- {
		t += shifts[i]
		j := (int(cs[i]-'a') + t) % 26
		cs[i] = byte('a' + j)
	}
	return string(cs)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
