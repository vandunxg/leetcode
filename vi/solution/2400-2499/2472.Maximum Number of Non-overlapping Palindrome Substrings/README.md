---
comments: true
difficulty: Hard
rating: 2013
source: Weekly Contest 319 Q4
tags:
    - Greedy
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2472. Maximum Number of Non-overlapping Palindrome Substrings](https://leetcode.com/problems/maximum-number-of-non-overlapping-palindrome-substrings)

[中文文档](/solution/2400-2499/2472.Maximum%20Number%20of%20Non-overlapping%20Palindrome%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p>Hãy chọn một tập các chuỗi con <strong>không giao nhau</strong> từ chuỗi <code>s</code> thỏa mãn các điều kiện sau:</p>

<ul>
	<li><strong>Độ dài</strong> của mỗi chuỗi con <strong>ít nhất</strong> là <code>k</code>.</li>
	<li>Mỗi chuỗi con là một <strong>palindrome</strong>.</li>
</ul>

<p>Trả về <em><strong>số lượng chuỗi con lớn nhất</strong> trong một lựa chọn tối ưu</em>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp nằm trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abaccdbbd&quot;, k = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể chọn các chuỗi con được gạch chân trong s = &quot;<u><strong>aba</strong></u>cc<u><strong>dbbd</strong></u>&quot;. Cả &quot;aba&quot; và &quot;dbbd&quot; đều là palindrome và có độ dài ít nhất k = 3.
Có thể chứng minh rằng không thể tìm được một lựa chọn có nhiều hơn hai chuỗi con hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;adbcda&quot;, k = 2
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Trong chuỗi không có chuỗi con palindrome nào có độ dài ít nhất 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta muốn chọn được nhiều chuỗi con palindrome không giao nhau có độ dài ít nhất $k$ nhất có thể. Với $n \le 2000$, việc liệt kê các phân hoạch sẽ quá chậm. Có thể tiền xử lý xem $s[i..j]$ có phải là palindrome hay không trong $O(n^2)$ bằng cách dùng $g[i][j]$. Ở vị trí $i$, lựa chọn còn lại là bỏ qua $s[i]$, hoặc chọn một palindrome bắt đầu tại $i$ có độ dài ít nhất $k$ rồi tiếp tục sau đầu phải của nó. Tính $f[i]$ từ phải sang trái giúp mỗi chuyển trạng thái chỉ cần xét các chỉ số lớn hơn.

<!-- thinking:end -->

Đầu tiên, tiền xử lý chuỗi $s$ để thu được $g[i][j]$, biểu thị liệu chuỗi con $s[i..j]$ có phải là palindrome hay không.

Sau đó, định nghĩa $f[i]$ là số lượng lớn nhất các chuỗi con palindrome không giao nhau có thể chọn từ $s[i..]$. Ban đầu, $f[n] = 0$. Với $i$ từ $n - 1$ giảm dần đến $0$, ta có thể bỏ qua $s[i]$, tức là $f[i] = f[i + 1]$; ta cũng có thể duyệt chỉ số kết thúc $j$ ($j \ge i + k - 1$), và nếu $g[i][j]$ là true, ta chọn palindrome này rồi tiếp tục từ $j + 1$. Cụ thể:

$$
\begin{aligned}
f[i] &= \begin{cases}
0, & i \geq n \\
\max\bigl\{f[i + 1],\ \max\limits_{\substack{j \ge i + k - 1 \\ g[i][j]}} \{f[j + 1] + 1\}\bigr\}, & i < n
\end{cases}
\end{aligned}
$$

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPalindromes(self, s: str, k: int) -> int:
        n = len(s)
        g = [[True] * n for _ in range(n)]
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                g[i][j] = s[i] == s[j] and g[i + 1][j - 1]
        f = [0] * (n + 1)
        for i in range(n - 1, -1, -1):
            f[i] = f[i + 1]
            for j in range(i + k - 1, n):
                if g[i][j]:
                    f[i] = max(f[i], 1 + f[j + 1])
        return f[0]
```

#### Java

```java
class Solution {
    public int maxPalindromes(String s, int k) {
        int n = s.length();
        boolean[][] g = new boolean[n][n];
        for (var row : g) {
            Arrays.fill(row, true);
        }
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                g[i][j] = s.charAt(i) == s.charAt(j) && g[i + 1][j - 1];
            }
        }
        int[] f = new int[n + 1];
        for (int i = n - 1; i >= 0; --i) {
            f[i] = f[i + 1];
            for (int j = i + k - 1; j < n; ++j) {
                if (g[i][j]) {
                    f[i] = Math.max(f[i], 1 + f[j + 1]);
                }
            }
        }
        return f[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxPalindromes(string s, int k) {
        int n = s.size();
        bool g[n][n];
        memset(g, true, sizeof(g));
        for (int i = n - 1; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                g[i][j] = s[i] == s[j] && g[i + 1][j - 1];
            }
        }
        int f[n + 1];
        memset(f, 0, sizeof(f));
        for (int i = n - 1; ~i; --i) {
            f[i] = f[i + 1];
            for (int j = i + k - 1; j < n; ++j) {
                if (g[i][j]) {
                    f[i] = max(f[i], 1 + f[j + 1]);
                }
            }
        }
        return f[0];
    }
};
```

#### Go

```go
func maxPalindromes(s string, k int) int {
	n := len(s)
	g := make([][]bool, n)
	for i := range g {
		g[i] = make([]bool, n)
		for j := range g[i] {
			g[i][j] = true
		}
	}
	for i := n - 1; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			g[i][j] = s[i] == s[j] && g[i+1][j-1]
		}
	}
	f := make([]int, n+1)
	for i := n - 1; i >= 0; i-- {
		f[i] = f[i+1]
		for j := i + k - 1; j < n; j++ {
			if g[i][j] {
				f[i] = max(f[i], 1+f[j+1])
			}
		}
	}
	return f[0]
}
```

#### TypeScript

```ts
function maxPalindromes(s: string, k: number): number {
    const n = s.length;
    const g: boolean[][] = Array.from({ length: n }, () => Array(n).fill(true));
    for (let i = n - 1; ~i; --i) {
        for (let j = i + 1; j < n; ++j) {
            g[i][j] = s[i] === s[j] && g[i + 1][j - 1];
        }
    }
    const f: number[] = Array(n + 1).fill(0);
    for (let i = n - 1; ~i; --i) {
        f[i] = f[i + 1];
        for (let j = i + k - 1; j < n; ++j) {
            if (g[i][j]) {
                f[i] = Math.max(f[i], 1 + f[j + 1]);
            }
        }
    }
    return f[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
