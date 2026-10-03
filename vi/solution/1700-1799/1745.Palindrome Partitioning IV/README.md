---
comments: true
difficulty: Hard
rating: 1924
source: Weekly Contest 226 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1745. Palindrome Partitioning IV](https://leetcode.com/problems/palindrome-partitioning-iv)

[中文文档](/solution/1700-1799/1745.Palindrome%20Partitioning%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, trả về <code>true</code> <em>nếu có thể chia chuỗi</em> <code>s</code> <em>thành ba chuỗi con palindrome <strong>không rỗng</strong>. Ngược lại, trả về </em><code>false</code>.​​​​​</p>

<p>Một chuỗi được gọi là palindrome nếu nó giống chính nó khi đảo ngược.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcbdd&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>&quot;abcbdd&quot; = &quot;a&quot; + &quot;bcb&quot; + &quot;dd&quot;, và cả ba chuỗi con đều là palindrome.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bcbddxy&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích: </strong>Không thể chia s thành 3 palindrome.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code>​​​​​​ consists only of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Chia chuỗi thành ba palindrome. Kiểm tra mỗi cách chia từ đầu sẽ cần $O(n^3)$ lần kiểm tra palindrome. Vì $n$ có thể lên đến vài nghìn, ta nên tính trước các palindrome.
>
> $f[i][j]$ cho biết $s[i..j]$ có phải palindrome hay không. Sau khi điền theo độ dài đoạn, thử hai vị trí chia; tổng thời gian là $O(n^2)$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ cho biết chuỗi con của $s$ từ ký tự thứ $i$ đến ký tự thứ $j$ có phải palindrome hay không, ban đầu $f[i][j] = \textit{true}$.

Sau đó, ta tính $f[i][j]$ theo công thức chuyển trạng thái sau:

$$
f[i][j] = \begin{cases}
\textit{true}, & \text{if } s[i] = s[j] \text{ and } (i + 1 = j \text{ or } f[i + 1][j - 1]) \\
\textit{false}, & \text{otherwise}
\end{cases}
$$

Vì $f[i][j]$ phụ thuộc vào $f[i + 1][j - 1]$, ta cần duyệt $i$ từ lớn đến nhỏ và $j$ từ nhỏ đến lớn, để khi tính $f[i][j]$ thì $f[i + 1][j - 1]$ đã được tính.

Tiếp theo, ta duyệt đầu phải $i$ của chuỗi con thứ nhất và đầu phải $j$ của chuỗi con thứ hai. Chuỗi con thứ ba bắt đầu trong đoạn $[j + 1, n - 1]$, với $n$ là độ dài chuỗi $s$. Nếu các chuỗi con $s[0..i]$, $s[i+1..j]$ và $s[j+1..n-1]$ đều là palindrome, ta tìm được cách chia hợp lệ và trả về $\textit{true}$.

Sau khi duyệt mọi cách chia, nếu không tìm thấy cách chia hợp lệ thì trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkPartitioning(self, s: str) -> bool:
        n = len(s)
        f = [[True] * n for _ in range(n)]
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                f[i][j] = s[i] == s[j] and (i + 1 == j or f[i + 1][j - 1])
        for i in range(n - 2):
            for j in range(i + 1, n - 1):
                if f[0][i] and f[i + 1][j] and f[j + 1][-1]:
                    return True
        return False
```

#### Java

```java
class Solution {
    public boolean checkPartitioning(String s) {
        int n = s.length();
        boolean[][] f = new boolean[n][n];
        for (var g : f) {
            Arrays.fill(g, true);
        }
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = s.charAt(i) == s.charAt(j) && (i + 1 == j || f[i + 1][j - 1]);
            }
        }
        for (int i = 0; i < n - 2; ++i) {
            for (int j = i + 1; j < n - 1; ++j) {
                if (f[0][i] && f[i + 1][j] && f[j + 1][n - 1]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkPartitioning(string s) {
        int n = s.size();
        vector<vector<bool>> f(n, vector<bool>(n, true));
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = s[i] == s[j] && (i + 1 == j || f[i + 1][j - 1]);
            }
        }
        for (int i = 0; i < n - 2; ++i) {
            for (int j = i + 1; j < n - 1; ++j) {
                if (f[0][i] && f[i + 1][j] && f[j + 1][n - 1]) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func checkPartitioning(s string) bool {
	n := len(s)
	f := make([][]bool, n)
	for i := range f {
		f[i] = make([]bool, n)
		for j := range f[i] {
			f[i][j] = true
		}
	}
	for i := n - 1; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			f[i][j] = s[i] == s[j] && (i+1 == j || f[i+1][j-1])
		}
	}
	for i := 0; i < n-2; i++ {
		for j := i + 1; j < n-1; j++ {
			if f[0][i] && f[i+1][j] && f[j+1][n-1] {
				return true
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function checkPartitioning(s: string): boolean {
    const n = s.length;
    const f: boolean[][] = Array.from({ length: n }, () => Array(n).fill(true));
    for (let i = n - 1; i >= 0; --i) {
        for (let j = i + 1; j < n; ++j) {
            f[i][j] = s[i] === s[j] && f[i + 1][j - 1];
        }
    }
    for (let i = 0; i < n - 2; ++i) {
        for (let j = i + 1; j < n - 1; ++j) {
            if (f[0][i] && f[i + 1][j] && f[j + 1][n - 1]) {
                return true;
            }
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
