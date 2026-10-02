---
comments: true
difficulty: Easy
rating: 1221
source: Weekly Contest 218 Q1
tags:
    - String
---

<!-- problem:start -->

# [1678. Goal Parser Interpretation](https://leetcode.com/problems/goal-parser-interpretation)

[中文文档](/solution/1600-1699/1678.Goal%20Parser%20Interpretation/README.md)

## Mô tả

<!-- description:start -->

<p>Ta có một <strong>Goal Parser</strong> có thể phân tích chuỗi <code>command</code>. Chuỗi <code>command</code> gồm các ký hiệu <code>&quot;G&quot;</code>, <code>&quot;()&quot;</code> và/hoặc <code>&quot;(al)&quot;</code> theo một thứ tự nào đó. Goal Parser diễn giải <code>&quot;G&quot;</code> thành chuỗi <code>&quot;G&quot;</code>, <code>&quot;()&quot;</code> thành chuỗi <code>&quot;o&quot;</code> và <code>&quot;(al)&quot;</code> thành chuỗi <code>&quot;al&quot;</code>. Các chuỗi sau khi diễn giải được nối lại theo thứ tự ban đầu.</p>

<p>Cho chuỗi <code>command</code>, hãy trả về <em>kết quả diễn giải <code>command</code> của <strong>Goal Parser</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> command = &quot;G()(al)&quot;
<strong>Output:</strong> &quot;Goal&quot;
<strong>Giải thích:</strong>&nbsp;Goal Parser diễn giải command như sau:
G -&gt; G
() -&gt; o
(al) -&gt; al
Kết quả sau cùng là &quot;Goal&quot;.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> command = &quot;G()()()()(al)&quot;
<strong>Output:</strong> &quot;Gooooal&quot;
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> command = &quot;(al)G(al)()()G&quot;
<strong>Output:</strong> &quot;alGalooG&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= command.length &lt;= 100</code></li>
	<li><code>command</code> gồm <code>&quot;G&quot;</code>, <code>&quot;()&quot;</code> và/hoặc <code>&quot;(al)&quot;</code> theo một thứ tự nào đó.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thay thế chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> command chỉ gồm `G`, `()` và `(al)`, có độ dài tối đa $100$. Phân tích chuỗi chỉ cần thay `()` bằng `o` và `(al)` bằng `al`.

<!-- thinking:end -->

Theo đề bài, ta chỉ cần thay `"()"` bằng `'o'` và `"(al)"` bằng `"al"` trong chuỗi `command`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def interpret(self, command: str) -> str:
        return command.replace('()', 'o').replace('(al)', 'al')
```

#### Java

```java
class Solution {
    public String interpret(String command) {
        return command.replace("()", "o").replace("(al)", "al");
    }
}
```

#### C++

```cpp
class Solution {
public:
    string interpret(string command) {
        while (command.find("()") != -1) command.replace(command.find("()"), 2, "o");
        while (command.find("(al)") != -1) command.replace(command.find("(al)"), 4, "al");
        return command;
    }
};
```

#### Go

```go
func interpret(command string) string {
	command = strings.ReplaceAll(command, "()", "o")
	command = strings.ReplaceAll(command, "(al)", "al")
	return command
}
```

#### TypeScript

```ts
function interpret(command: string): string {
    return command.replace(/\(\)/g, 'o').replace(/\(al\)/g, 'al');
}
```

#### Rust

```rust
impl Solution {
    pub fn interpret(command: String) -> String {
        command.replace("()", "o").replace("(al)", "al")
    }
}
```

#### C

```c
char* interpret(char* command) {
    int n = strlen(command);
    char* ans = malloc(sizeof(char) * n + 1);
    int i = 0;
    for (int j = 0; j < n; j++) {
        char c = command[j];
        if (c == 'G') {
            ans[i++] = 'G';
        } else if (c == '(') {
            if (command[j + 1] == ')') {
                ans[i++] = 'o';
            } else {
                ans[i++] = 'a';
                ans[i++] = 'l';
            }
        }
    }
    ans[i] = '\0';
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tạo chuỗi mới qua hai lần thay thế. Một lần duyệt duy nhất sẽ giữ `G`, ghi `o` cho `()` và `al` cho trường hợp còn lại, không cần tạo thêm các bản sao toàn bộ chuỗi.

<!-- thinking:end -->

Ta cũng có thể duyệt chuỗi `command`. Với mỗi ký tự $c$:

- Nếu là `'G'`, thêm trực tiếp $c$ vào chuỗi kết quả;
- Nếu là `'('`, kiểm tra ký tự tiếp theo có phải `')'` không. Nếu đúng, thêm `'o'`; nếu không, thêm `"al"` vào chuỗi kết quả.

Sau khi duyệt xong, trả về chuỗi kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def interpret(self, command: str) -> str:
        ans = []
        for i, c in enumerate(command):
            if c == 'G':
                ans.append(c)
            elif c == '(':
                ans.append('o' if command[i + 1] == ')' else 'al')
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String interpret(String command) {
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < command.length(); ++i) {
            char c = command.charAt(i);
            if (c == 'G') {
                ans.append(c);
            } else if (c == '(') {
                ans.append(command.charAt(i + 1) == ')' ? "o" : "al");
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
    string interpret(string command) {
        string ans;
        for (int i = 0; i < command.size(); ++i) {
            char c = command[i];
            if (c == 'G')
                ans += c;
            else if (c == '(')
                ans += command[i + 1] == ')' ? "o" : "al";
        }
        return ans;
    }
};
```

#### Go

```go
func interpret(command string) string {
	ans := &strings.Builder{}
	for i, c := range command {
		if c == 'G' {
			ans.WriteRune(c)
		} else if c == '(' {
			if command[i+1] == ')' {
				ans.WriteByte('o')
			} else {
				ans.WriteString("al")
			}
		}
	}
	return ans.String()
}
```

#### TypeScript

```ts
function interpret(command: string): string {
    const n = command.length;
    const ans: string[] = [];
    for (let i = 0; i < n; i++) {
        const c = command[i];
        if (c === 'G') {
            ans.push(c);
        } else if (c === '(') {
            ans.push(command[i + 1] === ')' ? 'o' : 'al');
        }
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn interpret(command: String) -> String {
        let mut ans = String::new();
        let bs = command.as_bytes();
        for i in 0..bs.len() {
            if bs[i] == b'G' {
                ans.push_str("G");
            }
            if bs[i] == b'(' {
                ans.push_str({
                    if bs[i + 1] == b')' {
                        "o"
                    } else {
                        "al"
                    }
                });
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
