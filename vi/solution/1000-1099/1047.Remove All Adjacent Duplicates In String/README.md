---
comments: true
difficulty: Easy
rating: 1286
source: Weekly Contest 137 Q2
tags:
    - Stack
    - String
---

<!-- problem:start -->

# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string)

[中文文档](/solution/1000-1099/1047.Remove%20All%20Adjacent%20Duplicates%20In%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường. Một thao tác <strong>xóa ký tự trùng</strong> là chọn hai chữ cái <strong>liền kề</strong> và <strong>giống nhau</strong> rồi xóa chúng.</p>

<p>Lặp lại thao tác <strong>xóa ký tự trùng</strong> trên <code>s</code> cho đến khi không thể thực hiện thêm.</p>

<p>Trả về <em>chuỗi cuối cùng sau khi thực hiện hết các thao tác xóa ký tự trùng</em>. Có thể chứng minh rằng đáp án là <strong>duy nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abbaca&quot;
<strong>Đầu ra:</strong> &quot;ca&quot;
<strong>Giải thích:</strong> 
Ví dụ, trong &quot;abbaca&quot;, ta có thể xóa &quot;bb&quot; vì hai chữ cái này liền kề và giống nhau; đây là thao tác duy nhất có thể thực hiện lúc đó. Sau thao tác này, chuỗi trở thành &quot;aaca&quot;, khi đó chỉ có thể xóa &quot;aa&quot;, nên chuỗi cuối cùng là &quot;ca&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;azxxzy&quot;
<strong>Đầu ra:</strong> &quot;ay&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xóa liên tục các cặp ký tự giống nhau liền kề có thể khiến ta phải quét lại chuỗi dài $10^5$. Sau khi xóa, chỉ các ký tự ở gần vị trí đó mới trở thành liền kề, vì vậy có thể dùng stack để lưu tiền tố đã rút gọn.
>
> Nếu ký tự hiện tại bằng phần tử trên cùng của stack thì pop phần tử đó; nếu không thì push ký tự vào stack.
>
> Sau khi duyệt xong, nội dung stack là chuỗi đã được rút gọn hoàn toàn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stk = []
        for c in s:
            if stk and stk[-1] == c:
                stk.pop()
            else:
                stk.append(c)
        return ''.join(stk)
```

#### Java

```java
class Solution {
    public String removeDuplicates(String s) {
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (sb.length() > 0 && sb.charAt(sb.length() - 1) == c) {
                sb.deleteCharAt(sb.length() - 1);
            } else {
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        string stk;
        for (char c : s) {
            if (!stk.empty() && stk[stk.size() - 1] == c) {
                stk.pop_back();
            } else {
                stk += c;
            }
        }
        return stk;
    }
};
```

#### Go

```go
func removeDuplicates(s string) string {
	stk := []rune{}
	for _, c := range s {
		if len(stk) > 0 && stk[len(stk)-1] == c {
			stk = stk[:len(stk)-1]
		} else {
			stk = append(stk, c)
		}
	}
	return string(stk)
}
```

#### Rust

```rust
impl Solution {
    pub fn remove_duplicates(s: String) -> String {
        let mut stack = Vec::new();
        for c in s.chars() {
            if !stack.is_empty() && *stack.last().unwrap() == c {
                stack.pop();
            } else {
                stack.push(c);
            }
        }
        stack.into_iter().collect()
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {string}
 */
var removeDuplicates = function (s) {
    const stk = [];
    for (const c of s) {
        if (stk.length && stk[stk.length - 1] == c) {
            stk.pop();
        } else {
            stk.push(c);
        }
    }
    return stk.join('');
};
```

#### C

```c
char* removeDuplicates(char* s) {
    int n = strlen(s);
    char* stack = malloc(sizeof(char) * (n + 1));
    int i = 0;
    for (int j = 0; j < n; j++) {
        char c = s[j];
        if (i && stack[i - 1] == c) {
            i--;
        } else {
            stack[i++] = c;
        }
    }
    stack[i] = '\0';
    return stack;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
