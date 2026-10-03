---
comments: true
difficulty: Medium
rating: 1533
source: Weekly Contest 249 Q2
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [1930. Unique Length-3 Palindromic Subsequences](https://leetcode.com/problems/unique-length-3-palindromic-subsequences)

[中文文档](/solution/1900-1999/1930.Unique%20Length-3%20Palindromic%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, hãy trả về <em>số lượng <strong>palindrome độ dài ba khác nhau</strong> là một <strong>chuỗi con</strong> của </em><code>s</code>.</p>

<p>Lưu ý rằng dù có nhiều cách tạo ra cùng một chuỗi con, chuỗi con đó vẫn chỉ được tính <strong>một lần</strong>.</p>

<p><strong>Palindrome</strong> là một chuỗi đọc xuôi hay đọc ngược đều giống nhau.</p>

<p><strong>Chuỗi con</strong> của một chuỗi là chuỗi mới được tạo từ chuỗi ban đầu bằng cách xóa đi một số ký tự (có thể không xóa ký tự nào) mà không thay đổi thứ tự tương đối của các ký tự còn lại.</p>

<ul>
	<li>Ví dụ, <code>&quot;ace&quot;</code> là một chuỗi con của <code>&quot;<u>a</u>b<u>c</u>d<u>e</u>&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabca&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 3 chuỗi con palindrome độ dài 3 là:
- &quot;aba&quot; (chuỗi con của &quot;<u>a</u>a<u>b</u>c<u>a</u>&quot;)
- &quot;aaa&quot; (chuỗi con của &quot;<u>aa</u>bc<u>a</u>&quot;)
- &quot;aca&quot; (chuỗi con của &quot;<u>a</u>ab<u>ca</u>&quot;)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;adc&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có chuỗi con palindrome độ dài 3 nào trong &quot;adc&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bbcbaba&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 4 chuỗi con palindrome độ dài 3 là:
- &quot;bbb&quot; (chuỗi con của &quot;<u>bb</u>c<u>b</u>aba&quot;)
- &quot;bcb&quot; (chuỗi con của &quot;<u>b</u>b<u>cb</u>aba&quot;)
- &quot;bab&quot; (chuỗi con của &quot;<u>b</u>bcb<u>ab</u>a&quot;)
- &quot;aba&quot; (chuỗi con của &quot;bbcb<u>aba</u>&quot;)
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê hai ký tự đầu mút + Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con palindrome độ dài-$3$ có dạng $c\_c$. Việc liệt kê các bộ ba là không thể với $n\le 10^5$.
>
> Chỉ có $26$ chữ cái, nên ta liệt kê hai ký tự ở hai đầu. Với một $c$ cố định, số ký tự khác nhau nằm giữa lần xuất hiện đầu tiên và lần xuất hiện cuối cùng của nó chính là số palindrome khác nhau có hai đầu là $c$.
>
> $\texttt{find}/\texttt{rfind}$ xác định hai đầu, còn một set đếm các ký tự ở giữa, với độ phức tạp $O(n|\Sigma|)$.

<!-- thinking:end -->

Vì chuỗi chỉ chứa các chữ cái tiếng Anh thường, ta có thể liệt kê trực tiếp tất cả các cặp ký tự ở hai đầu. Với mỗi cặp ký tự đầu mút c, ta tìm vị trí xuất hiện đầu tiên và cuối cùng của chúng trong chuỗi, lần lượt là $l$ và $r$. Nếu $r - l > 1$, ta đã tìm được một chuỗi con palindrome thỏa mãn điều kiện. Khi đó, ta đếm số ký tự khác nhau trong đoạn $[l+1,..r-1]$, đây chính là số chuỗi con palindrome có $c$ làm ký tự ở hai đầu, rồi cộng vào đáp án.

Sau khi liệt kê tất cả các cặp, ta thu được đáp án.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$, trong đó $n$ là độ dài chuỗi và $\Sigma$ là kích thước của tập ký tự. Trong bài này, $|\Sigma| = 26$. Độ phức tạp không gian là $O(|\Sigma|)$ hoặc $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPalindromicSubsequence(self, s: str) -> int:
        ans = 0
        for c in ascii_lowercase:
            l, r = s.find(c), s.rfind(c)
            if r - l > 1:
                ans += len(set(s[l + 1 : r]))
        return ans
```

#### Java

```java
class Solution {
    public int countPalindromicSubsequence(String s) {
        int ans = 0;
        for (char c = 'a'; c <= 'z'; ++c) {
            int l = s.indexOf(c), r = s.lastIndexOf(c);
            int mask = 0;
            for (int i = l + 1; i < r; ++i) {
                int j = s.charAt(i) - 'a';
                if ((mask >> j & 1) == 0) {
                    mask |= 1 << j;
                    ++ans;
                }
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
    int countPalindromicSubsequence(string s) {
        int ans = 0;
        for (char c = 'a'; c <= 'z'; ++c) {
            int l = s.find_first_of(c), r = s.find_last_of(c);
            int mask = 0;
            for (int i = l + 1; i < r; ++i) {
                int j = s[i] - 'a';
                if (mask >> j & 1 ^ 1) {
                    mask |= 1 << j;
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countPalindromicSubsequence(s string) (ans int) {
	for c := 'a'; c <= 'z'; c++ {
		l, r := strings.Index(s, string(c)), strings.LastIndex(s, string(c))
		mask := 0
		for i := l + 1; i < r; i++ {
			j := int(s[i] - 'a')
			if mask>>j&1 == 0 {
				mask |= 1 << j
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countPalindromicSubsequence(s: string): number {
    let ans = 0;
    const a = 'a'.charCodeAt(0);
    for (let ch = 0; ch < 26; ++ch) {
        const c = String.fromCharCode(ch + a);
        const l = s.indexOf(c);
        const r = s.lastIndexOf(c);
        let mask = 0;
        for (let i = l + 1; i < r; ++i) {
            const j = s.charCodeAt(i) - a;
            if (((mask >> j) & 1) ^ 1) {
                mask |= 1 << j;
                ++ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_palindromic_subsequence(s: String) -> i32 {
        let s_bytes = s.as_bytes();
        let mut ans = 0;
        for c in b'a'..=b'z' {
            if let (Some(l), Some(r)) = (
                s_bytes.iter().position(|&ch| ch == c),
                s_bytes.iter().rposition(|&ch| ch == c),
            ) {
                let mut mask = 0u32;
                for i in (l + 1)..r {
                    let j = (s_bytes[i] - b'a') as u32;
                    if (mask >> j & 1) == 0 {
                        mask |= 1 << j;
                        ans += 1;
                    }
                }
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var countPalindromicSubsequence = function (s) {
    let ans = 0;
    const a = 'a'.charCodeAt(0);
    for (let ch = 0; ch < 26; ++ch) {
        const c = String.fromCharCode(ch + a);
        const l = s.indexOf(c);
        const r = s.lastIndexOf(c);
        let mask = 0;
        for (let i = l + 1; i < r; ++i) {
            const j = s.charCodeAt(i) - a;
            if (((mask >> j) & 1) ^ 1) {
                mask |= 1 << j;
                ++ans;
            }
        }
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int CountPalindromicSubsequence(string s) {
        int ans = 0;
        for (char c = 'a'; c <= 'z'; ++c) {
            int l = s.IndexOf(c), r = s.LastIndexOf(c);
            int mask = 0;
            for (int i = l + 1; i < r; ++i) {
                int j = s[i] - 'a';
                if ((mask >> j & 1) == 0) {
                    mask |= 1 << j;
                    ++ans;
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
