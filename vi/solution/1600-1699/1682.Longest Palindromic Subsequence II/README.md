---
comments: true
difficulty: Medium
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1682. Longest Palindromic Subsequence II 🔒](https://leetcode.com/problems/longest-palindromic-subsequence-ii)

[中文文档](/solution/1600-1699/1682.Longest%20Palindromic%20Subsequence%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Dãy con của chuỗi <code>s</code> được gọi là <strong>dãy con đối xứng tốt</strong> nếu:</p>

<ul>
	<li>Nó là một dãy con của <code>s</code>.</li>
	<li>Nó là chuỗi đối xứng (đọc ngược vẫn có giá trị như cũ).</li>
	<li>Nó có độ dài <strong>chẵn</strong>.</li>
	<li>Không có hai ký tự liên tiếp nào bằng nhau, ngoại trừ hai ký tự ở giữa.</li>
</ul>

<p>Ví dụ, nếu <code>s = &quot;abcabcabb&quot;</code>, thì <code>&quot;abba&quot;</code> là một <strong>dãy con đối xứng tốt</strong>, còn <code>&quot;bcb&quot;</code> (độ dài không chẵn) và <code>&quot;bbbb&quot;</code> (có các ký tự liên tiếp bằng nhau) thì không.</p>

<p>Cho chuỗi <code>s</code>, hãy trả về <em><strong>độ dài</strong> của <strong>dãy con đối xứng tốt dài nhất</strong> trong </em><code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;bbabab&quot;
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Dãy con đối xứng tốt dài nhất của s là &quot;baab&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;dcbccacdb&quot;
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Dãy con đối xứng tốt dài nhất của s là &quot;dccd&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 250</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con đối xứng tốt có độ dài ít nhất $2$ và các cặp ký tự liên tiếp phải khác nhau. LPS thông thường chỉ cần hai đầu khoảng; ở đây ta còn phải nhớ ký tự của cặp gần nhất.
>
> $dfs(i,j,x)$ xét đoạn $[i,j]$ sau một cặp ký tự $x$. Hai đầu bằng nhau và khác $x$ tạo thành cặp mới; nếu không, thu hẹp một đầu. Ghi nhớ có độ phức tạp $O(n^2\lvert \Sigma \rvert)$.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i, j, x)$ biểu diễn độ dài dãy con đối xứng "tốt" dài nhất kết thúc bằng ký tự $x$ trong đoạn chỉ số $[i, j]$ của chuỗi $s$. Đáp án là $dfs(0, n - 1, 26)$.

Quá trình tính hàm $dfs(i, j, x)$ như sau:

- Nếu $i >= j$, thì $dfs(i, j, x) = 0$;
- Nếu $s[i] = s[j]$ và $s[i] \neq x$, thì $dfs(i, j, x) = dfs(i + 1, j - 1, s[i]) + 2$;
- Nếu $s[i] \neq s[j]$, thì $dfs(i, j, x) = max(dfs(i + 1, j, x), dfs(i, j - 1, x))$.

Trong quá trình này, ta dùng tìm kiếm có ghi nhớ để tránh tính lại.

Độ phức tạp thời gian là $O(n^2 \times C)$, trong đó $n$ là độ dài chuỗi $s$ và $C$ là kích thước tập ký tự. Trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        @cache
        def dfs(i, j, x):
            if i >= j:
                return 0
            if s[i] == s[j] and s[i] != x:
                return dfs(i + 1, j - 1, s[i]) + 2
            return max(dfs(i + 1, j, x), dfs(i, j - 1, x))

        ans = dfs(0, len(s) - 1, '')
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private int[][][] f;
    private String s;

    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        this.s = s;
        f = new int[n][n][27];
        for (var a : f) {
            for (var b : a) {
                Arrays.fill(b, -1);
            }
        }
        return dfs(0, n - 1, 26);
    }

    private int dfs(int i, int j, int x) {
        if (i >= j) {
            return 0;
        }
        if (f[i][j][x] != -1) {
            return f[i][j][x];
        }
        int ans = 0;
        if (s.charAt(i) == s.charAt(j) && s.charAt(i) - 'a' != x) {
            ans = dfs(i + 1, j - 1, s.charAt(i) - 'a') + 2;
        } else {
            ans = Math.max(dfs(i + 1, j, x), dfs(i, j - 1, x));
        }
        f[i][j][x] = ans;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int f[251][251][27];

    int longestPalindromeSubseq(string s) {
        int n = s.size();
        memset(f, -1, sizeof f);
        function<int(int, int, int)> dfs = [&](int i, int j, int x) -> int {
            if (i >= j) return 0;
            if (f[i][j][x] != -1) return f[i][j][x];
            int ans = 0;
            if (s[i] == s[j] && s[i] - 'a' != x)
                ans = dfs(i + 1, j - 1, s[i] - 'a') + 2;
            else
                ans = max(dfs(i + 1, j, x), dfs(i, j - 1, x));
            f[i][j][x] = ans;
            return ans;
        };
        return dfs(0, n - 1, 26);
    }
};
```

#### Go

```go
func longestPalindromeSubseq(s string) int {
	n := len(s)
	f := make([][][]int, n)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, 27)
			for k := range f[i][j] {
				f[i][j][k] = -1
			}
		}
	}
	var dfs func(i, j, x int) int
	dfs = func(i, j, x int) int {
		if i >= j {
			return 0
		}
		if f[i][j][x] != -1 {
			return f[i][j][x]
		}
		ans := 0
		if s[i] == s[j] && int(s[i]-'a') != x {
			ans = dfs(i+1, j-1, int(s[i]-'a')) + 2
		} else {
			ans = max(dfs(i+1, j, x), dfs(i, j-1, x))
		}
		f[i][j][x] = ans
		return ans
	}
	return dfs(0, n-1, 26)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
