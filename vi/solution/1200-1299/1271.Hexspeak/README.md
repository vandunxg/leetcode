---
comments: true
difficulty: Easy
rating: 1384
source: Biweekly Contest 14 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [1271. Hexspeak 🔒](https://leetcode.com/problems/hexspeak)

[中文文档](/solution/1200-1299/1271.Hexspeak/README.md)

## Mô tả

<!-- description:start -->

<p>Một số thập phân có thể được chuyển thành <strong>biểu diễn Hexspeak</strong> bằng cách trước tiên chuyển số đó thành chuỗi thập lục phân viết hoa, sau đó thay mọi chữ số <code>&#39;0&#39;</code> bằng chữ cái <code>&#39;O&#39;</code> và chữ số <code>&#39;1&#39;</code> bằng chữ cái <code>&#39;I&#39;</code>. Biểu diễn này hợp lệ khi và chỉ khi nó chỉ gồm các chữ cái trong tập <code>{&#39;A&#39;, &#39;B&#39;, &#39;C&#39;, &#39;D&#39;, &#39;E&#39;, &#39;F&#39;, &#39;I&#39;, &#39;O&#39;}</code>.</p>

<p>Cho chuỗi <code>num</code> biểu diễn số nguyên thập phân <code>n</code>, hãy <em>trả về <strong>biểu diễn Hexspeak</strong> của </em><code>n</code><em> nếu biểu diễn đó hợp lệ, nếu không thì trả về </em><code>&quot;ERROR&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;257&quot;
<strong>Đầu ra:</strong> &quot;IOI&quot;
<strong>Giải thích:</strong> 257 ở hệ thập lục phân là 101.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;3&quot;
<strong>Đầu ra:</strong> &quot;ERROR&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 12</code></li>
	<li><code>num</code> không chứa số 0 ở đầu.</li>
	<li><code>num</code> biểu diễn một số nguyên trong khoảng <code>[1, 10<sup>12</sup>]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Viết số dưới dạng thập lục phân, đổi $0,1$ thành $O,I$, rồi loại bỏ nếu có chữ số khác. $N$ có thể đạt $10^{12}$: chuyển sang $hex$, thay thế rồi kiểm tra xem các ký tự có thuộc $ABCDEFIO$ hay không. Đây chính là mô phỏng theo đề bài.

<!-- thinking:end -->

Chuyển số thành chuỗi thập lục phân, sau đó duyệt chuỗi, đổi chữ số $0$ thành chữ cái $O$ và chữ số $1$ thành chữ cái $I$. Cuối cùng, kiểm tra xem chuỗi sau khi chuyển đổi có hợp lệ hay không.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là kích thước của số thập phân được biểu diễn bởi $num$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def toHexspeak(self, num: str) -> str:
        s = set('ABCDEFIO')
        t = hex(int(num))[2:].upper().replace('0', 'O').replace('1', 'I')
        return t if all(c in s for c in t) else 'ERROR'
```

#### Java

```java
class Solution {
    private static final Set<Character> S = Set.of('A', 'B', 'C', 'D', 'E', 'F', 'I', 'O');

    public String toHexspeak(String num) {
        String t
            = Long.toHexString(Long.valueOf(num)).toUpperCase().replace("0", "O").replace("1", "I");
        for (char c : t.toCharArray()) {
            if (!S.contains(c)) {
                return "ERROR";
            }
        }
        return t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string toHexspeak(string num) {
        stringstream ss;
        ss << hex << stol(num);
        string t = ss.str();
        for (int i = 0; i < t.size(); ++i) {
            if (t[i] >= '2' && t[i] <= '9') return "ERROR";
            if (t[i] == '0')
                t[i] = 'O';
            else if (t[i] == '1')
                t[i] = 'I';
            else
                t[i] = t[i] - 32;
        }
        return t;
    }
};
```

#### Go

```go
func toHexspeak(num string) string {
	x, _ := strconv.Atoi(num)
	t := strings.ToUpper(fmt.Sprintf("%x", x))
	t = strings.ReplaceAll(t, "0", "O")
	t = strings.ReplaceAll(t, "1", "I")
	for _, c := range t {
		if c >= '2' && c <= '9' {
			return "ERROR"
		}
	}
	return t
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
