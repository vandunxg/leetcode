---
comments: true
difficulty: Medium
tags:
    - String
    - Dynamic Programming
    - Longest Common Subsequence
---

<!-- problem:start -->

# [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence)

[中文文档](/solution/1100-1199/1143.Longest%20Common%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>text1</code> và <code>text2</code>, hãy trả về <em>độ dài của <strong>dãy con chung</strong> dài nhất của chúng</em>. Nếu không có <strong>dãy con chung</strong>, trả về <code>0</code>.</p>

<p><strong>Dãy con</strong> của một chuỗi là chuỗi mới được tạo từ chuỗi ban đầu bằng cách xóa một số ký tự (có thể không xóa ký tự nào) mà không làm thay đổi thứ tự tương đối của các ký tự còn lại.</p>

<ul>
	<li>Ví dụ, <code>&quot;ace&quot;</code> là dãy con của <code>&quot;abcde&quot;</code>.</li>
</ul>

<p><strong>Dãy con chung</strong> của hai chuỗi là dãy con xuất hiện trong cả hai chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> text1 = &quot;abcde&quot;, text2 = &quot;ace&quot; 
<strong>Đầu ra:</strong> 3  
<strong>Giải thích:</strong> Dãy con chung dài nhất là &quot;ace&quot; và có độ dài bằng 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> text1 = &quot;abc&quot;, text2 = &quot;abc&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Dãy con chung dài nhất là &quot;abc&quot; và có độ dài bằng 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> text1 = &quot;abc&quot;, text2 = &quot;def&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có dãy con chung nào như vậy, nên kết quả là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text1.length, text2.length &lt;= 1000</code></li>
	<li><code>text1</code> và <code>text2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con chung không nhất thiết phải gồm các ký tự liên tiếp, nên việc liệt kê mọi dãy con sẽ rất tốn kém. $f[i][j]$ là LCS của hai tiền tố: nếu ký tự cuối trùng nhau thì cộng $1$ vào $f[i-1][j-1]$; nếu không, chọn kết quả tốt hơn khi bỏ một ký tự ở một trong hai chuỗi. Tính theo độ dài tiền tố chỉ cần các tiền tố ngắn hơn.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là độ dài dãy con chung dài nhất của $i$ ký tự đầu tiên trong $text1$ và $j$ ký tự đầu tiên trong $text2$. Vì vậy, đáp án là $f[m][n]$, trong đó $m$ và $n$ lần lượt là độ dài của $text1$ và $text2$.

Nếu ký tự thứ $i$ của $text1$ trùng với ký tự thứ $j$ của $text2$, thì $f[i][j] = f[i - 1][j - 1] + 1$; nếu hai ký tự khác nhau, thì $f[i][j] = max(f[i - 1][j], f[i][j - 1])$. Công thức chuyển trạng thái là:

$$
f[i][j] =
\begin{cases}
f[i - 1][j - 1] + 1, & \textit{if } text1[i - 1] = text2[j - 1] \\
\max(f[i - 1][j], f[i][j - 1]), & \textit{if } text1[i - 1] \neq text2[j - 1]
\end{cases}
$$

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của $text1$ và $text2$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if text1[i - 1] == text2[j - 1]:
                    f[i][j] = f[i - 1][j - 1] + 1
                else:
                    f[i][j] = max(f[i - 1][j], f[i][j - 1])
        return f[m][n]
```

#### Java

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] f = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                } else {
                    f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
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
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size(), n = text2.size();
        int f[m + 1][n + 1];
        memset(f, 0, sizeof f);
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (text1[i - 1] == text2[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1;
                } else {
                    f[i][j] = max(f[i - 1][j], f[i][j - 1]);
                }
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func longestCommonSubsequence(text1 string, text2 string) int {
	m, n := len(text1), len(text2)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if text1[i-1] == text2[j-1] {
				f[i][j] = f[i-1][j-1] + 1
			} else {
				f[i][j] = max(f[i-1][j], f[i][j-1])
			}
		}
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function longestCommonSubsequence(text1: string, text2: string): number {
    const m = text1.length;
    const n = text2.length;
    const f = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                f[i][j] = f[i - 1][j - 1] + 1;
            } else {
                f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
            }
        }
    }
    return f[m][n];
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_common_subsequence(text1: String, text2: String) -> i32 {
        let (m, n) = (text1.len(), text2.len());
        let (text1, text2) = (text1.as_bytes(), text2.as_bytes());
        let mut f = vec![vec![0; n + 1]; m + 1];
        for i in 1..=m {
            for j in 1..=n {
                f[i][j] = if text1[i - 1] == text2[j - 1] {
                    f[i - 1][j - 1] + 1
                } else {
                    f[i - 1][j].max(f[i][j - 1])
                };
            }
        }
        f[m][n]
    }
}
```

#### JavaScript

```js
/**
 * @param {string} text1
 * @param {string} text2
 * @return {number}
 */
var longestCommonSubsequence = function (text1, text2) {
    const m = text1.length;
    const n = text2.length;
    const f = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (text1[i - 1] == text2[j - 1]) {
                f[i][j] = f[i - 1][j - 1] + 1;
            } else {
                f[i][j] = Math.max(f[i - 1][j], f[i][j - 1]);
            }
        }
    }
    return f[m][n];
};
```

#### C#

```cs
public class Solution {
    public int LongestCommonSubsequence(string text1, string text2) {
        int m = text1.Length, n = text2.Length;
        int[,] f = new int[m + 1, n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (text1[i - 1] == text2[j - 1]) {
                    f[i, j] = f[i - 1, j - 1] + 1;
                } else {
                    f[i, j] = Math.Max(f[i - 1, j], f[i, j - 1]);
                }
            }
        }
        return f[m, n];
    }
}
```

#### Kotlin

```kotlin
class Solution {
    fun longestCommonSubsequence(text1: String, text2: String): Int {
        val m = text1.length
        val n = text2.length
        val f = Array(m + 1) { IntArray(n + 1) }
        for (i in 1..m) {
            for (j in 1..n) {
                if (text1[i - 1] == text2[j - 1]) {
                    f[i][j] = f[i - 1][j - 1] + 1
                } else {
                    f[i][j] = Math.max(f[i - 1][j], f[i][j - 1])
                }
            }
        }
        return f[m][n]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
