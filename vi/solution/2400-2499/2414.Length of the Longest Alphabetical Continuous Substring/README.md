---
comments: true
difficulty: Medium
rating: 1221
source: Weekly Contest 311 Q2
tags:
    - String
---

<!-- problem:start -->

# [2414. Length of the Longest Alphabetical Continuous Substring](https://leetcode.com/problems/length-of-the-longest-alphabetical-continuous-substring)

[中文文档](/solution/2400-2499/2414.Length%20of%20the%20Longest%20Alphabetical%20Continuous%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Chuỗi liên tiếp theo thứ tự bảng chữ cái</strong> là chuỗi gồm các chữ cái liên tiếp trong bảng chữ cái. Nói cách khác, đó là một chuỗi con bất kỳ của chuỗi <code>&quot;abcdefghijklmnopqrstuvwxyz&quot;</code>.</p>

<ul>
	<li>Ví dụ, <code>&quot;abc&quot;</code> là một chuỗi liên tiếp theo thứ tự bảng chữ cái, còn <code>&quot;acb&quot;</code> và <code>&quot;za&quot;</code> thì không.</li>
</ul>

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái viết thường, hãy trả về <em>độ dài của chuỗi con liên tiếp theo thứ tự bảng chữ cái <strong>dài nhất</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abacaba&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 4 chuỗi con liên tiếp phân biệt: &quot;a&quot;, &quot;b&quot;, &quot;c&quot; và &quot;ab&quot;.
&quot;ab&quot; là chuỗi con liên tiếp dài nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcde&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> &quot;abcde&quot; là chuỗi con liên tiếp dài nhất.
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

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Với chuỗi có độ dài $n\le 10^5$, ta không cần liệt kê các điểm đầu và điểm cuối của mọi chuỗi con. Nếu mã ASCII của hai ký tự liên tiếp chênh nhau một đơn vị, ta kéo dài chuỗi hiện tại; nếu không, đặt lại độ dài của nó về $1$. Duyệt một lần là đủ để ghi nhận độ dài lớn nhất.

<!-- thinking:end -->

Ta có thể duyệt chuỗi $s$ và dùng biến $\textit{ans}$ để lưu độ dài của chuỗi con liên tiếp theo thứ tự bảng chữ cái dài nhất, đồng thời dùng một biến $\textit{cnt}$ để lưu độ dài của chuỗi con liên tiếp hiện tại. Ban đầu, $\textit{ans} = \textit{cnt} = 1$.

Tiếp theo, ta bắt đầu duyệt chuỗi $s$ từ ký tự có chỉ số $1$. Với mỗi ký tự $s[i]$, nếu $s[i] - s[i - 1] = 1$, điều đó có nghĩa là ký tự hiện tại và ký tự trước đó liên tiếp nhau. Khi đó, $\textit{cnt} = \textit{cnt} + 1$, rồi cập nhật $\textit{ans} = \max(\textit{ans}, \textit{cnt})$. Ngược lại, nếu ký tự hiện tại và ký tự trước đó không liên tiếp nhau, ta đặt $\textit{cnt} = 1$.

Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestContinuousSubstring(self, s: str) -> int:
        ans = cnt = 1
        for x, y in pairwise(map(ord, s)):
            if y - x == 1:
                cnt += 1
                ans = max(ans, cnt)
            else:
                cnt = 1
        return ans
```

#### Java

```java
class Solution {
    public int longestContinuousSubstring(String s) {
        int ans = 1, cnt = 1;
        for (int i = 1; i < s.length(); ++i) {
            if (s.charAt(i) - s.charAt(i - 1) == 1) {
                ans = Math.max(ans, ++cnt);
            } else {
                cnt = 1;
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
    int longestContinuousSubstring(string s) {
        int ans = 1, cnt = 1;
        for (int i = 1; i < s.size(); ++i) {
            if (s[i] - s[i - 1] == 1) {
                ans = max(ans, ++cnt);
            } else {
                cnt = 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestContinuousSubstring(s string) int {
	ans, cnt := 1, 1
	for i := range s[1:] {
		if s[i+1]-s[i] == 1 {
			cnt++
			ans = max(ans, cnt)
		} else {
			cnt = 1
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestContinuousSubstring(s: string): number {
    let [ans, cnt] = [1, 1];
    for (let i = 1; i < s.length; ++i) {
        if (s.charCodeAt(i) - s.charCodeAt(i - 1) === 1) {
            ans = Math.max(ans, ++cnt);
        } else {
            cnt = 1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_continuous_substring(s: String) -> i32 {
        let mut ans = 1;
        let mut cnt = 1;
        let s = s.as_bytes();
        for i in 1..s.len() {
            if s[i] - s[i - 1] == 1 {
                cnt += 1;
                ans = ans.max(cnt);
            } else {
                cnt = 1;
            }
        }
        ans
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int longestContinuousSubstring(char* s) {
    int n = strlen(s);
    int ans = 1, cnt = 1;
    for (int i = 1; i < n; ++i) {
        if (s[i] - s[i - 1] == 1) {
            ++cnt;
            ans = max(ans, cnt);
        } else {
            cnt = 1;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
