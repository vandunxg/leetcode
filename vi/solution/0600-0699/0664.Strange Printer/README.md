---
comments: true
difficulty: Hard
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [664. Strange Printer](https://leetcode.com/problems/strange-printer)

[中文文档](/solution/0600-0699/0664.Strange%20Printer/README.md)

## Mô tả

<!-- description:start -->

<p>Có một máy in đặc biệt với hai tính chất sau:</p>

<ul>
	<li>Mỗi lần in, máy chỉ có thể in một dãy gồm <strong>cùng một ký tự</strong>.</li>
	<li>Ở mỗi lượt, máy có thể bắt đầu và kết thúc việc in ký tự mới tại bất kỳ vị trí nào; phần in mới sẽ che các ký tự đã có.</li>
</ul>

<p>Cho chuỗi <code>s</code>, hãy trả về <em>số lượt in ít nhất cần để máy in chuỗi đó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaabbb&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trước tiên in &quot;aaa&quot;, sau đó in &quot;bbb&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aba&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trước tiên in &quot;aaa&quot;, sau đó in &quot;b&quot; từ vị trí thứ hai của chuỗi để che ký tự &#39;a&#39; hiện có.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt in tô một đoạn liên tiếp bằng cùng một chữ cái. $n\le 100$ phù hợp với interval DP.
>
> $f[i][j]$ là số lượt in ít nhất cho $s[i..j]$. Nếu hai đầu mút giống nhau, có thể dùng chung lượt in cuối; nếu không, thử mọi điểm chia. Tính các đoạn ngắn trước.

<!-- thinking:end -->

Định nghĩa $f[i][j]$ là số lượt in ít nhất để in $s[i..j]$, với giá trị khởi tạo $f[i][j]=\infty$. Đáp án là $f[0][n-1]$, trong đó $n$ là độ dài chuỗi $s$.

Xét $f[i][j]$. Nếu $s[i] = s[j]$, ta có thể in $s[j]$ cùng lượt với $s[i]$, nên có thể bỏ qua $s[j]$ và tiếp tục in $s[i+1..j-1]$. Nếu $s[i] \neq s[j]$, cần in riêng hai chuỗi con $s[i..k]$ và $s[k+1..j]$, với $k \in [i,j)$. Khi đó, ta có công thức chuyển trạng thái:

$$
f[i][j]=
\begin{cases}
1, & \textit{if } i=j \\
f[i][j-1], & \textit{if } s[i]=s[j] \\
\min_{i \leq k < j} \{f[i][k]+f[k+1][j]\}, & \textit{otherwise}
\end{cases}
$$

Ta có thể duyệt $i$ từ lớn xuống nhỏ và $j$ từ nhỏ lên lớn để bảo đảm $f[i][j-1]$, $f[i][k]$ và $f[k+1][j]$ đã được tính khi tính $f[i][j]$.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def strangePrinter(self, s: str) -> int:
        n = len(s)
        f = [[inf] * n for _ in range(n)]
        for i in range(n - 1, -1, -1):
            f[i][i] = 1
            for j in range(i + 1, n):
                if s[i] == s[j]:
                    f[i][j] = f[i][j - 1]
                else:
                    for k in range(i, j):
                        f[i][j] = min(f[i][j], f[i][k] + f[k + 1][j])
        return f[0][-1]
```

#### Java

```java
class Solution {
    public int strangePrinter(String s) {
        final int inf = 1 << 30;
        int n = s.length();
        int[][] f = new int[n][n];
        for (var g : f) {
            Arrays.fill(g, inf);
        }
        for (int i = n - 1; i >= 0; --i) {
            f[i][i] = 1;
            for (int j = i + 1; j < n; ++j) {
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = f[i][j - 1];
                } else {
                    for (int k = i; k < j; ++k) {
                        f[i][j] = Math.min(f[i][j], f[i][k] + f[k + 1][j]);
                    }
                }
            }
        }
        return f[0][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int strangePrinter(string s) {
        int n = s.size();
        int f[n][n];
        memset(f, 0x3f, sizeof(f));
        for (int i = n - 1; ~i; --i) {
            f[i][i] = 1;
            for (int j = i + 1; j < n; ++j) {
                if (s[i] == s[j]) {
                    f[i][j] = f[i][j - 1];
                } else {
                    for (int k = i; k < j; ++k) {
                        f[i][j] = min(f[i][j], f[i][k] + f[k + 1][j]);
                    }
                }
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func strangePrinter(s string) int {
	n := len(s)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = 1 << 30
		}
	}
	for i := n - 1; i >= 0; i-- {
		f[i][i] = 1
		for j := i + 1; j < n; j++ {
			if s[i] == s[j] {
				f[i][j] = f[i][j-1]
			} else {
				for k := i; k < j; k++ {
					f[i][j] = min(f[i][j], f[i][k]+f[k+1][j])
				}
			}
		}
	}
	return f[0][n-1]
}
```

#### TypeScript

```ts
function strangePrinter(s: string): number {
    const n = s.length;
    const f: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(1 << 30));
    for (let i = n - 1; i >= 0; --i) {
        f[i][i] = 1;
        for (let j = i + 1; j < n; ++j) {
            if (s[i] === s[j]) {
                f[i][j] = f[i][j - 1];
            } else {
                for (let k = i; k < j; ++k) {
                    f[i][j] = Math.min(f[i][j], f[i][k] + f[k + 1][j]);
                }
            }
        }
    }
    return f[0][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
