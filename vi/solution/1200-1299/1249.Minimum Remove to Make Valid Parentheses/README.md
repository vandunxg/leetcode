---
comments: true
difficulty: Medium
rating: 1657
source: Weekly Contest 161 Q3
tags:
    - Stack
    - String
---

<!-- problem:start -->

# [1249. Minimum Remove to Make Valid Parentheses](https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses)

[中文文档](/solution/1200-1299/1249.Minimum%20Remove%20to%20Make%20Valid%20Parentheses/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <font face="monospace">s</font> gồm các ký tự <code>&#39;(&#39;</code> , <code>&#39;)&#39;</code> và chữ cái tiếng Anh viết thường.</p>

<p>Nhiệm vụ của bạn là xóa ít dấu ngoặc nhất có thể ( <code>&#39;(&#39;</code> hoặc <code>&#39;)&#39;</code>, ở bất kỳ vị trí nào ) để chuỗi ngoặc thu được hợp lệ, rồi trả về <strong>bất kỳ</strong> chuỗi hợp lệ nào.</p>

<p>Định nghĩa chính thức: chuỗi ngoặc hợp lệ khi và chỉ khi:</p>

<ul>
	<li>Đó là chuỗi rỗng, chỉ chứa các chữ cái viết thường, hoặc</li>
	<li>Có thể viết dưới dạng <code>AB</code> (<code>A</code> nối với <code>B</code>), trong đó <code>A</code> và <code>B</code> đều là chuỗi hợp lệ, hoặc</li>
	<li>Có thể viết dưới dạng <code>(A)</code>, trong đó <code>A</code> là chuỗi hợp lệ.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;lee(t(c)o)de)&quot;
<strong>Output:</strong> &quot;lee(t(c)o)de&quot;
<strong>Giải thích:</strong> &quot;lee(t(co)de)&quot; , &quot;lee(t(c)ode)&quot; cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;a)b(c)d&quot;
<strong>Output:</strong> &quot;ab(c)d&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;))((&quot;
<strong>Output:</strong> &quot;&quot;
<strong>Giải thích:</strong> Chuỗi rỗng cũng hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;(&#39;</code> , <code>&#39;)&#39;</code> hoặc một chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lượt duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xóa ít dấu ngoặc nhất để chuỗi hợp lệ, với $n \le 10^5$. Có thể dùng stack để đánh dấu các dấu ngoặc không hợp lệ; cũng có thể dùng một biến đếm: duyệt từ trái sang phải và bỏ các dấu $)$ không khớp, sau đó duyệt từ phải sang trái để bỏ các dấu $($ dư.
>
> Lượt đầu đảm bảo trong mọi prefix, số dấu ngoặc phải không vượt quá số dấu ngoặc trái; lượt sau làm tương tự theo chiều ngược lại. Các dấu ngoặc còn lại sẽ khớp nhau, còn chữ cái được giữ nguyên.

<!-- thinking:end -->

Đầu tiên, ta duyệt từ trái sang phải và loại bỏ các dấu ngoặc phải dư. Sau đó, duyệt từ phải sang trái và loại bỏ các dấu ngoặc trái dư.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

Bài toán tương tự:

- [678. Valid Parenthesis String](https://github.com/doocs/leetcode/blob/main/solution/0600-0699/0678.Valid%20Parenthesis%20String/README_EN.md)
- [2116. Check if a Parentheses String Can Be Valid](https://github.com/doocs/leetcode/blob/main/solution/2100-2199/2116.Check%20if%20a%20Parentheses%20String%20Can%20Be%20Valid/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        stk = []
        x = 0
        for c in s:
            if c == ')' and x == 0:
                continue
            if c == '(':
                x += 1
            elif c == ')':
                x -= 1
            stk.append(c)
        x = 0
        ans = []
        for c in stk[::-1]:
            if c == '(' and x == 0:
                continue
            if c == ')':
                x += 1
            elif c == '(':
                x -= 1
            ans.append(c)
        return ''.join(ans[::-1])
```

#### Java

```java
class Solution {
    public String minRemoveToMakeValid(String s) {
        Deque<Character> stk = new ArrayDeque<>();
        int x = 0;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (c == ')' && x == 0) {
                continue;
            }
            if (c == '(') {
                ++x;
            } else if (c == ')') {
                --x;
            }
            stk.push(c);
        }
        StringBuilder ans = new StringBuilder();
        x = 0;
        while (!stk.isEmpty()) {
            char c = stk.pop();
            if (c == '(' && x == 0) {
                continue;
            }
            if (c == ')') {
                ++x;
            } else if (c == '(') {
                --x;
            }
            ans.append(c);
        }
        return ans.reverse().toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        string stk;
        int x = 0;
        for (char& c : s) {
            if (c == ')' && x == 0) continue;
            if (c == '(')
                ++x;
            else if (c == ')')
                --x;
            stk.push_back(c);
        }
        string ans;
        x = 0;
        while (stk.size()) {
            char c = stk.back();
            stk.pop_back();
            if (c == '(' && x == 0) continue;
            if (c == ')')
                ++x;
            else if (c == '(')
                --x;
            ans.push_back(c);
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func minRemoveToMakeValid(s string) string {
	stk := []byte{}
	x := 0
	for i := range s {
		c := s[i]
		if c == ')' && x == 0 {
			continue
		}
		if c == '(' {
			x++
		} else if c == ')' {
			x--
		}
		stk = append(stk, c)
	}
	ans := []byte{}
	x = 0
	for i := len(stk) - 1; i >= 0; i-- {
		c := stk[i]
		if c == '(' && x == 0 {
			continue
		}
		if c == ')' {
			x++
		} else if c == '(' {
			x--
		}
		ans = append(ans, c)
	}
	for i, j := 0, len(ans)-1; i < j; i, j = i+1, j-1 {
		ans[i], ans[j] = ans[j], ans[i]
	}
	return string(ans)
}
```

#### TypeScript

```ts
function minRemoveToMakeValid(s: string): string {
    let left = 0;
    let right = 0;
    for (const c of s) {
        if (c === '(') {
            left++;
        } else if (c === ')') {
            if (right < left) {
                right++;
            }
        }
    }

    let hasLeft = 0;
    let res = '';
    for (const c of s) {
        if (c === '(') {
            if (hasLeft < right) {
                hasLeft++;
                res += c;
            }
        } else if (c === ')') {
            if (hasLeft != 0 && right !== 0) {
                right--;
                hasLeft--;
                res += c;
            }
        } else {
            res += c;
        }
    }
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_remove_to_make_valid(s: String) -> String {
        let bs = s.as_bytes();
        let mut right = {
            let mut left = 0;
            let mut right = 0;
            for c in bs.iter() {
                match c {
                    &b'(' => {
                        left += 1;
                    }
                    &b')' if right < left => {
                        right += 1;
                    }
                    _ => {}
                }
            }
            right
        };
        let mut has_left = 0;
        let mut res = vec![];
        for c in bs.iter() {
            match c {
                &b'(' => {
                    if has_left < right {
                        has_left += 1;
                        res.push(*c);
                    }
                }
                &b')' => {
                    if has_left != 0 && right != 0 {
                        right -= 1;
                        has_left -= 1;
                        res.push(*c);
                    }
                }
                _ => {
                    res.push(*c);
                }
            }
        }
        String::from_utf8_lossy(&res).to_string()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
