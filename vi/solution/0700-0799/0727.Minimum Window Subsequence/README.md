---
comments: true
difficulty: Hard
tags:
    - String
    - Dynamic Programming
    - Sliding Window
---

<!-- problem:start -->

# [727. Minimum Window Subsequence 🔒](https://leetcode.com/problems/minimum-window-subsequence)

[中文文档](/solution/0700-0799/0727.Minimum%20Window%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s1</code> và <code>s2</code>, hãy trả về <em>chuỗi con liên tiếp ngắn nhất của </em><code>s1</code><em> sao cho </em><code>s2</code><em> là một dãy con của chuỗi con đó</em>.</p>

<p>Nếu không có đoạn con nào trong <code>s1</code> chứa đủ các ký tự của <code>s2</code>, hãy trả về chuỗi rỗng <code>&quot;&quot;</code>. Nếu có nhiều đoạn ngắn nhất như vậy, hãy trả về đoạn có <strong>chỉ số bắt đầu nhỏ nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;abcdebdde&quot;, s2 = &quot;bde&quot;
<strong>Đầu ra:</strong> &quot;bcde&quot;
<strong>Giải thích:</strong>
&quot;bcde&quot; là đáp án vì nó xuất hiện trước &quot;bdde&quot;, đoạn có cùng độ dài.
&quot;deb&quot; không phải đoạn con ngắn hơn vì các ký tự của s2 trong đoạn phải xuất hiện đúng thứ tự.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;jmeqksfrsdcmsiwvaovztaqenprpvnbstl&quot;, s2 = &quot;u&quot;
<strong>Đầu ra:</strong> &quot;&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= s2.length &lt;= 100</code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tìm đoạn ngắn nhất trong $s_1$ có chứa $s_2$ như một dãy con. Vì $|s_1|\le 2\times 10^4$ và $|s_2|\le 100$, thử mọi đoạn con sẽ không hiệu quả.
>
> Khi ký tự cuối của $s_2$ được khớp, độ dài đoạn con được xác định bởi vị trí bắt đầu của lần khớp đó. Lưu chỉ số bắt đầu trong DP giúp khôi phục mọi ứng viên.
>
> $f[i][j]$ lưu vị trí bắt đầu đó: khi ký tự khớp, kế thừa $f[i-1][j-1]$ (hoặc là $i$ nếu $j=1$); nếu không thì lấy $f[i-1][j]$. Mỗi khi $f[i][n]>0$, ta cập nhật đoạn ngắn nhất.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là vị trí bắt đầu của chuỗi con ngắn nhất trong $i$ ký tự đầu của chuỗi $\textit{s1}$ có chứa $j$ ký tự đầu của chuỗi $\textit{s2}$. Nếu không tồn tại chuỗi con như vậy thì giá trị là $0$.

Ta có thể suy ra công thức chuyển trạng thái:

$$
f[i][j] = \begin{cases}
i, & j = 1 \textit{ and } s1[i-1] = s2[j] \\
f[i - 1][j - 1], & j > 1 \textit{ and } s1[i-1] = s2[j-1] \\
f[i - 1][j], & s1[i-1] \ne s2[j-1]
\end{cases}
$$

Tiếp theo, ta chỉ cần duyệt $\textit{s1}$. Nếu $f[i][n] \gt 0$, hãy cập nhật vị trí bắt đầu và độ dài của chuỗi con ngắn nhất. Cuối cùng, trả về chuỗi con đó.

Độ phức tạp thời gian và không gian đều là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của chuỗi $\textit{s1}$ và $\textit{s2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minWindow(self, s1: str, s2: str) -> str:
        m, n = len(s1), len(s2)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for i, a in enumerate(s1, 1):
            for j, b in enumerate(s2, 1):
                if a == b:
                    f[i][j] = i if j == 1 else f[i - 1][j - 1]
                else:
                    f[i][j] = f[i - 1][j]
        p, k = 0, m + 1
        for i, a in enumerate(s1, 1):
            if a == s2[n - 1] and f[i][n]:
                j = f[i][n] - 1
                if i - j < k:
                    k = i - j
                    p = j
        return "" if k > m else s1[p : p + k]
```

#### Java

```java
class Solution {
    public String minWindow(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        int[][] f = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    f[i][j] = j == 1 ? i : f[i - 1][j - 1];
                } else {
                    f[i][j] = f[i - 1][j];
                }
            }
        }
        int p = 0, k = m + 1;
        for (int i = 1; i <= m; ++i) {
            if (s1.charAt(i - 1) == s2.charAt(n - 1) && f[i][n] > 0) {
                int j = f[i][n] - 1;
                if (i - j < k) {
                    k = i - j;
                    p = j;
                }
            }
        }
        return k > m ? "" : s1.substring(p, p + k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string minWindow(string s1, string s2) {
        int m = s1.size(), n = s2.size();
        int f[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (s1[i - 1] == s2[j - 1]) {
                    f[i][j] = j == 1 ? i : f[i - 1][j - 1];
                } else {
                    f[i][j] = f[i - 1][j];
                }
            }
        }
        int p = 0, k = m + 1;
        for (int i = 1; i <= m; ++i) {
            if (s1[i - 1] == s2[n - 1] && f[i][n]) {
                int j = f[i][n] - 1;
                if (i - j < k) {
                    k = i - j;
                    p = j;
                }
            }
        }
        return k > m ? "" : s1.substr(p, k);
    }
};
```

#### Go

```go
func minWindow(s1 string, s2 string) string {
	m, n := len(s1), len(s2)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if s1[i-1] == s2[j-1] {
				if j == 1 {
					f[i][j] = i
				} else {
					f[i][j] = f[i-1][j-1]
				}
			} else {
				f[i][j] = f[i-1][j]
			}
		}
	}
	p, k := 0, m+1
	for i := 1; i <= m; i++ {
		if s1[i-1] == s2[n-1] && f[i][n] > 0 {
			j := f[i][n] - 1
			if i-j < k {
				k = i - j
				p = j
			}
		}
	}
	if k > m {
		return ""
	}
	return s1[p : p+k]
}
```

#### TypeScript

```ts
function minWindow(s1: string, s2: string): string {
    const m = s1.length;
    const n = s2.length;
    const f: number[][] = Array(m + 1)
        .fill(0)
        .map(() => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (s1[i - 1] === s2[j - 1]) {
                f[i][j] = j === 1 ? i : f[i - 1][j - 1];
            } else {
                f[i][j] = f[i - 1][j];
            }
        }
    }
    let p = 0;
    let k = m + 1;
    for (let i = 1; i <= m; ++i) {
        if (s1[i - 1] === s2[n - 1] && f[i][n]) {
            const j = f[i][n] - 1;
            if (i - j < k) {
                k = i - j;
                p = j;
            }
        }
    }
    return k > m ? '' : s1.slice(p, p + k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
