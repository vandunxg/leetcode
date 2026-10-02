---
comments: true
difficulty: Medium
tags:
    - String
    - Dynamic Programming
    - Longest Common Subsequence
---

<!-- problem:start -->

# [583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings)

[中文文档](/solution/0500-0599/0583.Delete%20Operation%20for%20Two%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>word1</code> và <code>word2</code>, hãy trả về <em>số <strong>bước</strong> ít nhất cần thực hiện để biến</em> <code>word1</code> <em>và</em> <code>word2</code> <em>thành hai chuỗi giống nhau</em>.</p>

<p>Mỗi <strong>bước</strong>, bạn có thể xóa chính xác một ký tự khỏi một trong hai chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;sea&quot;, word2 = &quot;eat&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Cần một bước để biến &quot;sea&quot; thành &quot;ea&quot; và thêm một bước để biến &quot;eat&quot; thành &quot;ea&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;leetcode&quot;, word2 = &quot;etco&quot;
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length, word2.length &lt;= 500</code></li>
	<li><code>word1</code> và <code>word2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ được phép xóa, nên số bước tối thiểu bằng $|s|+|t|-2\cdot\mathrm{LCS}$. Ta cũng có thể viết trực tiếp công thức truy hồi cho số lần xóa.
>
> $f[i][j]$ là số lần xóa ít nhất để hai tiền tố trở nên giống nhau. Nếu ký tự cuối trùng nhau, lấy giá trị $f[i-1][j-1]$; nếu không, xóa một ký tự ở một trong hai chuỗi rồi cộng thêm một. Các trường hợp biên bằng độ dài tiền tố còn lại. Đáp án là $f[m][n]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số lần xóa ít nhất để làm cho $i$ ký tự đầu tiên của chuỗi $\textit{word1}$ và $j$ ký tự đầu tiên của chuỗi $\textit{word2}$ giống nhau. Đáp án là $f[m][n]$, trong đó $m$ và $n$ lần lượt là độ dài của $\textit{word1}$ và $\textit{word2}$.

Ban đầu, nếu $j = 0$ thì $f[i][0] = i$; nếu $i = 0$ thì $f[0][j] = j$.

Khi $i, j > 0$, nếu $\textit{word1}[i - 1] = \textit{word2}[j - 1]$ thì $f[i][j] = f[i - 1][j - 1]$; ngược lại, $f[i][j] = \min(f[i - 1][j], f[i][j - 1]) + 1$.

Finally, return $f[m][n]$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là độ dài của chuỗi $\textit{word1}$ và $\textit{word2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            f[i][0] = i
        for j in range(1, n + 1):
            f[0][j] = j
        for i, a in enumerate(word1, 1):
            for j, b in enumerate(word2, 1):
                if a == b:
                    f[i][j] = f[i - 1][j - 1]
                else:
                    f[i][j] = min(f[i - 1][j], f[i][j - 1]) + 1
        return f[m][n]
```

#### Java

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] f = new int[m + 1][n + 1];
        for (int i = 0; i <= m; ++i) {
            f[i][0] = i;
        }
        for (int j = 0; j <= n; ++j) {
            f[0][j] = j;
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                char a = word1.charAt(i - 1);
                char b = word2.charAt(j - 1);
                if (a == b) {
                    f[i][j] = f[i - 1][j - 1];
                } else {
                    f[i][j] = Math.min(f[i - 1][j], f[i][j - 1]) + 1;
                }
            }
        }
        return f[m][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.length(), n = word2.length();
        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        for (int i = 0; i <= m; ++i) {
            f[i][0] = i;
        }
        for (int j = 0; j <= n; ++j) {
            f[0][j] = j;
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                char a = word1[i - 1];
                char b = word2[j - 1];
                if (a == b) {
                    f[i][j] = f[i - 1][j - 1];
                } else {
                    f[i][j] = min(f[i - 1][j], f[i][j - 1]) + 1;
                }
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func minDistance(word1 string, word2 string) int {
	m, n := len(word1), len(word2)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
        f[i][0] = i
	}
	for j := 1; j <= n; j++ {
		f[0][j] = j
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			a, b := word1[i-1], word2[j-1]
			if a == b {
				f[i][j] = f[i-1][j-1]
			} else {
				f[i][j] = 1 + min(f[i-1][j], f[i][j-1])
			}
		}
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function minDistance(word1: string, word2: string): number {
    const m = word1.length;
    const n = word2.length;
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        f[i][0] = i;
    }
    for (let j = 1; j <= n; ++j) {
        f[0][j] = j;
    }
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (word1[i - 1] === word2[j - 1]) {
                f[i][j] = f[i - 1][j - 1];
            } else {
                f[i][j] = Math.min(f[i - 1][j], f[i][j - 1]) + 1;
            }
        }
    }
    return f[m][n];
}
```

#### Rust

```rust
impl Solution {
    pub fn min_distance(word1: String, word2: String) -> i32 {
        let m = word1.len();
        let n = word2.len();
        let s: Vec<char> = word1.chars().collect();
        let t: Vec<char> = word2.chars().collect();
        let mut f = vec![vec![0; n + 1]; m + 1];

        for i in 0..=m {
            f[i][0] = i as i32;
        }
        for j in 0..=n {
            f[0][j] = j as i32;
        }

        for i in 1..=m {
            for j in 1..=n {
                let a = s[i - 1];
                let b = t[j - 1];
                if a == b {
                    f[i][j] = f[i - 1][j - 1];
                } else {
                    f[i][j] = std::cmp::min(f[i - 1][j], f[i][j - 1]) + 1;
                }
            }
        }
        f[m][n]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
