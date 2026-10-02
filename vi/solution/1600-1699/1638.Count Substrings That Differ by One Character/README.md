---
comments: true
difficulty: Medium
rating: 1744
source: Biweekly Contest 38 Q3
tags:
    - Hash Table
    - String
    - Dynamic Programming
    - Enumeration
---

<!-- problem:start -->

# [1638. Count Substrings That Differ by One Character](https://leetcode.com/problems/count-substrings-that-differ-by-one-character)

[中文文档](/solution/1600-1699/1638.Count%20Substrings%20That%20Differ%20by%20One%20Character/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi<code>s</code>Và<code>t</code>, hãy tìm số cách bạn có thể chọn một chuỗi con không trống của<code>s</code>và thay thế một<strong>single character</strong>bởi một ký tự khác sao cho chuỗi con kết quả là chuỗi con của<code>t</code>. Nói cách khác, hãy tìm số chuỗi con trong<code>s</code>khác với một số chuỗi con trong<code>t</code>qua<strong>exactly</strong>một nhân vật.</p>

<p>Ví dụ: các chuỗi con được gạch chân trong<code>&quot;<u>compute</u>r&quot;</code>Và<code>&quot;<u>computa</u>tion&quot;</code>chỉ khác nhau ở<code>&#39;e&#39;</code>/<code>&#39;a&#39;</code>, vì vậy đây là một cách hợp lệ.</p>

<p>Trở lại<em>the number of substrings that satisfy the condition above.</em></p>

<p>MỘT<strong>substring</strong>là một chuỗi ký tự liền kề nhau trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aba&quot;, t = &quot;baba&quot;
<strong>Output:</strong> 6
<strong>Explanation:</strong> The following are the pairs of substrings from s and t that differ by exactly 1 character:
(&quot;<u>a</u>ba&quot;, &quot;<u>b</u>aba&quot;)
(&quot;<u>a</u>ba&quot;, &quot;ba<u>b</u>a&quot;)
(&quot;ab<u>a</u>&quot;, &quot;<u>b</u>aba&quot;)
(&quot;ab<u>a</u>&quot;, &quot;ba<u>b</u>a&quot;)
(&quot;a<u>b</u>a&quot;, &quot;b<u>a</u>ba&quot;)
(&quot;a<u>b</u>a&quot;, &quot;bab<u>a</u>&quot;)
The underlined portions are the substrings that are chosen from s and t.
</pre>

​​<strong class="example">Ví dụ 2:</strong>

<pre>
<strong>Input:</strong> s = &quot;ab&quot;, t = &quot;bb&quot;
<strong>Output:</strong> 3
<strong>Explanation:</strong> The following are the pairs of substrings from s and t that differ by 1 character:
(&quot;<u>a</u>b&quot;, &quot;<u>b</u>b&quot;)
(&quot;<u>a</u>b&quot;, &quot;b<u>b</u>&quot;)
(&quot;<u>ab</u>&quot;, &quot;<u>bb</u>&quot;)
​​​​The underlined portions are the substrings that are chosen from s and t.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 100</code></li>
	<li><code>s</code>Và<code>t</code>chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Suy nghĩ**
>
> Cả hai chuỗi đều có độ dài tối đa$100$. Một cặp hợp lệ chỉ khác nhau ở một vị trí; mở rộng từ sự không khớp đó, việc chạy các ký tự bằng nhau ở mỗi bên sẽ xác định số lần cắt được thực hiện.
>
> Liệt kê$(i,j)$với$s[i]\ne t[j]$, đo các nhịp bằng nhau$l$Và$r$, và thêm$(l+1)(r+1)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubstrings(self, s: str, t: str) -> int:
        ans = 0
        m, n = len(s), len(t)
        for i, a in enumerate(s):
            for j, b in enumerate(t):
                if a != b:
                    l = r = 0
                    while i > l and j > l and s[i - l - 1] == t[j - l - 1]:
                        l += 1
                    while (
                        i + r + 1 < m and j + r + 1 < n and s[i + r + 1] == t[j + r + 1]
                    ):
                        r += 1
                    ans += (l + 1) * (r + 1)
        return ans
```

#### Java

```java
class Solution {
    public int countSubstrings(String s, String t) {
        int ans = 0;
        int m = s.length(), n = t.length();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (s.charAt(i) != t.charAt(j)) {
                    int l = 0, r = 0;
                    while (i - l > 0 && j - l > 0 && s.charAt(i - l - 1) == t.charAt(j - l - 1)) {
                        ++l;
                    }
                    while (i + r + 1 < m && j + r + 1 < n
                        && s.charAt(i + r + 1) == t.charAt(j + r + 1)) {
                        ++r;
                    }
                    ans += (l + 1) * (r + 1);
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
    int countSubstrings(string s, string t) {
        int ans = 0;
        int m = s.size(), n = t.size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (s[i] != t[j]) {
                    int l = 0, r = 0;
                    while (i - l > 0 && j - l > 0 && s[i - l - 1] == t[j - l - 1]) {
                        ++l;
                    }
                    while (i + r + 1 < m && j + r + 1 < n && s[i + r + 1] == t[j + r + 1]) {
                        ++r;
                    }
                    ans += (l + 1) * (r + 1);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSubstrings(s string, t string) (ans int) {
	m, n := len(s), len(t)
	for i, a := range s {
		for j, b := range t {
			if a != b {
				l, r := 0, 0
				for i > l && j > l && s[i-l-1] == t[j-l-1] {
					l++
				}
				for i+r+1 < m && j+r+1 < n && s[i+r+1] == t[j+r+1] {
					r++
				}
				ans += (l + 1) * (r + 1)
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Suy nghĩ**
>
> Giải pháp 1 mở rộng từ mọi điểm không khớp và lặp lại so sánh. Tính toán trước hậu tố bằng nhau dài nhất$f$kết thúc tại$(i,j)$và tiền tố bằng nhau dài nhất$g$bắt đầu từ đó.
>
> Mỗi sự không phù hợp sau đó sẽ đóng góp$(f[i][j]+1)(g[i+1][j+1]+1)$TRONG$O(mn)$tổng thời gian.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubstrings(self, s: str, t: str) -> int:
        ans = 0
        m, n = len(s), len(t)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        g = [[0] * (n + 1) for _ in range(m + 1)]
        for i, a in enumerate(s, 1):
            for j, b in enumerate(t, 1):
                if a == b:
                    f[i][j] = f[i - 1][j - 1] + 1
        for i in range(m - 1, -1, -1):
            for j in range(n - 1, -1, -1):
                if s[i] == t[j]:
                    g[i][j] = g[i + 1][j + 1] + 1
                else:
                    ans += (f[i][j] + 1) * (g[i + 1][j + 1] + 1)
        return ans
```

#### Java

```java
class Solution {
    public int countSubstrings(String s, String t) {
        int ans = 0;
        int m = s.length(), n = t.length();
        int[][] f = new int[m + 1][n + 1];
        int[][] g = new int[m + 1][n + 1];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (s.charAt(i) == t.charAt(j)) {
                    f[i + 1][j + 1] = f[i][j] + 1;
                }
            }
        }
        for (int i = m - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (s.charAt(i) == t.charAt(j)) {
                    g[i][j] = g[i + 1][j + 1] + 1;
                } else {
                    ans += (f[i][j] + 1) * (g[i + 1][j + 1] + 1);
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
    int countSubstrings(string s, string t) {
        int ans = 0;
        int m = s.length(), n = t.length();
        int f[m + 1][n + 1];
        int g[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        memset(g, 0, sizeof(g));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (s[i] == t[j]) {
                    f[i + 1][j + 1] = f[i][j] + 1;
                }
            }
        }
        for (int i = m - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (s[i] == t[j]) {
                    g[i][j] = g[i + 1][j + 1] + 1;
                } else {
                    ans += (f[i][j] + 1) * (g[i + 1][j + 1] + 1);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSubstrings(s string, t string) (ans int) {
	m, n := len(s), len(t)
	f := make([][]int, m+1)
	g := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
		g[i] = make([]int, n+1)
	}
	for i, a := range s {
		for j, b := range t {
			if a == b {
				f[i+1][j+1] = f[i][j] + 1
			}
		}
	}
	for i := m - 1; i >= 0; i-- {
		for j := n - 1; j >= 0; j-- {
			if s[i] == t[j] {
				g[i][j] = g[i+1][j+1] + 1
			} else {
				ans += (f[i][j] + 1) * (g[i+1][j+1] + 1)
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
