---
comments: true
difficulty: Hard
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [730. Count Different Palindromic Subsequences](https://leetcode.com/problems/count-different-palindromic-subsequences)

[中文文档](/solution/0700-0799/0730.Count%20Different%20Palindromic%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi s, hãy trả về <em>số lượng dãy con đối xứng không rỗng khác nhau trong</em> <code>s</code>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Dãy con của một chuỗi được tạo bằng cách xóa không hoặc nhiều ký tự khỏi chuỗi đó.</p>

<p>Một dãy là đối xứng nếu nó giống với dãy được viết theo thứ tự ngược lại.</p>

<p>Hai dãy <code>a<sub>1</sub>, a<sub>2</sub>, ...</code> và <code>b<sub>1</sub>, b<sub>2</sub>, ...</code> khác nhau nếu tồn tại một <code>i</code> sao cho <code>a<sub>i</sub> != b<sub>i</sub></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bccb&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> 6 dãy con đối xứng không rỗng khác nhau là &#39;b&#39;, &#39;c&#39;, &#39;bb&#39;, &#39;cc&#39;, &#39;bcb&#39;, &#39;bccb&#39;.
Lưu ý rằng &#39;bcb&#39; chỉ được tính một lần dù nó xuất hiện hai lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdabcdabcdabcdabcdabcdabcdabcddcbadcbadcbadcbadcbadcbadcbadcba&quot;
<strong>Đầu ra:</strong> 104860361
<strong>Giải thích:</strong> Có 3104860382 dãy con đối xứng không rỗng khác nhau; lấy modulo 10<sup>9</sup> + 7 được 104860361.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> là <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code>, <code>&#39;c&#39;</code> hoặc <code>&#39;d&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần đếm các dãy con đối xứng khác nhau; $n\le 1000$ và chỉ có bốn ký tự. Không thể liệt kê mọi dãy con, còn DP đối xứng thông thường sẽ đếm trùng cùng một chuỗi xuất hiện ở các đoạn khác nhau.
>
> Phân loại theo ký tự ở hai đầu: các palindrome được bọc bởi $c$ gồm một chuỗi mới cộng với bốn loại bên trong, hoặc chỉ có riêng $c$. Từ đó có thể dùng interval DP tiêu chuẩn trên các đoạn ngắn hơn.
>
> $dp[i][j][k]$ là số dãy con trong $s[i..j]$ bắt đầu và kết thúc bằng ký tự $k$. Nếu hai đầu đều bằng $c$, ta có $2+\sum dp[i+1][j-1]$; nếu không, thu hẹp phía không phải $c$. Cộng bốn giá trị trên đoạn $[0,n-1]$ để có đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPalindromicSubsequences(self, s: str) -> int:
        mod = 10**9 + 7
        n = len(s)
        dp = [[[0] * 4 for _ in range(n)] for _ in range(n)]
        for i, c in enumerate(s):
            dp[i][i][ord(c) - ord('a')] = 1
        for l in range(2, n + 1):
            for i in range(n - l + 1):
                j = i + l - 1
                for c in 'abcd':
                    k = ord(c) - ord('a')
                    if s[i] == s[j] == c:
                        dp[i][j][k] = 2 + sum(dp[i + 1][j - 1])
                    elif s[i] == c:
                        dp[i][j][k] = dp[i][j - 1][k]
                    elif s[j] == c:
                        dp[i][j][k] = dp[i + 1][j][k]
                    else:
                        dp[i][j][k] = dp[i + 1][j - 1][k]
        return sum(dp[0][-1]) % mod
```

#### Java

```java
class Solution {
    private final int MOD = (int) 1e9 + 7;

    public int countPalindromicSubsequences(String s) {
        int n = s.length();
        long[][][] dp = new long[n][n][4];
        for (int i = 0; i < n; ++i) {
            dp[i][i][s.charAt(i) - 'a'] = 1;
        }
        for (int l = 2; l <= n; ++l) {
            for (int i = 0; i + l <= n; ++i) {
                int j = i + l - 1;
                for (char c = 'a'; c <= 'd'; ++c) {
                    int k = c - 'a';
                    if (s.charAt(i) == c && s.charAt(j) == c) {
                        dp[i][j][k] = 2 + dp[i + 1][j - 1][0] + dp[i + 1][j - 1][1]
                            + dp[i + 1][j - 1][2] + dp[i + 1][j - 1][3];
                        dp[i][j][k] %= MOD;
                    } else if (s.charAt(i) == c) {
                        dp[i][j][k] = dp[i][j - 1][k];
                    } else if (s.charAt(j) == c) {
                        dp[i][j][k] = dp[i + 1][j][k];
                    } else {
                        dp[i][j][k] = dp[i + 1][j - 1][k];
                    }
                }
            }
        }
        long ans = 0;
        for (int k = 0; k < 4; ++k) {
            ans += dp[0][n - 1][k];
        }
        return (int) (ans % MOD);
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    int countPalindromicSubsequences(string s) {
        int mod = 1e9 + 7;
        int n = s.size();
        vector<vector<vector<ll>>> dp(n, vector<vector<ll>>(n, vector<ll>(4)));
        for (int i = 0; i < n; ++i) dp[i][i][s[i] - 'a'] = 1;
        for (int l = 2; l <= n; ++l) {
            for (int i = 0; i + l <= n; ++i) {
                int j = i + l - 1;
                for (char c = 'a'; c <= 'd'; ++c) {
                    int k = c - 'a';
                    if (s[i] == c && s[j] == c)
                        dp[i][j][k] = 2 + accumulate(dp[i + 1][j - 1].begin(), dp[i + 1][j - 1].end(), 0ll) % mod;
                    else if (s[i] == c)
                        dp[i][j][k] = dp[i][j - 1][k];
                    else if (s[j] == c)
                        dp[i][j][k] = dp[i + 1][j][k];
                    else
                        dp[i][j][k] = dp[i + 1][j - 1][k];
                }
            }
        }
        ll ans = accumulate(dp[0][n - 1].begin(), dp[0][n - 1].end(), 0ll);
        return (int) (ans % mod);
    }
};
```

#### Go

```go
func countPalindromicSubsequences(s string) int {
	mod := int(1e9) + 7
	n := len(s)
	dp := make([][][]int, n)
	for i := range dp {
		dp[i] = make([][]int, n)
		for j := range dp[i] {
			dp[i][j] = make([]int, 4)
		}
	}
	for i, c := range s {
		dp[i][i][c-'a'] = 1
	}
	for l := 2; l <= n; l++ {
		for i := 0; i+l <= n; i++ {
			j := i + l - 1
			for _, c := range [4]byte{'a', 'b', 'c', 'd'} {
				k := int(c - 'a')
				if s[i] == c && s[j] == c {
					dp[i][j][k] = 2 + (dp[i+1][j-1][0]+dp[i+1][j-1][1]+dp[i+1][j-1][2]+dp[i+1][j-1][3])%mod
				} else if s[i] == c {
					dp[i][j][k] = dp[i][j-1][k]
				} else if s[j] == c {
					dp[i][j][k] = dp[i+1][j][k]
				} else {
					dp[i][j][k] = dp[i+1][j-1][k]
				}
			}
		}
	}
	ans := 0
	for _, v := range dp[0][n-1] {
		ans += v
	}
	return ans % mod
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
