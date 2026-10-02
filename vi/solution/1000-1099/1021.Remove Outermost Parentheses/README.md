---
comments: true
difficulty: Easy
rating: 1311
source: Weekly Contest 131 Q1
tags:
    - Stack
    - String
    - Parentheses
---

<!-- problem:start -->

# [1021. Remove Outermost Parentheses](https://leetcode.com/problems/remove-outermost-parentheses)

[中文文档](/solution/1000-1099/1021.Remove%20Outermost%20Parentheses/README.md)

## Mô tả

<!-- description:start -->

<p>Chuỗi ngoặc hợp lệ có thể là chuỗi rỗng <code>&quot;&quot;</code>, <code>&quot;(&quot; + A + &quot;)&quot;</code>, hoặc <code>A + B</code>, trong đó <code>A</code> và <code>B</code> là các chuỗi ngoặc hợp lệ, còn <code>+</code> biểu thị phép nối chuỗi.</p>

<ul>
	<li>Ví dụ, <code>&quot;&quot;</code>, <code>&quot;()&quot;</code>, <code>&quot;(())()&quot;</code> và <code>&quot;(()(()))&quot;</code> đều là chuỗi ngoặc hợp lệ.</li>
</ul>

<p>Chuỗi ngoặc hợp lệ <code>s</code> được gọi là nguyên tố nếu nó không rỗng và không thể tách thành <code>s = A + B</code>, trong đó <code>A</code> và <code>B</code> đều là các chuỗi ngoặc hợp lệ không rỗng.</p>

<p>Cho chuỗi ngoặc hợp lệ <code>s</code>, xét phân rã nguyên tố của nó: <code>s = P<sub>1</sub> + P<sub>2</sub> + ... + P<sub>k</sub></code>, trong đó <code>P<sub>i</sub></code> là các chuỗi ngoặc hợp lệ nguyên tố.</p>

<p>Hãy trả về <code>s</code> <em>sau khi loại bỏ cặp ngoặc ngoài cùng của mỗi chuỗi nguyên tố trong phân rã nguyên tố của </em><code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(()())(())&quot;
<strong>Đầu ra:</strong> &quot;()()()&quot;
<strong>Giải thích:</strong> 
Chuỗi đầu vào là &quot;(()())(())&quot;, có phân rã nguyên tố là &quot;(()())&quot; + &quot;(())&quot;.
Sau khi loại bỏ cặp ngoặc ngoài cùng của mỗi phần, ta được &quot;()()&quot; + &quot;()&quot; = &quot;()()()&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(()())(())(()(()))&quot;
<strong>Đầu ra:</strong> &quot;()()()()(())&quot;
<strong>Giải thích:</strong> 
Chuỗi đầu vào là &quot;(()())(())(()(()))&quot;, có phân rã nguyên tố là &quot;(()())&quot; + &quot;(())&quot; + &quot;(()(()))&quot;.
Sau khi loại bỏ cặp ngoặc ngoài cùng của mỗi phần, ta được &quot;()()&quot; + &quot;()&quot; + &quot;()(())&quot; = &quot;()()()()(())&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;()()&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong> 
Chuỗi đầu vào là &quot;()()&quot;, có phân rã nguyên tố là &quot;()&quot; + &quot;()&quot;.
Sau khi loại bỏ cặp ngoặc ngoài cùng của mỗi phần, ta được &quot;&quot; + &quot;&quot; = &quot;&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;(&#39;</code> hoặc <code>&#39;)&#39;</code>.</li>
	<li><code>s</code> là một chuỗi ngoặc hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có thể tách chuỗi thành các phần nguyên tố rồi bỏ cặp ngoặc ngoài cùng của từng phần, nhưng không cần duyệt lần thứ hai. Cặp ngoặc ngoài cùng chính là các ký tự làm độ sâu chuyển từ $0$ sang lớn hơn $0$ rồi quay lại $0$.
>
> Vì vậy, dùng biến đếm độ sâu và chỉ giữ ký tự không thuộc cặp ngoặc ngoài cùng: tăng biến đếm trước khi gặp `'('` rồi giữ ký tự nếu độ sâu lớn hơn $1$; giảm biến đếm trước khi gặp `')'` rồi giữ ký tự nếu độ sâu vẫn dương.
>
> Một lượt duyệt là đủ để tạo kết quả với bộ nhớ phụ hằng số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeOuterParentheses(self, s: str) -> str:
        ans = []
        cnt = 0
        for c in s:
            if c == '(':
                cnt += 1
                if cnt > 1:
                    ans.append(c)
            else:
                cnt -= 1
                if cnt > 0:
                    ans.append(c)
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String removeOuterParentheses(String s) {
        StringBuilder ans = new StringBuilder();
        int cnt = 0;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (c == '(') {
                if (++cnt > 1) {
                    ans.append(c);
                }
            } else {
                if (--cnt > 0) {
                    ans.append(c);
                }
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        string ans;
        int cnt = 0;
        for (char& c : s) {
            if (c == '(') {
                if (++cnt > 1) {
                    ans.push_back(c);
                }
            } else {
                if (--cnt) {
                    ans.push_back(c);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeOuterParentheses(s string) string {
	ans := []rune{}
	cnt := 0
	for _, c := range s {
		if c == '(' {
			cnt++
			if cnt > 1 {
				ans = append(ans, c)
			}
		} else {
			cnt--
			if cnt > 0 {
				ans = append(ans, c)
			}
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function removeOuterParentheses(s: string): string {
    let res = '';
    let depth = 0;
    for (const c of s) {
        if (c === '(') {
            depth++;
        }
        if (depth !== 1) {
            res += c;
        }
        if (c === ')') {
            depth--;
        }
    }
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn remove_outer_parentheses(s: String) -> String {
        let mut res = String::new();
        let mut depth = 0;
        for c in s.chars() {
            if c == '(' {
                depth += 1;
            }
            if depth != 1 {
                res.push(c);
            }
            if c == ')' {
                depth -= 1;
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 rẽ nhánh theo hai loại dấu ngoặc. Mọi ký tự được gặp khi độ sâu lớn hơn $1$ đều nằm bên trong một phần nguyên tố.
>
> Ta cập nhật độ sâu khi gặp `'('`, thêm ký tự vào kết quả nếu độ sâu lớn hơn $1$, rồi giảm độ sâu khi gặp `')'`. Đây vẫn là cùng một biến đếm, nhưng chỉ cần một thao tác ghi ký tự.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeOuterParentheses(self, s: str) -> str:
        ans = []
        cnt = 0
        for c in s:
            if c == '(':
                cnt += 1
            if cnt > 1:
                ans.append(c)
            if c == ')':
                cnt -= 1
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String removeOuterParentheses(String s) {
        StringBuilder ans = new StringBuilder();
        int cnt = 0;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (c == '(') {
                ++cnt;
            }
            if (cnt > 1) {
                ans.append(c);
            }
            if (c == ')') {
                --cnt;
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        string ans;
        int cnt = 0;
        for (char& c : s) {
            if (c == '(') {
                ++cnt;
            }
            if (cnt > 1) {
                ans.push_back(c);
            }
            if (c == ')') {
                --cnt;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeOuterParentheses(s string) string {
	ans := []rune{}
	cnt := 0
	for _, c := range s {
		if c == '(' {
			cnt++
		}
		if cnt > 1 {
			ans = append(ans, c)
		}
		if c == ')' {
			cnt--
		}
	}
	return string(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
