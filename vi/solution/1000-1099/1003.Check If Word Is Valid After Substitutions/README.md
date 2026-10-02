---
comments: true
difficulty: Medium
rating: 1426
source: Weekly Contest 126 Q2
tags:
    - Stack
    - String
---

<!-- problem:start -->

# [1003. Check If Word Is Valid After Substitutions](https://leetcode.com/problems/check-if-word-is-valid-after-substitutions)

[中文文档](/solution/1000-1099/1003.Check%20If%20Word%20Is%20Valid%20After%20Substitutions/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy xác định chuỗi có <strong>hợp lệ</strong> hay không.</p>

<p>Chuỗi <code>s</code> được gọi là <strong>hợp lệ</strong> nếu bắt đầu từ chuỗi rỗng <code>t = &quot;&quot;</code>, ta có thể <strong>biến đổi </strong><code>t</code><strong> thành </strong><code>s</code> bằng cách thực hiện thao tác sau <strong>bao nhiêu lần tùy ý</strong>:</p>

<ul>
	<li>Chèn chuỗi <code>&quot;abc&quot;</code> vào bất kỳ vị trí nào trong <code>t</code>. Cụ thể hơn, <code>t</code> trở thành <code>t<sub>left</sub> + &quot;abc&quot; + t<sub>right</sub></code>, trong đó <code>t == t<sub>left</sub> + t<sub>right</sub></code>. Lưu ý rằng <code>t<sub>left</sub></code> và <code>t<sub>right</sub></code> có thể <strong>rỗng</strong>.</li>
</ul>

<p>Trả về <code>true</code> <em>nếu </em><code>s</code><em> là chuỗi <strong>hợp lệ</strong>; ngược lại, trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabcbc&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
&quot;&quot; -&gt; &quot;<u>abc</u>&quot; -&gt; &quot;a<u>abc</u>bc&quot;
Vì vậy, &quot;aabcbc&quot; là chuỗi hợp lệ.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcabcababcc&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
&quot;&quot; -&gt; &quot;<u>abc</u>&quot; -&gt; &quot;abc<u>abc</u>&quot; -&gt; &quot;abcabc<u>abc</u>&quot; -&gt; &quot;abcabcab<u>abc</u>c&quot;
Vì vậy, &quot;abcabcababcc&quot; là chuỗi hợp lệ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abccba&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể tạo ra &quot;abccba&quot; bằng thao tác trên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Liên tục xóa $\textit{abc}$ khỏi $s$ sẽ cho đáp án đúng, nhưng với $n\le 2\times 10^4$, mỗi lượt quét mất thời gian tuyến tính nên trường hợp xấu nhất có độ phức tạp bậc hai. Nếu độ dài không chia hết cho $3$, không thể tạo ra $s$ bằng cách chèn chuỗi.
>
> Chuỗi hợp lệ được tạo bằng cách chèn $\textit{abc}$ vào bất kỳ vị trí nào. Vì vậy, khi đọc từ trái sang phải, nếu ba ký tự trên đỉnh stack là $\textit{abc}$ thì đó là một lần chèn đã hoàn tất và có thể loại bỏ.
>
> Ta dùng stack để thực hiện cách này: sau mỗi lần push, nếu ba ký tự cuối tạo thành $\textit{abc}$ thì pop chúng. Chuỗi hợp lệ khi và chỉ khi stack rỗng sau cùng.

<!-- thinking:end -->

Quan sát thao tác trong đề, mỗi lần chèn chuỗi $\textit{"abc"}$ vào một vị trí bất kỳ, độ dài chuỗi tăng thêm $3$. Vì vậy, nếu $s$ hợp lệ thì độ dài của nó phải chia hết cho $3$. Trước tiên, ta kiểm tra độ dài chuỗi $s$. Nếu không chia hết cho $3$, chắc chắn $s$ không hợp lệ và có thể trả về ngay $\textit{false}$.

Tiếp theo, ta duyệt từng ký tự $c$ trong chuỗi $s$ và push $c$ vào stack $t$. Nếu stack $t$ có ít nhất $3$ phần tử và ba phần tử trên đỉnh tạo thành chuỗi $\textit{"abc"}$, ta pop ba phần tử đó khỏi stack. Sau đó, tiếp tục duyệt ký tự kế tiếp trong $s$.

Sau khi duyệt xong, nếu stack $t$ rỗng thì chuỗi $s$ hợp lệ và ta trả về $\textit{true}$; ngược lại, trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) % 3:
            return False
        t = []
        for c in s:
            t.append(c)
            if ''.join(t[-3:]) == 'abc':
                t[-3:] = []
        return not t
```

#### Java

```java
class Solution {
    public boolean isValid(String s) {
        if (s.length() % 3 > 0) {
            return false;
        }
        StringBuilder t = new StringBuilder();
        for (char c : s.toCharArray()) {
            t.append(c);
            if (t.length() >= 3 && "abc".equals(t.substring(t.length() - 3))) {
                t.delete(t.length() - 3, t.length());
            }
        }
        return t.isEmpty();
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isValid(string s) {
        if (s.size() % 3) {
            return false;
        }
        string t;
        for (char c : s) {
            t.push_back(c);
            if (t.size() >= 3 && t.substr(t.size() - 3, 3) == "abc") {
                t.erase(t.end() - 3, t.end());
            }
        }
        return t.empty();
    }
};
```

#### Go

```go
func isValid(s string) bool {
	if len(s)%3 > 0 {
		return false
	}
	t := []byte{}
	for i := range s {
		t = append(t, s[i])
		if len(t) >= 3 && string(t[len(t)-3:]) == "abc" {
			t = t[:len(t)-3]
		}
	}
	return len(t) == 0
}
```

#### TypeScript

```ts
function isValid(s: string): boolean {
    if (s.length % 3 !== 0) {
        return false;
    }
    const t: string[] = [];
    for (const c of s) {
        t.push(c);
        if (t.slice(-3).join('') === 'abc') {
            t.splice(-3);
        }
    }
    return t.length === 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
