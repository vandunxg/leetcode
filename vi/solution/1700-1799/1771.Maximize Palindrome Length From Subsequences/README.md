---
comments: true
difficulty: Hard
rating: 2182
source: Weekly Contest 229 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1771. Maximize Palindrome Length From Subsequences](https://leetcode.com/problems/maximize-palindrome-length-from-subsequences)

[中文文档](/solution/1700-1799/1771.Maximize%20Palindrome%20Length%20From%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>word1</code> và <code>word2</code>. Bạn muốn tạo một chuỗi theo cách sau:</p>

<ul>
<li>Chọn một dãy con <strong>khác rỗng</strong> <code>subsequence1</code> từ <code>word1</code>.</li>
<li>Chọn một dãy con <strong>khác rỗng</strong> <code>subsequence2</code> từ <code>word2</code>.</li>
<li>Nối hai dãy con: <code>subsequence1 + subsequence2</code> để tạo chuỗi.</li>
</ul>

<p>Trả về <em><strong>độ dài</strong> của <strong>palindrome</strong> dài nhất có thể tạo theo cách trên. </em>Nếu không thể tạo palindrome, trả về <code>0</code>.</p>

<p><strong>Dãy con</strong> của chuỗi <code>s</code> là chuỗi có được bằng cách xóa một số ký tự (có thể không xóa ký tự nào) khỏi <code>s</code> mà không thay đổi thứ tự các ký tự còn lại.</p>

<p><strong>Palindrome</strong> là chuỗi đọc xuôi hay ngược đều giống nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> word1 = &quot;cacb&quot;, word2 = &quot;cbba&quot;
<strong>Output:</strong> 5
<strong>Explanation:</strong> Choose &quot;ab&quot; from word1 and &quot;cba&quot; from word2 to make &quot;abcba&quot;, which is a palindrome.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> word1 = &quot;ab&quot;, word2 = &quot;ab&quot;
<strong>Output:</strong> 3
<strong>Explanation:</strong> Choose &quot;ab&quot; from word1 and &quot;a&quot; from word2 to make &quot;aba&quot;, which is a palindrome.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> word1 = &quot;aa&quot;, word2 = &quot;bb&quot;
<strong>Output:</strong> 0
<strong>Explanation:</strong> You cannot construct a palindrome from the described method, so return 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length, word2.length &lt;= 1000</code></li>
<li><code>word1</code> và <code>word2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Chọn một dãy con khác rỗng từ mỗi từ rồi nối chúng thành một palindrome. Đây là bài toán LPS trên $s=\textit{word1}+\textit{word2}$, trong đó hai đầu phải đến từ hai từ khác nhau.
>
> $f[i][j]$ là LPS của $s[i..j]$. Nếu hai đầu bằng nhau, cộng $2$ vào kết quả bên trong; ghi nhận độ dài khi $i$ thuộc từ đầu và $j$ thuộc từ thứ hai. Nếu hai đầu khác nhau, chọn trạng thái tốt hơn khi bỏ một đầu.

<!-- thinking:end -->

Trước hết, nối hai chuỗi `word1` và `word2` để được chuỗi $s$. Khi đó, bài toán trở thành tìm độ dài dãy con đối xứng dài nhất trong $s$. Tuy nhiên, khi tính đáp án, cần đảm bảo palindrome có ít nhất một ký tự từ `word1` và một ký tự từ `word2`.

Đặt $f[i][j]$ là độ dài dãy con đối xứng dài nhất trong đoạn con của $s$ có phạm vi chỉ số $[i, j]$.

Nếu $s[i] = s[j]$, thì $s[i]$ và $s[j]$ phải thuộc dãy con đối xứng dài nhất, khi đó $f[i][j] = f[i + 1][j - 1] + 2$. Đồng thời, cần kiểm tra $s[i]$ và $s[j]$ có lần lượt đến từ `word1` và `word2` hay không. Nếu có, cập nhật đáp án bằng $ans=\max(ans, f[i][j])$.

Nếu $s[i] \neq s[j]$, thì $s[i]$ và $s[j]$ chắc chắn không thể cùng xuất hiện trong dãy con đối xứng dài nhất, khi đó $f[i][j] = max(f[i + 1][j], f[i][j - 1])$.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPalindrome(self, word1: str, word2: str) -> int:
        s = word1 + word2
        n = len(s)
        f = [[0] * n for _ in range(n)]
        for i in range(n):
            f[i][i] = 1
        ans = 0
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                if s[i] == s[j]:
                    f[i][j] = f[i + 1][j - 1] + 2
                    if i < len(word1) <= j:
                        ans = max(ans, f[i][j])
                else:
                    f[i][j] = max(f[i + 1][j], f[i][j - 1])
        return ans
```

#### Java

```java
class Solution {
    public int longestPalindrome(String word1, String word2) {
        String s = word1 + word2;
        int n = s.length();
        int[][] f = new int[n][n];
        for (int i = 0; i < n; ++i) {
            f[i][i] = 1;
        }
        int ans = 0;
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = f[i + 1][j - 1] + 2;
                    if (i < word1.length() && j >= word1.length()) {
                        ans = Math.max(ans, f[i][j]);
                    }
                } else {
                    f[i][j] = Math.max(f[i + 1][j], f[i][j - 1]);
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
    int longestPalindrome(string word1, string word2) {
        string s = word1 + word2;
        int n = s.size();
        int f[n][n];
        memset(f, 0, sizeof f);
        for (int i = 0; i < n; ++i) f[i][i] = 1;
        int ans = 0;
        for (int i = n - 2; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s[i] == s[j]) {
                    f[i][j] = f[i + 1][j - 1] + 2;
                    if (i < word1.size() && j >= word1.size()) {
                        ans = max(ans, f[i][j]);
                    }
                } else {
                    f[i][j] = max(f[i + 1][j], f[i][j - 1]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestPalindrome(word1 string, word2 string) (ans int) {
	s := word1 + word2
	n := len(s)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
		f[i][i] = 1
	}
	for i := n - 2; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			if s[i] == s[j] {
				f[i][j] = f[i+1][j-1] + 2
				if i < len(word1) && j >= len(word1) && ans < f[i][j] {
					ans = f[i][j]
				}
			} else {
				f[i][j] = max(f[i+1][j], f[i][j-1])
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestPalindrome(word1: string, word2: string): number {
    const s = word1 + word2;
    const n = s.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: n }, () => 0));
    for (let i = 0; i < n; ++i) {
        f[i][i] = 1;
    }
    let ans = 0;
    for (let i = n - 2; ~i; --i) {
        for (let j = i + 1; j < n; ++j) {
            if (s[i] === s[j]) {
                f[i][j] = f[i + 1][j - 1] + 2;
                if (i < word1.length && j >= word1.length) {
                    ans = Math.max(ans, f[i][j]);
                }
            } else {
                f[i][j] = Math.max(f[i + 1][j], f[i][j - 1]);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_palindrome(word1: String, word2: String) -> i32 {
        let s: Vec<char> = format!("{}{}", word1, word2).chars().collect();
        let n = s.len();
        let mut f = vec![vec![0; n]; n];
        for i in 0..n {
            f[i][i] = 1;
        }
        let mut ans = 0;
        for i in (0..n - 1).rev() {
            for j in i + 1..n {
                if s[i] == s[j] {
                    f[i][j] = f[i + 1][j - 1] + 2;
                    if i < word1.len() && j >= word1.len() {
                        ans = ans.max(f[i][j]);
                    }
                } else {
                    f[i][j] = f[i + 1][j].max(f[i][j - 1]);
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
