---
comments: true
difficulty: Medium
tags:
    - String
    - Dynamic Programming
    - Longest Common Subsequence
---

<!-- problem:start -->

# [712. Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings)

[中文文档](/solution/0700-0799/0712.Minimum%20ASCII%20Delete%20Sum%20for%20Two%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s1</code> và <code>s2</code>, hãy trả về <em>tổng giá trị <strong>ASCII</strong> nhỏ nhất của các ký tự cần xóa để hai chuỗi bằng nhau</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;sea&quot;, s2 = &quot;eat&quot;
<strong>Đầu ra:</strong> 231
<strong>Giải thích:</strong> Xóa &quot;s&quot; khỏi &quot;sea&quot; sẽ cộng giá trị ASCII của &quot;s&quot; (115) vào tổng.
Xóa &quot;t&quot; khỏi &quot;eat&quot; sẽ cộng 116 vào tổng.
Sau cùng, hai chuỗi bằng nhau và 115 + 116 = 231 là tổng nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;delete&quot;, s2 = &quot;leet&quot;
<strong>Đầu ra:</strong> 403
<strong>Giải thích:</strong> Xóa &quot;dee&quot; khỏi &quot;delete&quot; để chuỗi còn lại là &quot;let&quot;,
sẽ cộng 100[d] + 101[e] + 101[e] vào tổng.
Xóa &quot;e&quot; khỏi &quot;leet&quot; sẽ cộng 101[e] vào tổng.
Sau cùng, hai chuỗi đều bằng &quot;let&quot; và đáp án là 100+101+101+101 = 403.
Nếu thay vào đó đưa cả hai chuỗi về &quot;lee&quot; hoặc &quot;eet&quot;, ta sẽ có tổng lần lượt là 433 hoặc 417, lớn hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length &lt;= 1000</code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Cần làm cho hai chuỗi bằng nhau bằng cách xóa ký tự, với chi phí là tổng giá trị ASCII của các ký tự bị xóa. Độ dài tối đa $1000$, nên đệ quy ngây thơ trên các cách căn chỉnh sẽ lặp lại nhiều phép tính.
>
> Đây là bài toán edit distance chỉ cho phép xóa: nếu hai ký tự cuối giống nhau thì không tốn chi phí; nếu khác nhau thì phải xóa ký tự cuối của một bên và cộng giá trị ASCII của nó. Với prefix rỗng, chi phí là tổng giá trị ASCII của toàn bộ chuỗi còn lại.
>
> Gọi $f[i][j]$ là chi phí nhỏ nhất cho hai prefix. Tính bảng theo công thức chuyển trạng thái trên; đáp án là $f[m][n]$. Độ phức tạp thời gian và không gian đều là $O(mn)$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là tổng giá trị ASCII nhỏ nhất của các ký tự cần xóa để $i$ ký tự đầu của $s_1$ bằng $j$ ký tự đầu của $s_2$. Đáp án là $f[m][n]$.

Nếu $s_1[i-1] = s_2[j-1]$, thì $f[i][j] = f[i-1][j-1]$. Nếu không, ta có thể xóa $s_1[i-1]$ hoặc $s_2[j-1]$ để tối thiểu hóa $f[i][j]$. Do đó, công thức chuyển trạng thái là:

$$
f[i][j]=
\begin{cases}
f[i-1][j-1], & s_1[i-1] = s_2[j-1] \\
min(f[i-1][j] + s_1[i-1], f[i][j-1] + s_2[j-1]), & s_1[i-1] \neq s_2[j-1]
\end{cases}
$$

Trạng thái ban đầu là $f[0][j] = f[0][j-1] + s_2[j-1]$, $f[i][0] = f[i-1][0] + s_1[i-1]$.

Cuối cùng, trả về $f[m][n]$.

Độ phức tạp thời gian và không gian đều là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của $s_1$ và $s_2$.

Các bài toán tương tự:

- [1143. Longest Common Subsequence](https://github.com/doocs/leetcode/blob/main/solution/1100-1199/1143.Longest%20Common%20Subsequence/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDeleteSum(self, s1: str, s2: str) -> int:
        m, n = len(s1), len(s2)
        f = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            f[i][0] = f[i - 1][0] + ord(s1[i - 1])
        for j in range(1, n + 1):
            f[0][j] = f[0][j - 1] + ord(s2[j - 1])
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if s1[i - 1] == s2[j - 1]:
                    f[i][j] = f[i - 1][j - 1]
                else:
                    f[i][j] = min(
                        f[i - 1][j] + ord(s1[i - 1]), f[i][j - 1] + ord(s2[j - 1])
                    )
        return f[m][n]
```

#### Java

```java
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        int[][] f = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            f[i][0] = f[i - 1][0] + s1.charAt(i - 1);
        }
        for (int j = 1; j <= n; ++j) {
            f[0][j] = f[0][j - 1] + s2.charAt(j - 1);
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    f[i][j] = f[i - 1][j - 1];
                } else {
                    f[i][j]
                        = Math.min(f[i - 1][j] + s1.charAt(i - 1), f[i][j - 1] + s2.charAt(j - 1));
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
    int minimumDeleteSum(string s1, string s2) {
        int m = s1.size(), n = s2.size();
        int f[m + 1][n + 1];
        memset(f, 0, sizeof f);
        for (int i = 1; i <= m; ++i) {
            f[i][0] = f[i - 1][0] + s1[i - 1];
        }
        for (int j = 1; j <= n; ++j) {
            f[0][j] = f[0][j - 1] + s2[j - 1];
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (s1[i - 1] == s2[j - 1]) {
                    f[i][j] = f[i - 1][j - 1];
                } else {
                    f[i][j] = min(f[i - 1][j] + s1[i - 1], f[i][j - 1] + s2[j - 1]);
                }
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func minimumDeleteSum(s1 string, s2 string) int {
	m, n := len(s1), len(s2)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	for i, c := range s1 {
		f[i+1][0] = f[i][0] + int(c)
	}
	for j, c := range s2 {
		f[0][j+1] = f[0][j] + int(c)
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if s1[i-1] == s2[j-1] {
				f[i][j] = f[i-1][j-1]
			} else {
				f[i][j] = min(f[i-1][j]+int(s1[i-1]), f[i][j-1]+int(s2[j-1]))
			}
		}
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function minimumDeleteSum(s1: string, s2: string): number {
    const m = s1.length;
    const n = s2.length;
    const f = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        f[i][0] = f[i - 1][0] + s1[i - 1].charCodeAt(0);
    }
    for (let j = 1; j <= n; ++j) {
        f[0][j] = f[0][j - 1] + s2[j - 1].charCodeAt(0);
    }
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (s1[i - 1] === s2[j - 1]) {
                f[i][j] = f[i - 1][j - 1];
            } else {
                f[i][j] = Math.min(
                    f[i - 1][j] + s1[i - 1].charCodeAt(0),
                    f[i][j - 1] + s2[j - 1].charCodeAt(0),
                );
            }
        }
    }
    return f[m][n];
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_delete_sum(s1: String, s2: String) -> i32 {
        let m: usize = s1.len();
        let n: usize = s2.len();
        let b1 = s1.as_bytes();
        let b2 = s2.as_bytes();

        let mut f: Vec<Vec<i32>> = vec![vec![0; n + 1]; m + 1];

        for i in 1..=m {
            f[i][0] = f[i - 1][0] + b1[i - 1] as i32;
        }
        for j in 1..=n {
            f[0][j] = f[0][j - 1] + b2[j - 1] as i32;
        }

        for i in 1..=m {
            for j in 1..=n {
                if b1[i - 1] == b2[j - 1] {
                    f[i][j] = f[i - 1][j - 1];
                } else {
                    f[i][j] = std::cmp::min(
                        f[i - 1][j] + b1[i - 1] as i32,
                        f[i][j - 1] + b2[j - 1] as i32,
                    );
                }
            }
        }

        f[m][n]
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s1
 * @param {string} s2
 * @return {number}
 */
var minimumDeleteSum = function (s1, s2) {
    const m = s1.length;
    const n = s2.length;
    const f = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        f[i][0] = f[i - 1][0] + s1[i - 1].charCodeAt(0);
    }
    for (let j = 1; j <= n; ++j) {
        f[0][j] = f[0][j - 1] + s2[j - 1].charCodeAt(0);
    }
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            if (s1[i - 1] === s2[j - 1]) {
                f[i][j] = f[i - 1][j - 1];
            } else {
                f[i][j] = Math.min(
                    f[i - 1][j] + s1[i - 1].charCodeAt(0),
                    f[i][j - 1] + s2[j - 1].charCodeAt(0),
                );
            }
        }
    }
    return f[m][n];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
