---
comments: true
difficulty: Medium
tags:
    - String
    - Binary Search
    - Dynamic Programming
    - Suffix Array
    - Suffix Tree
    - Hash Function
    - Rolling Hash
    - Suffix Automato
---

<!-- problem:start -->

# [1062. Longest Repeating Substring 🔒](https://leetcode.com/problems/longest-repeating-substring)

[中文文档](/solution/1000-1099/1062.Longest%20Repeating%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy trả về độ dài của chuỗi con lặp dài nhất. Nếu không có chuỗi con nào lặp lại, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Không có chuỗi con nào lặp lại.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abbaba&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Các chuỗi con lặp dài nhất là &quot;ab&quot; và &quot;ba&quot;; mỗi chuỗi xuất hiện hai lần.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabcaabdaab&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Chuỗi con lặp dài nhất là &quot;aab&quot;, xuất hiện <code>3</code> lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Có thể tìm chuỗi con lặp dài nhất bằng suffix array hoặc binary search kết hợp hashing; với $n\le 2000$, cũng có thể dùng quy hoạch động $O(n^2)$. Độ dài hậu tố chung kết thúc tại hai vị trí khác nhau $i>j$ tăng thêm một khi $s[i]=s[j]$.
>
> $f[i][j]$ lưu độ dài đó. Duyệt $i$ và các $j<i$, chuyển trạng thái khi hai ký tự bằng nhau, đồng thời cập nhật giá trị lớn nhất.
>
> Đáp án là giá trị lớn nhất của $f[i][j]$.

<!-- thinking:end -->

Định nghĩa $f[i][j]$ là độ dài lớn nhất của chuỗi con lặp kết thúc tại $s[i]$ và $s[j]$. Ban đầu, $f[i][j]=0$.

Duyệt $i$ trong đoạn $[1, n)$ và $j$ trong đoạn $[0, i)$. Nếu $s[i]=s[j]$, ta có:

$$
f[i][j]=
\begin{cases}
f[i-1][j-1]+1, & j>0 \\
1, & j=0
\end{cases}
$$

Đáp án là giá trị lớn nhất trong tất cả $f[i][j]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài chuỗi $s$.

Bài toán tương tự:

- [1044. Longest Duplicate Substring 🔒](https://github.com/doocs/leetcode/blob/main/solution/1000-1099/1044.Longest%20Duplicate%20Substring/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestRepeatingSubstring(self, s: str) -> int:
        n = len(s)
        f = [[0] * n for _ in range(n)]
        ans = 0
        for i in range(1, n):
            for j in range(i):
                if s[i] == s[j]:
                    f[i][j] = 1 + (f[i - 1][j - 1] if j else 0)
                    ans = max(ans, f[i][j])
        return ans
```

#### Java

```java
class Solution {
    public int longestRepeatingSubstring(String s) {
        int n = s.length();
        int[][] f = new int[n][n];
        int ans = 0;
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = 1 + (j > 0 ? f[i - 1][j - 1] : 0);
                    ans = Math.max(ans, f[i][j]);
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestRepeatingSubstring(string s) {
        int n = s.length();
        int f[n][n];
        memset(f, 0, sizeof(f));
        int ans = 0;
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (s[i] == s[j]) {
                    f[i][j] = 1 + (j > 0 ? f[i - 1][j - 1] : 0);
                    ans = max(ans, f[i][j]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestRepeatingSubstring(s string) (ans int) {
	n := len(s)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
	}
	for i := 1; i < n; i++ {
		for j := 0; j < i; j++ {
			if s[i] == s[j] {
				if j > 0 {
					f[i][j] = f[i-1][j-1]
				}
				f[i][j]++
				ans = max(ans, f[i][j])
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function longestRepeatingSubstring(s: string): number {
    const n = s.length;
    const f: number[][] = Array.from({ length: n }).map(() => Array(n).fill(0));
    let ans = 0;
    for (let i = 1; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (s[i] === s[j]) {
                f[i][j] = 1 + (f[i - 1][j - 1] || 0);
                ans = Math.max(ans, f[i][j]);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_repeating_substring(s: String) -> i32 {
        let n = s.len();
        let mut f = vec![vec![0; n]; n];
        let mut ans = 0;
        let s = s.as_bytes();

        for i in 1..n {
            for j in 0..i {
                if s[i] == s[j] {
                    f[i][j] = if j > 0 { f[i - 1][j - 1] + 1 } else { 1 };
                    ans = ans.max(f[i][j]);
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
