---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - String
    - Dynamic Programming
    - Parentheses
---

<!-- problem:start -->

# [678. Valid Parenthesis String](https://leetcode.com/problems/valid-parenthesis-string)

[中文文档](/solution/0600-0699/0678.Valid%20Parenthesis%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> chỉ chứa ba loại ký tự: <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code> và <code>&#39;*&#39;</code>. Hãy trả về <code>true</code> <em>nếu</em> <code>s</code> <em><strong>hợp lệ</strong></em>.</p>

<p>Các quy tắc sau xác định một chuỗi <strong>hợp lệ</strong>:</p>

<ul>
	<li>Mỗi dấu ngoặc mở <code>&#39;(&#39;</code> phải có dấu ngoặc đóng tương ứng <code>&#39;)&#39;</code>.</li>
	<li>Mỗi dấu ngoặc đóng <code>&#39;)&#39;</code> phải có dấu ngoặc mở tương ứng <code>&#39;(&#39;</code>.</li>
	<li>Dấu ngoặc mở <code>&#39;(&#39;</code> phải đứng trước dấu ngoặc đóng tương ứng <code>&#39;)&#39;</code>.</li>
	<li><code>&#39;*&#39;</code> có thể được xem là một dấu ngoặc đóng <code>&#39;)&#39;</code>, một dấu ngoặc mở <code>&#39;(&#39;</code> hoặc chuỗi rỗng <code>&quot;&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = "()"
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = "(*)"
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> s = "(*))"
<strong>Đầu ra:</strong> true
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s[i]</code> là <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code> hoặc <code>&#39;*&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> `*` có thể là dấu ngoặc mở, dấu ngoặc đóng hoặc ký tự rỗng. Backtracking với ba lựa chọn sẽ tốn kém khi độ dài lên đến $100$.
>
> Dùng interval DP: $dp[i][j]$ cho biết $s[i..j]$ có hợp lệ hay không. Một ký tự `*` riêng lẻ là hợp lệ; đoạn dài hơn có thể là một cặp ngoặc bao quanh phần giữa hợp lệ, hoặc được tách thành hai đoạn đều hợp lệ.

<!-- thinking:end -->

Gọi `dp[i][j]` là true khi và chỉ khi đoạn `s[i], s[i+1], ..., s[j]` có thể trở thành hợp lệ. Khi đó, `dp[i][j]` chỉ đúng nếu:

- `s[i]` là `'*'` và đoạn `s[i+1], s[i+2], ..., s[j]` có thể trở thành hợp lệ;
- hoặc `s[i]` có thể được xem là `'('`, đồng thời tồn tại `k` trong `[i+1, j]` sao cho `s[k]` có thể được xem là `')'`, và hai đoạn do `s[k]` phân tách (`s[i+1: k]` và `s[k+1: j+1]`) đều có thể trở thành hợp lệ;

- Độ phức tạp thời gian: $O(n^3)$, với $n$ là độ dài chuỗi. Có $O(n^2)$ trạng thái tương ứng với các phần tử của dp và mỗi trạng thái cần trung bình $O(n)$ thao tác.
- Độ phức tạp không gian: $O(n^2)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkValidString(self, s: str) -> bool:
        n = len(s)
        dp = [[False] * n for _ in range(n)]
        for i, c in enumerate(s):
            dp[i][i] = c == '*'
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                dp[i][j] = (
                    s[i] in '(*' and s[j] in '*)' and (i + 1 == j or dp[i + 1][j - 1])
                )
                dp[i][j] = dp[i][j] or any(
                    dp[i][k] and dp[k + 1][j] for k in range(i, j)
                )
        return dp[0][-1]
```

#### Java

```java
class Solution {
    public boolean checkValidString(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        for (int i = 0; i < n; ++i) {
            dp[i][i] = s.charAt(i) == '*';
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                char a = s.charAt(i), b = s.charAt(j);
                dp[i][j] = (a == '(' || a == '*') && (b == '*' || b == ')')
                    && (i + 1 == j || dp[i + 1][j - 1]);
                for (int k = i; k < j && !dp[i][j]; ++k) {
                    dp[i][j] = dp[i][k] && dp[k + 1][j];
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkValidString(string s) {
        int n = s.size();
        vector<vector<bool>> dp(n, vector<bool>(n));
        for (int i = 0; i < n; ++i) {
            dp[i][i] = s[i] == '*';
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                char a = s[i], b = s[j];
                dp[i][j] = (a == '(' || a == '*') && (b == '*' || b == ')') && (i + 1 == j || dp[i + 1][j - 1]);
                for (int k = i; k < j && !dp[i][j]; ++k) {
                    dp[i][j] = dp[i][k] && dp[k + 1][j];
                }
            }
        }
        return dp[0][n - 1];
    }
};
```

#### Go

```go
func checkValidString(s string) bool {
	n := len(s)
	dp := make([][]bool, n)
	for i := range dp {
		dp[i] = make([]bool, n)
		dp[i][i] = s[i] == '*'
	}
	for i := n - 2; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			a, b := s[i], s[j]
			dp[i][j] = (a == '(' || a == '*') && (b == '*' || b == ')') && (i+1 == j || dp[i+1][j-1])
			for k := i; k < j && !dp[i][j]; k++ {
				dp[i][j] = dp[i][k] && dp[k+1][j]
			}
		}
	}
	return dp[0][n-1]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> DP bậc ba là quá dư cho bài này. Duyệt từ trái sang phải, xem `(` và `*` là nguồn để ghép với `)`; sau đó duyệt từ phải sang trái, xem `)` và `*` là nguồn để ghép với `(`. Cả hai lượt đều phải thành công.

<!-- thinking:end -->

Ta duyệt hai lượt: lượt đầu từ trái sang phải để đảm bảo mỗi dấu ngoặc đóng đều được ghép hợp lệ; lượt thứ hai từ phải sang trái để đảm bảo mỗi dấu ngoặc mở đều được ghép hợp lệ.

- Độ phức tạp thời gian: $O(n)$, với $n$ là độ dài chuỗi.
- Độ phức tạp không gian: $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkValidString(self, s: str) -> bool:
        x = 0
        for c in s:
            if c in '(*':
                x += 1
            elif x:
                x -= 1
            else:
                return False
        x = 0
        for c in s[::-1]:
            if c in '*)':
                x += 1
            elif x:
                x -= 1
            else:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean checkValidString(String s) {
        int x = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) != ')') {
                ++x;
            } else if (x > 0) {
                --x;
            } else {
                return false;
            }
        }
        x = 0;
        for (int i = n - 1; i >= 0; --i) {
            if (s.charAt(i) != '(') {
                ++x;
            } else if (x > 0) {
                --x;
            } else {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkValidString(string s) {
        int x = 0, n = s.size();
        for (int i = 0; i < n; ++i) {
            if (s[i] != ')') {
                ++x;
            } else if (x) {
                --x;
            } else {
                return false;
            }
        }
        x = 0;
        for (int i = n - 1; i >= 0; --i) {
            if (s[i] != '(') {
                ++x;
            } else if (x) {
                --x;
            } else {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func checkValidString(s string) bool {
	x := 0
	for _, c := range s {
		if c != ')' {
			x++
		} else if x > 0 {
			x--
		} else {
			return false
		}
	}
	x = 0
	for i := len(s) - 1; i >= 0; i-- {
		if s[i] != '(' {
			x++
		} else if x > 0 {
			x--
		} else {
			return false
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
