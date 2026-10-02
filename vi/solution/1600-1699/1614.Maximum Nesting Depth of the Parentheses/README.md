---
comments: true
difficulty: Easy
rating: 1322
source: Weekly Contest 210 Q1
tags:
    - Stack
    - String
    - Parentheses
---

<!-- problem:start -->

# [1614. Maximum Nesting Depth of the Parentheses](https://leetcode.com/problems/maximum-nesting-depth-of-the-parentheses)

[中文文档](/solution/1600-1699/1614.Maximum%20Nesting%20Depth%20of%20the%20Parentheses/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>s</code> là một <strong>chuỗi ngoặc hợp lệ</strong>, hãy trả về <strong>độ sâu lồng nhau</strong> của<em> </em><code>s</code>. Độ sâu lồng nhau là số ngoặc lồng nhau <strong>lớn nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;(1+(2*3)+((8)/4))+1&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số 8 nằm bên trong 3 cặp ngoặc lồng nhau trong chuỗi.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;(1)+((2))+(((3)))&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số 3 nằm bên trong 3 cặp ngoặc lồng nhau trong chuỗi.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;()(())((()()))&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">3</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> consists of digits <code>0-9</code> and characters <code>&#39;+&#39;</code>, <code>&#39;-&#39;</code>, <code>&#39;*&#39;</code>, <code>&#39;/&#39;</code>, <code>&#39;(&#39;</code>, and <code>&#39;)&#39;</code>.</li>
	<li>Đảm bảo biểu thức ngoặc <code>s</code> là một VPS.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi luôn hợp lệ và có độ dài không quá $100$. Độ sâu lồng nhau là số ngoặc mở chưa đóng lớn nhất trong quá trình quét.
>
> Chữ số và toán tử không ảnh hưởng đến độ sâu; chỉ các dấu ngoặc mới quan trọng.
>
> Biến đếm $d$ tăng khi gặp `(`, cập nhật đáp án, và giảm khi gặp `)`. Chỉ cần duyệt một lần.

<!-- thinking:end -->

Ta dùng biến $d$ để ghi nhận độ sâu hiện tại, ban đầu $d = 0$.

Duyệt chuỗi $s$. Khi gặp ngoặc trái, tăng độ sâu $d$ lên một và cập nhật đáp án bằng giá trị lớn hơn giữa độ sâu hiện tại $d$ và đáp án. Khi gặp ngoặc phải, giảm $d$ đi một.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDepth(self, s: str) -> int:
        ans = d = 0
        for c in s:
            if c == '(':
                d += 1
                ans = max(ans, d)
            elif c == ')':
                d -= 1
        return ans
```

#### Java

```java
class Solution {
    public int maxDepth(String s) {
        int ans = 0, d = 0;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (c == '(') {
                ans = Math.max(ans, ++d);
            } else if (c == ')') {
                --d;
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
    int maxDepth(string s) {
        int ans = 0, d = 0;
        for (char& c : s) {
            if (c == '(') {
                ans = max(ans, ++d);
            } else if (c == ')') {
                --d;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxDepth(s string) (ans int) {
	d := 0
	for _, c := range s {
		if c == '(' {
			d++
			ans = max(ans, d)
		} else if c == ')' {
			d--
		}
	}
	return
}
```

#### TypeScript

```ts
function maxDepth(s: string): number {
    let ans = 0;
    let d = 0;
    for (const c of s) {
        if (c === '(') {
            ans = Math.max(ans, ++d);
        } else if (c === ')') {
            --d;
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var maxDepth = function (s) {
    let ans = 0;
    let d = 0;
    for (const c of s) {
        if (c === '(') {
            ans = Math.max(ans, ++d);
        } else if (c === ')') {
            --d;
        }
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int MaxDepth(string s) {
        int ans = 0, d = 0;
        foreach(char c in s) {
            if (c == '(') {
                ans = Math.Max(ans, ++d);
            } else if (c == ')') {
                --d;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
