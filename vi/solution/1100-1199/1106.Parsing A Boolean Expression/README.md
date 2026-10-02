---
comments: true
difficulty: Hard
rating: 1880
source: Weekly Contest 143 Q4
tags:
    - Stack
    - Recursion
    - String
---

<!-- problem:start -->

# [1106. Parsing A Boolean Expression](https://leetcode.com/problems/parsing-a-boolean-expression)

[中文文档](/solution/1100-1199/1106.Parsing%20A%20Boolean%20Expression/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Biểu thức boolean</strong> là biểu thức có kết quả là <code>true</code> hoặc <code>false</code>. Biểu thức có thể thuộc một trong các dạng sau:</p>

<ul>
	<li><code>&#39;t&#39;</code> cho kết quả <code>true</code>.</li>
	<li><code>&#39;f&#39;</code> cho kết quả <code>false</code>.</li>
	<li><code>&#39;!(subExpr)&#39;</code> cho kết quả là phép <strong>NOT logic</strong> của biểu thức bên trong <code>subExpr</code>.</li>
	<li><code>&#39;&amp;(subExpr<sub>1</sub>, subExpr<sub>2</sub>, ..., subExpr<sub>n</sub>)&#39;</code> cho kết quả là phép <strong>AND logic</strong> của các biểu thức bên trong <code>subExpr<sub>1</sub>, subExpr<sub>2</sub>, ..., subExpr<sub>n</sub></code>, với <code>n &gt;= 1</code>.</li>
	<li><code>&#39;|(subExpr<sub>1</sub>, subExpr<sub>2</sub>, ..., subExpr<sub>n</sub>)&#39;</code> cho kết quả là phép <strong>OR logic</strong> của các biểu thức bên trong <code>subExpr<sub>1</sub>, subExpr<sub>2</sub>, ..., subExpr<sub>n</sub></code>, với <code>n &gt;= 1</code>.</li>
</ul>

<p>Cho chuỗi <code>expression</code> biểu diễn một <strong>biểu thức boolean</strong>, hãy trả về <em>giá trị của biểu thức đó</em>.</p>

<p><strong>Đảm bảo</strong> biểu thức đã cho hợp lệ và tuân theo các quy tắc trên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;&amp;(|(f))&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> 
Trước tiên, tính |(f) --&gt; f. Khi đó biểu thức trở thành &quot;&amp;(f)&quot;.
Tiếp theo, tính &amp;(f) --&gt; f. Khi đó biểu thức trở thành &quot;f&quot;.
Cuối cùng, trả về false.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;|(f,f,f,t)&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Giá trị của (false OR false OR false OR true) là true.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> expression = &quot;!(&amp;(f,t))&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 
Trước tiên, tính &amp;(f,t) --&gt; (false AND true) --&gt; false --&gt; f. Khi đó biểu thức trở thành &quot;!(f)&quot;.
Tiếp theo, tính !(f) --&gt; NOT false --&gt; true. Ta trả về true.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= expression.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li>expression[i] là một trong các ký tự sau: <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code>, <code>&#39;&amp;&#39;</code>, <code>&#39;|&#39;</code>, <code>&#39;!&#39;</code>, <code>&#39;t&#39;</code>, <code>&#39;f&#39;</code> và <code>&#39;,&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Biểu thức lồng nhau với các toán tử `!`, `&` và `|`; khi đệ quy phân tích, cần ghép đúng ngoặc và dấu phẩy. Duyệt từ trái sang phải, đẩy `t`, `f` và các toán tử vào stack. Khi gặp `)`, lấy các phần tử khỏi stack đến khi gặp toán tử, đếm số giá trị true/false vừa lấy rồi rút gọn lớp biểu thức trong ngoặc đó.
>
> Dấu phẩy chỉ dùng để phân tách nên không cần đưa vào stack. Cuối cùng, stack còn lại một ký tự duy nhất là giá trị của toàn bộ biểu thức.

<!-- thinking:end -->

Với dạng bài phân tích biểu thức này, ta có thể dùng stack hỗ trợ.

Ta duyệt biểu thức `expression` từ trái sang phải. Với mỗi ký tự $c$ gặp được:

- Nếu $c$ thuộc "tf!&|", đẩy trực tiếp ký tự đó vào stack;
- Nếu $c$ là dấu ngoặc phải `)`, lấy các phần tử khỏi stack cho đến khi gặp toán tử `'!'`, `'&'` hoặc `'|'`. Trong quá trình này, dùng các biến $t$ và $f$ để đếm số ký tự `'t'` và `'f'` được lấy ra. Cuối cùng, dựa vào số ký tự đã lấy và toán tử, tính ký tự mới `'t'` hoặc `'f'` rồi đẩy vào stack.

Sau khi duyệt hết biểu thức `expression`, stack chỉ còn một ký tự. Nếu đó là `'t'`, trả về `true`; nếu không, trả về `false`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài của biểu thức `expression`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def parseBoolExpr(self, expression: str) -> bool:
        stk = []
        for c in expression:
            if c in 'tf!&|':
                stk.append(c)
            elif c == ')':
                t = f = 0
                while stk[-1] in 'tf':
                    t += stk[-1] == 't'
                    f += stk[-1] == 'f'
                    stk.pop()
                match stk.pop():
                    case '!':
                        c = 't' if f else 'f'
                    case '&':
                        c = 'f' if f else 't'
                    case '|':
                        c = 't' if t else 'f'
                stk.append(c)
        return stk[0] == 't'
```

#### Java

```java
class Solution {
    public boolean parseBoolExpr(String expression) {
        Deque<Character> stk = new ArrayDeque<>();
        for (char c : expression.toCharArray()) {
            if (c != '(' && c != ')' && c != ',') {
                stk.push(c);
            } else if (c == ')') {
                int t = 0, f = 0;
                while (stk.peek() == 't' || stk.peek() == 'f') {
                    t += stk.peek() == 't' ? 1 : 0;
                    f += stk.peek() == 'f' ? 1 : 0;
                    stk.pop();
                }
                char op = stk.pop();
                c = 'f';
                if ((op == '!' && f > 0) || (op == '&' && f == 0) || (op == '|' && t > 0)) {
                    c = 't';
                }
                stk.push(c);
            }
        }
        return stk.peek() == 't';
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool parseBoolExpr(string expression) {
        stack<char> stk;
        for (char c : expression) {
            if (c != '(' && c != ')' && c != ',')
                stk.push(c);
            else if (c == ')') {
                int t = 0, f = 0;
                while (stk.top() == 't' || stk.top() == 'f') {
                    t += stk.top() == 't';
                    f += stk.top() == 'f';
                    stk.pop();
                }
                char op = stk.top();
                stk.pop();
                if (op == '!') c = f ? 't' : 'f';
                if (op == '&') c = f ? 'f' : 't';
                if (op == '|') c = t ? 't' : 'f';
                stk.push(c);
            }
        }
        return stk.top() == 't';
    }
};
```

#### Go

```go
func parseBoolExpr(expression string) bool {
	stk := []rune{}
	for _, c := range expression {
		if c != '(' && c != ')' && c != ',' {
			stk = append(stk, c)
		} else if c == ')' {
			var t, f int
			for stk[len(stk)-1] == 't' || stk[len(stk)-1] == 'f' {
				if stk[len(stk)-1] == 't' {
					t++
				} else {
					f++
				}
				stk = stk[:len(stk)-1]
			}
			op := stk[len(stk)-1]
			stk = stk[:len(stk)-1]
			c = 'f'
			if (op == '!' && f > 0) || (op == '&' && f == 0) || (op == '|' && t > 0) {
				c = 't'
			}
			stk = append(stk, c)
		}
	}
	return stk[0] == 't'
}
```

#### TypeScript

```ts
function parseBoolExpr(expression: string): boolean {
    const expr = expression;
    const n = expr.length;
    let i = 0;
    const dfs = () => {
        let res: boolean[] = [];
        while (i < n) {
            const c = expr[i++];
            if (c === ')') {
                break;
            }

            if (c === '!') {
                res.push(!dfs()[0]);
            } else if (c === '|') {
                res.push(dfs().some(v => v));
            } else if (c === '&') {
                res.push(dfs().every(v => v));
            } else if (c === 't') {
                res.push(true);
            } else if (c === 'f') {
                res.push(false);
            }
        }
        return res;
    };
    return dfs()[0];
}
```

#### Rust

```rust
impl Solution {
    fn dfs(i: &mut usize, expr: &[u8]) -> Vec<bool> {
        let n = expr.len();
        let mut res = Vec::new();
        while *i < n {
            let c = expr[*i];
            *i += 1;
            match c {
                b')' => {
                    break;
                }
                b't' => {
                    res.push(true);
                }
                b'f' => {
                    res.push(false);
                }
                b'!' => {
                    res.push(!Self::dfs(i, expr)[0]);
                }
                b'&' => {
                    res.push(Self::dfs(i, expr).iter().all(|v| *v));
                }
                b'|' => {
                    res.push(Self::dfs(i, expr).iter().any(|v| *v));
                }
                _ => {}
            }
        }
        res
    }

    pub fn parse_bool_expr(expression: String) -> bool {
        let expr = expression.as_bytes();
        let mut i = 0;
        Self::dfs(&mut i, expr)[0]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
