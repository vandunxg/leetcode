---
comments: true
difficulty: Hard
rating: 1979
source: Weekly Contest 165 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1278. Palindrome Partitioning III](https://leetcode.com/problems/palindrome-partitioning-iii)

[中文文档](/solution/1200-1299/1278.Palindrome%20Partitioning%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho chuỗi <code>s</code> chỉ gồm chữ cái thường và số nguyên <code>k</code>. Bạn cần thực hiện các bước sau:</p>

<ul>
	<li>Trước tiên, thay đổi một số ký tự trong <code>s</code> thành các chữ cái tiếng Anh thường khác.</li>
	<li>Sau đó, chia <code>s</code> thành <code>k</code> chuỗi con không rỗng, đôi một không giao nhau, sao cho mỗi chuỗi con đều là palindrome.</li>
</ul>

<p>Hãy trả về <em>số ký tự ít nhất cần thay đổi để chia chuỗi theo yêu cầu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abc&quot;, k = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>&nbsp;Bạn có thể chia chuỗi thành &quot;ab&quot; và &quot;c&quot;, rồi thay đổi 1 ký tự trong &quot;ab&quot; để chuỗi này trở thành palindrome.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabbc&quot;, k = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>&nbsp;Bạn có thể chia chuỗi thành &quot;aa&quot;, &quot;bb&quot; và &quot;c&quot;; tất cả đều là palindrome.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;, k = 8
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= s.length &lt;= 100</code>.</li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta chia chuỗi thành $k$ palindrome với số lần thay đổi ít nhất. Vì $n \le 100$, có thể dùng quy hoạch động. Ta tính trước chi phí biến một đoạn thành palindrome: cộng 1 nếu hai đầu khác nhau, cộng với chi phí của đoạn bên trong.
>
> $f[i][j]$ là số lần thay đổi ít nhất để chia $i$ ký tự đầu thành $j$ phần; ta thử vị trí kết thúc $h$ của phần trước rồi cộng thêm $g[h][i-1]$. Tính trước trong $O(n^2)$; các bước chuyển trạng thái mất $O(n^2 k)$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số lần thay đổi ít nhất cần thiết để chia $i$ ký tự đầu tiên của chuỗi $s$ thành $j$ chuỗi con palindrome. Giả sử chỉ số $i$ bắt đầu từ 1, đáp án là $f[n][k]$.

Để tính $f[i][j]$, ta có thể duyệt vị trí $h$ là ký tự cuối của chuỗi con palindrome thứ $(j-1)$. Khi đó, $f[i][j]$ bằng giá trị nhỏ nhất của $f[h][j-1] + g[h][i-1]$, trong đó $g[h][i-1]$ là số lần thay đổi ít nhất cần để biến chuỗi con $s[h..i-1]$ thành palindrome (có thể tiền xử lý phần này với độ phức tạp thời gian $O(n^2)$).

Độ phức tạp thời gian là $O(n^2 \times k)$ và độ phức tạp không gian là $O(n \times (n + k))$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def palindromePartition(self, s: str, k: int) -> int:
        n = len(s)
        g = [[0] * n for _ in range(n)]
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                g[i][j] = int(s[i] != s[j])
                if i + 1 < j:
                    g[i][j] += g[i + 1][j - 1]

        f = [[0] * (k + 1) for _ in range(n + 1)]
        for i in range(1, n + 1):
            for j in range(1, min(i, k) + 1):
                if j == 1:
                    f[i][j] = g[0][i - 1]
                else:
                    f[i][j] = inf
                    for h in range(j - 1, i):
                        f[i][j] = min(f[i][j], f[h][j - 1] + g[h][i - 1])
        return f[n][k]
```

#### Java

```java
class Solution {
    public int palindromePartition(String s, int k) {
        int n = s.length();
        int[][] g = new int[n][n];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i; j < n; ++j) {
                g[i][j] = s.charAt(i) != s.charAt(j) ? 1 : 0;
                if (i + 1 < j) {
                    g[i][j] += g[i + 1][j - 1];
                }
            }
        }
        int[][] f = new int[n + 1][k + 1];
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= Math.min(i, k); ++j) {
                if (j == 1) {
                    f[i][j] = g[0][i - 1];
                } else {
                    f[i][j] = 10000;
                    for (int h = j - 1; h < i; ++h) {
                        f[i][j] = Math.min(f[i][j], f[h][j - 1] + g[h][i - 1]);
                    }
                }
            }
        }
        return f[n][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int palindromePartition(string s, int k) {
        int n = s.size();
        vector<vector<int>> g(n, vector<int>(n));
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i; j < n; ++j) {
                g[i][j] = s[i] != s[j] ? 1 : 0;
                if (i + 1 < j) g[i][j] += g[i + 1][j - 1];
            }
        }
        vector<vector<int>> f(n + 1, vector<int>(k + 1));
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= min(i, k); ++j) {
                if (j == 1) {
                    f[i][j] = g[0][i - 1];
                } else {
                    f[i][j] = 10000;
                    for (int h = j - 1; h < i; ++h) {
                        f[i][j] = min(f[i][j], f[h][j - 1] + g[h][i - 1]);
                    }
                }
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func palindromePartition(s string, k int) int {
	n := len(s)
	g := make([][]int, n)
	for i := range g {
		g[i] = make([]int, n)
	}
	for i := n - 1; i >= 0; i-- {
		for j := 1; j < n; j++ {
			if s[i] != s[j] {
				g[i][j] = 1
			}
			if i+1 < j {
				g[i][j] += g[i+1][j-1]
			}
		}
	}
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
	}
	for i := 1; i <= n; i++ {
		for j := 1; j <= min(i, k); j++ {
			if j == 1 {
				f[i][j] = g[0][i-1]
			} else {
				f[i][j] = 100000
				for h := j - 1; h < i; h++ {
					f[i][j] = min(f[i][j], f[h][j-1]+g[h][i-1])
				}
			}
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function palindromePartition(s: string, k: number): number {
    const n = s.length;
    const g: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    for (let i = n - 1; i >= 0; i--) {
        for (let j = i + 1; j < n; j++) {
            g[i][j] = s[i] !== s[j] ? 1 : 0;
            if (i + 1 < j) {
                g[i][j] += g[i + 1][j - 1];
            }
        }
    }
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(k + 1).fill(0));
    for (let i = 1; i <= n; i++) {
        for (let j = 1; j <= Math.min(i, k); j++) {
            if (j === 1) {
                f[i][j] = g[0][i - 1];
            } else {
                f[i][j] = 1 << 30;
                for (let h = j - 1; h < i; h++) {
                    f[i][j] = Math.min(f[i][j], f[h][j - 1] + g[h][i - 1]);
                }
            }
        }
    }
    return f[n][k];
}
```

#### Rust

```rust
impl Solution {
    pub fn palindrome_partition(s: String, k: i32) -> i32 {
        let n = s.len();
        let s: Vec<char> = s.chars().collect();
        let mut g = vec![vec![0; n]; n];

        for i in (0..n).rev() {
            for j in i + 1..n {
                g[i][j] = if s[i] != s[j] { 1 } else { 0 };
                if i + 1 < j {
                    g[i][j] += g[i + 1][j - 1];
                }
            }
        }

        let mut f = vec![vec![0; (k + 1) as usize]; n + 1];
        let inf = i32::MAX;

        for i in 1..=n {
            for j in 1..=i.min(k as usize) {
                if j == 1 {
                    f[i][j] = g[0][i - 1];
                } else {
                    f[i][j] = inf;
                    for h in (j - 1)..i {
                        f[i][j] = f[i][j].min(f[h][j - 1] + g[h][i - 1]);
                    }
                }
            }
        }

        f[n][k as usize]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
