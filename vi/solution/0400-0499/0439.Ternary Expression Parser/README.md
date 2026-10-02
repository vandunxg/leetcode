---
comments: true
difficulty: Medium
tags:
    - Stack
    - Recursion
    - String
---

<!-- problem:start -->

# [439. Ternary Expression Parser 🔒](https://leetcode.com/problems/ternary-expression-parser)

[中文文档](/solution/0400-0499/0439.Ternary%20Expression%20Parser/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>expression</code> biểu diễn các biểu thức ternary lồng nhau tùy ý. Hãy tính và trả về <em>kết quả của biểu thức</em>.</p>

<p>Có thể giả định biểu thức đầu vào hợp lệ và chỉ chứa chữ số, <code>&#39;?&#39;</code>, <code>&#39;:&#39;</code>, <code>&#39;T&#39;</code> và <code>&#39;F&#39;</code>, trong đó <code>&#39;T&#39;</code> là true và <code>&#39;F&#39;</code> là false. Mọi số trong biểu thức đều là số <strong>một chữ số</strong> (nằm trong khoảng <code>[0, 9]</code>).</p>

<p>Các biểu thức điều kiện được nhóm từ phải sang trái (như trong hầu hết ngôn ngữ lập trình), và kết quả luôn là một chữ số, <code>&#39;T&#39;</code> hoặc <code>&#39;F&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;T?2:3&quot;
<strong>Đầu ra:</strong> &quot;2&quot;
<strong>Giải thích:</strong> Nếu điều kiện đúng thì kết quả là 2; ngược lại kết quả là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;F?1:T?4:5&quot;
<strong>Đầu ra:</strong> &quot;4&quot;
<strong>Giải thích:</strong> Các biểu thức điều kiện được nhóm từ phải sang trái. Có thể thêm dấu ngoặc để đọc/tính biểu thức như sau:
&quot;(F ? 1 : (T ? 4 : 5))&quot; --&gt; &quot;(F ? 1 : 4)&quot; --&gt; &quot;4&quot;
hoặc &quot;(F ? 1 : (T ? 4 : 5))&quot; --&gt; &quot;(T ? 4 : 5)&quot; --&gt; &quot;4&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;T?T?F:5:3&quot;
<strong>Đầu ra:</strong> &quot;F&quot;
<strong>Giải thích:</strong> Các biểu thức điều kiện được nhóm từ phải sang trái. Có thể thêm dấu ngoặc để đọc/tính biểu thức như sau:
&quot;(T ? (T ? F : 5) : 3)&quot; --&gt; &quot;(T ? F : 3)&quot; --&gt; &quot;F&quot;
&quot;(T ? (T ? F : 5) : 3)&quot; --&gt; &quot;(T ? F : 5)&quot; --&gt; &quot;F&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>5 &lt;= expression.length &lt;= 10<sup>4</sup></code></li>
	<li><code>expression</code> chỉ gồm chữ số, <code>&#39;T&#39;</code>, <code>&#39;F&#39;</code>, <code>&#39;?&#39;</code> và <code>&#39;:&#39;</code>.</li>
	<li>Đảm bảo <code>expression</code> là biểu thức ternary hợp lệ và mỗi số đều là số <strong>một chữ số</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Biểu thức ternary có tính kết hợp phải: $T?T?1:2:3$ tính lựa chọn bên trong trước. Nếu duyệt từ trái sang phải, cần đọc trước cả hai nhánh; dùng recursive descent thì phức tạp hơn cần thiết.
>
> Duyệt từ phải sang trái: đưa các ký tự thông thường vào stack, bỏ qua dấu hai chấm; khi gặp dấu hỏi, ký tự kế tiếp là điều kiện—giữ lại một trong hai kết quả trên stack tùy theo $T/F$.
>
> Duyệt từ phải sang trái giúp tính các biểu thức bên trong trước, vì vậy stack luôn chứa các nhánh đã rút gọn và cuối cùng còn lại một ký tự.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def parseTernary(self, expression: str) -> str:
        stk = []
        cond = False
        for c in expression[::-1]:
            if c == ':':
                continue
            if c == '?':
                cond = True
            else:
                if cond:
                    if c == 'T':
                        x = stk.pop()
                        stk.pop()
                        stk.append(x)
                    else:
                        stk.pop()
                    cond = False
                else:
                    stk.append(c)
        return stk[0]
```

#### Java

```java
class Solution {
    public String parseTernary(String expression) {
        Deque<Character> stk = new ArrayDeque<>();
        boolean cond = false;
        for (int i = expression.length() - 1; i >= 0; --i) {
            char c = expression.charAt(i);
            if (c == ':') {
                continue;
            }
            if (c == '?') {
                cond = true;
            } else {
                if (cond) {
                    if (c == 'T') {
                        char x = stk.pop();
                        stk.pop();
                        stk.push(x);
                    } else {
                        stk.pop();
                    }
                    cond = false;
                } else {
                    stk.push(c);
                }
            }
        }
        return String.valueOf(stk.peek());
    }
}
```

#### C++

```cpp
class Solution {
public:
    string parseTernary(string expression) {
        string stk;
        bool cond = false;
        reverse(expression.begin(), expression.end());
        for (char& c : expression) {
            if (c == ':') {
                continue;
            }
            if (c == '?') {
                cond = true;
            } else {
                if (cond) {
                    if (c == 'T') {
                        char x = stk.back();
                        stk.pop_back();
                        stk.pop_back();
                        stk.push_back(x);
                    } else {
                        stk.pop_back();
                    }
                    cond = false;
                } else {
                    stk.push_back(c);
                }
            }
        }
        return {stk[0]};
    }
};
```

#### Go

```go
func parseTernary(expression string) string {
	stk := []byte{}
	cond := false
	for i := len(expression) - 1; i >= 0; i-- {
		c := expression[i]
		if c == ':' {
			continue
		}
		if c == '?' {
			cond = true
		} else {
			if cond {
				if c == 'T' {
					x := stk[len(stk)-1]
					stk = stk[:len(stk)-2]
					stk = append(stk, x)
				} else {
					stk = stk[:len(stk)-1]
				}
				cond = false
			} else {
				stk = append(stk, c)
			}
		}
	}
	return string(stk[0])
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
