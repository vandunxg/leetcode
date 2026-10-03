---
comments: true
difficulty: Medium
rating: 1839
source: Weekly Contest 298 Q3
tags:
    - Greedy
    - Memoization
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2311. Longest Binary Subsequence Less Than or Equal to K](https://leetcode.com/problems/longest-binary-subsequence-less-than-or-equal-to-k)

[中文文档](/solution/2300-2399/2311.Longest%20Binary%20Subsequence%20Less%20Than%20or%20Equal%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code> và một số nguyên dương <code>k</code>.</p>

<p>Trả về <em>độ dài của <strong>dãy con</strong> dài nhất của </em><code>s</code><em> biểu diễn một <strong>số nhị phân</strong> nhỏ hơn hoặc bằng</em> <code>k</code>.</p>

<p>Lưu ý:</p>

<ul>
	<li>Dãy con có thể chứa <strong>các số 0 ở đầu</strong>.</li>
	<li>Chuỗi rỗng được xem là bằng <code>0</code>.</li>
	<li><strong>Dãy con</strong> là một chuỗi có thể thu được từ một chuỗi khác bằng cách xóa một số hoặc không xóa ký tự nào, nhưng không thay đổi thứ tự của các ký tự còn lại.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1001010&quot;, k = 5
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Dãy con dài nhất của s biểu diễn một số nhị phân nhỏ hơn hoặc bằng 5 là &quot;00010&quot;, vì số này bằng 2 trong hệ thập phân.
Lưu ý rằng &quot;00100&quot; và &quot;00101&quot; cũng là các đáp án có thể chọn, lần lượt bằng 4 và 5 trong hệ thập phân.
Độ dài của dãy con này là 5, nên kết quả trả về là 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00101001&quot;, k = 1
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> &quot;000001&quot; là dãy con dài nhất của s biểu diễn một số nhị phân nhỏ hơn hoặc bằng 1, vì số này bằng 1 trong hệ thập phân.
Độ dài của dãy con này là 6, nên kết quả trả về là 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con phải biểu diễn một giá trị $\le k$. Vì $|s| \le 1000$, việc liệt kê tất cả các tập con là không thể. Các số 0 không bao giờ làm tăng giá trị, nên ta nên giữ lại tất cả.
>
> Số $1$ ở vị trí cao hơn sẽ có giá trị lớn hơn, vì vậy ta duyệt từ phải sang trái và thử lấy từng số $1$. Xem độ dài hiện tại là chỉ số bit; giữ số 1 nếu giá trị mới vẫn $\le k$. Khi vượt quá khoảng $30$ bit, giá trị sẽ lớn hơn $k$, nên ta bỏ qua các số 1 đó.

<!-- thinking:end -->

Dãy con nhị phân dài nhất phải bao gồm tất cả các số $0$ trong chuỗi ban đầu. Dựa trên điều này, ta duyệt $s$ từ phải sang trái. Khi gặp một $1$, ta kiểm tra xem việc thêm $1$ này vào dãy con có giữ cho số nhị phân $v \leq k$ hay không.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubsequence(self, s: str, k: int) -> int:
        ans = v = 0
        for c in s[::-1]:
            if c == "0":
                ans += 1
            elif ans < 30 and (v | 1 << ans) <= k:
                v |= 1 << ans
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int longestSubsequence(String s, int k) {
        int ans = 0, v = 0;
        for (int i = s.length() - 1; i >= 0; --i) {
            if (s.charAt(i) == '0') {
                ++ans;
            } else if (ans < 30 && (v | 1 << ans) <= k) {
                v |= 1 << ans;
                ++ans;
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
    int longestSubsequence(string s, int k) {
        int ans = 0, v = 0;
        for (int i = s.size() - 1; ~i; --i) {
            if (s[i] == '0') {
                ++ans;
            } else if (ans < 30 && (v | 1 << ans) <= k) {
                v |= 1 << ans;
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubsequence(s string, k int) (ans int) {
	for i, v := len(s)-1, 0; i >= 0; i-- {
		if s[i] == '0' {
			ans++
		} else if ans < 30 && (v|1<<ans) <= k {
			v |= 1 << ans
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function longestSubsequence(s: string, k: number): number {
    let ans = 0;
    for (let i = s.length - 1, v = 0; ~i; --i) {
        if (s[i] == '0') {
            ++ans;
        } else if (ans < 30 && (v | (1 << ans)) <= k) {
            v |= 1 << ans;
            ++ans;
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {number} k
 * @return {number}
 */
var longestSubsequence = function (s, k) {
    let ans = 0;
    for (let i = s.length - 1, v = 0; ~i; --i) {
        if (s[i] == '0') {
            ++ans;
        } else if (ans < 30 && (v | (1 << ans)) <= k) {
            v |= 1 << ans;
            ++ans;
        }
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int LongestSubsequence(string s, int k) {
        int ans = 0, v = 0;
        for (int i = s.Length - 1; i >= 0; --i) {
            if (s[i] == '0') {
                ++ans;
            } else if (ans < 30 && (v | 1 << ans) <= k) {
                v |= 1 << ans;
                ++ans;
            }
        }
        return ans;
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_subsequence(s: String, k: i32) -> i32 {
        let mut ans = 0;
        let mut v = 0;
        let s = s.as_bytes();
        for i in (0..s.len()).rev() {
            if s[i] == b'0' {
                ans += 1;
            } else if ans < 30 && (v | (1 << ans)) <= k {
                v |= 1 << ans;
                ans += 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
