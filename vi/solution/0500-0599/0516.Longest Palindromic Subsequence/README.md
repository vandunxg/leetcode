---
comments: true
difficulty: Medium
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence)

[中文文档](/solution/0500-0599/0516.Longest%20Palindromic%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy tìm <em>độ dài <strong>dãy con</strong> đối xứng dài nhất</em> trong <code>s</code>.</p>

<p><strong>Dãy con</strong> là một dãy có thể tạo ra từ dãy khác bằng cách xóa một số phần tử hoặc không xóa phần tử nào, đồng thời giữ nguyên thứ tự các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bbbab&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một dãy con đối xứng dài nhất có thể là &quot;bbbb&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cbbd&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Một dãy con đối xứng dài nhất có thể là &quot;bb&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con đối xứng không cần nằm liền nhau, nên mở rộng từ tâm là chưa đủ; liệt kê mọi dãy con thì có quá nhiều trường hợp.
>
> Gọi $f[i][j]$ là độ dài LPS của đoạn $s[i..j]$. Nếu hai đầu bằng nhau, cộng $2$ vào kết quả của đoạn bên trong; nếu không, bỏ một đầu. Điền bảng theo độ dài đoạn tăng dần (duyệt $i$ từ phải sang trái, $j$ từ trái sang phải) để các trạng thái phụ thuộc đã có sẵn. Đáp án là $f[0][n-1]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là độ dài dãy con đối xứng dài nhất trong chuỗi con từ ký tự thứ $i$ đến ký tự thứ $j$ của chuỗi $s$. Ban đầu, $f[i][i] = 1$, còn các vị trí khác đều có giá trị $0$.

Nếu $s[i] = s[j]$ thì $f[i][j] = f[i + 1][j - 1] + 2$; ngược lại, $f[i][j] = \max(f[i + 1][j], f[i][j - 1])$.

Vì giá trị $f[i][j]$ phụ thuộc vào $f[i + 1][j - 1]$, $f[i + 1][j]$ và $f[i][j - 1]$, ta cần duyệt $i$ từ lớn xuống nhỏ và duyệt $j$ từ nhỏ lên lớn.

Đáp án là $f[0][n - 1]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        f = [[0] * n for _ in range(n)]
        for i in range(n):
            f[i][i] = 1
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                if s[i] == s[j]:
                    f[i][j] = f[i + 1][j - 1] + 2
                else:
                    f[i][j] = max(f[i + 1][j], f[i][j - 1])
        return f[0][-1]
```

#### Java

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] f = new int[n][n];
        for (int i = 0; i < n; ++i) {
            f[i][i] = 1;
        }
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = f[i + 1][j - 1] + 2;
                } else {
                    f[i][j] = Math.max(f[i + 1][j], f[i][j - 1]);
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
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < n; ++i) {
            f[i][i] = 1;
        }
        for (int i = n - 1; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s[i] == s[j]) {
                    f[i][j] = f[i + 1][j - 1] + 2;
                } else {
                    f[i][j] = max(f[i + 1][j], f[i][j - 1]);
                }
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func longestPalindromeSubseq(s string) int {
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
			} else {
				f[i][j] = max(f[i+1][j], f[i][j-1])
			}
		}
	}
	return f[0][n-1]
}
```

#### TypeScript

```ts
function longestPalindromeSubseq(s: string): number {
    const n = s.length;
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    for (let i = 0; i < n; ++i) {
        f[i][i] = 1;
    }
    for (let i = n - 2; ~i; --i) {
        for (let j = i + 1; j < n; ++j) {
            if (s[i] === s[j]) {
                f[i][j] = f[i + 1][j - 1] + 2;
            } else {
                f[i][j] = Math.max(f[i + 1][j], f[i][j - 1]);
            }
        }
    }
    return f[0][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
