---
comments: true
difficulty: Medium
tags:
    - Stack
    - String
    - Parentheses
---

<!-- problem:start -->

# [856. Score of Parentheses](https://leetcode.com/problems/score-of-parentheses)

[中文文档](/solution/0800-0899/0856.Score%20of%20Parentheses/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi ngoặc cân bằng <code>s</code>, hãy trả về <em><strong>điểm số</strong> của chuỗi</em>.</p>

<p><strong>Điểm số</strong> của một chuỗi ngoặc cân bằng được tính theo các quy tắc sau:</p>

<ul>
	<li><code>&quot;()&quot;</code> có điểm số là <code>1</code>.</li>
	<li><code>AB</code> có điểm số là <code>A + B</code>, trong đó <code>A</code> và <code>B</code> là các chuỗi ngoặc cân bằng.</li>
	<li><code>(A)</code> có điểm số là <code>2 * A</code>, trong đó <code>A</code> là một chuỗi ngoặc cân bằng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;()&quot;
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(())&quot;
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;()()&quot;
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 50</code></li>
	<li><code>s</code> chỉ gồm <code>&#39;(&#39;</code> và <code>&#39;)&#39;</code>.</li>
	<li><code>s</code> là một chuỗi ngoặc cân bằng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số được nhân đôi theo mỗi tầng lồng nhau: cặp $()$ trong cùng có điểm $1$. Ta không cần dựng cây hay tính toàn bộ biểu thức bằng stack.
>
> Duyệt chuỗi và theo dõi độ sâu $d$; khi gặp cặp $()$ đóng lại, cộng $1\ll d$ vào đáp án. Các dấu ngoặc đóng khác chỉ làm giảm độ sâu.

<!-- thinking:end -->

Ta nhận thấy `()` là cấu trúc duy nhất đóng góp điểm số; các cặp ngoặc bên ngoài chỉ nhân điểm của cấu trúc này lên. Vì vậy, ta chỉ cần chú ý đến `()`.

Ta dùng $d$ để theo dõi độ sâu ngoặc hiện tại. Với mỗi `(`, ta tăng độ sâu thêm một; với mỗi `)`, ta giảm độ sâu đi một. Khi gặp `()`, ta cộng $2^d$ vào đáp án.

Xét ví dụ `(()(()))`. Trước tiên, ta tìm hai cặp ngoặc `()` nằm bên trong rồi cộng $2^d$ tương ứng vào điểm số. Thực chất, ta đang tính điểm số của `(()) + ((()))`.

```bash
( ( ) ( ( ) ) )
  ^ ^   ^ ^

( ( ) ) + ( ( ( ) ) )
  ^ ^         ^ ^
```

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài chuỗi.

Các bài toán liên quan đến dấu ngoặc:

- [678. Valid Parenthesis String](https://github.com/doocs/leetcode/blob/main/solution/0600-0699/0678.Valid%20Parenthesis%20String/README.md)
- [1021. Remove Outermost Parentheses](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1021.Remove%20Outermost%20Parentheses/README.md)
- [1096. Brace Expansion II](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1096.Brace%20Expansion%20II/README.md)
- [1249. Minimum Remove to Make Valid Parentheses](https://github.com/doocs/leetcode/blob/main/solution/1200-1299/1249.Minimum%20Remove%20to%20Make%20Valid%20Parentheses/README.md)
- [1541. Minimum Insertions to Balance a Parentheses String](https://github.com/doocs/leetcode/blob/main/solution/1500-1599/1541.Minimum%20Insertions%20to%20Balance%20a%20Parentheses%20String/README.md)
- [2116. Check if a Parentheses String Can Be Valid](https://github.com/doocs/leetcode/blob/main/solution/2100-2199/2116.Check%20if%20a%20Parentheses%20String%20Can%20Be%20Valid/README.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def scoreOfParentheses(self, s: str) -> int:
        ans = d = 0
        for i, c in enumerate(s):
            if c == '(':
                d += 1
            else:
                d -= 1
                if s[i - 1] == '(':
                    ans += 1 << d
        return ans
```

#### Java

```java
class Solution {
    public int scoreOfParentheses(String s) {
        int ans = 0, d = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == '(') {
                ++d;
            } else {
                --d;
                if (s.charAt(i - 1) == '(') {
                    ans += 1 << d;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int scoreOfParentheses(string s) {
        int ans = 0, d = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == '(') {
                ++d;
            } else {
                --d;
                if (s[i - 1] == '(') {
                    ans += 1 << d;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func scoreOfParentheses(s string) int {
	ans, d := 0, 0
	for i, c := range s {
		if c == '(' {
			d++
		} else {
			d--
			if s[i-1] == '(' {
				ans += 1 << d
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
