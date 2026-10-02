---
comments: true
difficulty: Medium
rating: 1759
source: Biweekly Contest 32 Q3
tags:
    - Stack
    - Greedy
    - String
    - Parentheses
---

<!-- problem:start -->

# [1541. Minimum Insertions to Balance a Parentheses String](https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string)

[中文文档](/solution/1500-1599/1541.Minimum%20Insertions%20to%20Balance%20a%20Parentheses%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi ngoặc <code>s</code> chỉ chứa các ký tự <code>&#39;(&#39;</code> và <code>&#39;)&#39;</code>. Chuỗi ngoặc được gọi là <strong>cân bằng</strong> nếu:</p>

<ul>
	<li>Mỗi ngoặc trái <code>&#39;(&#39;</code> phải có hai ngoặc phải liên tiếp tương ứng <code>&#39;))&#39;</code>.</li>
	<li>Ngoặc trái <code>&#39;(&#39;</code> phải đứng trước hai ngoặc phải liên tiếp tương ứng <code>&#39;))&#39;</code>.</li>
</ul>

<p>Nói cách khác, ta xem <code>&#39;(&#39;</code> là ngoặc mở và <code>&#39;))&#39;</code> là ngoặc đóng.</p>

<ul>
	<li>For example, <code>&quot;())&quot;</code>, <code>&quot;())(())))&quot;</code> and <code>&quot;(())())))&quot;</code> are balanced, <code>&quot;)()&quot;</code>, <code>&quot;()))&quot;</code> and <code>&quot;(()))&quot;</code> are not balanced.</li>
</ul>

<p>Bạn có thể chèn các ký tự <code>&#39;(&#39;</code> và <code>&#39;)&#39;</code> vào bất kỳ vị trí nào để cân bằng chuỗi khi cần.</p>

<p>Trả về <em>số lần chèn ít nhất</em> cần thiết để làm <code>s</code> cân bằng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;(()))&quot;
<strong>Output:</strong> 1
<strong>Explanation:</strong> &#39;(&#39; thứ hai có hai &#39;))&#39; tương ứng, nhưng &#39;(&#39; đầu tiên chỉ có một &#39;)&#39;. Cần thêm một &#39;)&#39; vào cuối chuỗi để được &quot;(())))&quot; cân bằng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;())&quot;
<strong>Output:</strong> 0
<strong>Explanation:</strong> Chuỗi đã cân bằng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;))())(&quot;
<strong>Output:</strong> 3
<strong>Explanation:</strong> Thêm &#39;(&#39; để ghép với &#39;))&#39; đầu tiên, thêm &#39;))&#39; để ghép với &#39;(&#39; cuối cùng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> consists of <code>&#39;(&#39;</code> and <code>&#39;)&#39;</code> only.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi hợp lệ ghép mỗi '(' với hai ')' liên tiếp. Vì $n\le 10^5$, việc quét lại các vị trí chèn nhiều lần là không phù hợp; một lượt duyệt greedy là đủ.
>
> Duy trì $x$, số ngoặc trái chưa ghép. Gặp '(', tăng $x$. Gặp ')', chèn một ngoặc phải nếu ký tự kế tiếp không phải ')'. Sau đó, nếu $x=0$ thì chèn một ngoặc trái, còn không thì dùng một ngoặc trái đang chờ. Sau khi duyệt xong, mỗi ngoặc trái còn lại cần hai ngoặc phải.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minInsertions(self, s: str) -> int:
        ans = x = 0
        i, n = 0, len(s)
        while i < n:
            if s[i] == '(':
                # 待匹配的左括号加 1
                x += 1
            else:
                if i < n - 1 and s[i + 1] == ')':
                    # 有连续两个右括号，i 往后移动
                    i += 1
                else:
                    # 只有一个右括号，插入一个
                    ans += 1
                if x == 0:
                    # 无待匹配的左括号，插入一个
                    ans += 1
                else:
                    # 待匹配的左括号减 1
                    x -= 1
            i += 1
        # 遍历结束，仍有待匹配的左括号，说明右括号不足，插入 x << 1 个
        ans += x << 1
        return ans
```

#### Java

```java
class Solution {
    public int minInsertions(String s) {
        int ans = 0, x = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '(') {
                ++x;
            } else {
                if (i < n - 1 && s.charAt(i + 1) == ')') {
                    ++i;
                } else {
                    ++ans;
                }
                if (x == 0) {
                    ++ans;
                } else {
                    --x;
                }
            }
        }
        ans += x << 1;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minInsertions(string s) {
        int ans = 0, x = 0;
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (s[i] == '(') {
                ++x;
            } else {
                if (i < n - 1 && s[i + 1] == ')') {
                    ++i;
                } else {
                    ++ans;
                }
                if (x == 0) {
                    ++ans;
                } else {
                    --x;
                }
            }
        }
        ans += x << 1;
        return ans;
    }
};
```

#### Go

```go
func minInsertions(s string) int {
	ans, x, n := 0, 0, len(s)
	for i := 0; i < n; i++ {
		if s[i] == '(' {
			x++
		} else {
			if i < n-1 && s[i+1] == ')' {
				i++
			} else {
				ans++
			}
			if x == 0 {
				ans++
			} else {
				x--
			}
		}
	}
	ans += x << 1
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
