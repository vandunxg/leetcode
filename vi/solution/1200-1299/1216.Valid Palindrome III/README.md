---
comments: true
difficulty: Hard
rating: 1753
source: Biweekly Contest 10 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1216. Valid Palindrome III 🔒](https://leetcode.com/problems/valid-palindrome-iii)

[中文文档](/solution/1200-1299/1216.Valid%20Palindrome%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>, trả về <code>true</code> nếu <code>s</code> là <code>k</code><strong>-palindrome</strong>.</p>

<p>Một chuỗi là <code>k</code><strong>-palindrome</strong> nếu có thể biến nó thành palindrome bằng cách xóa nhiều nhất <code>k</code> ký tự.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdeca&quot;, k = 2
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Xóa các ký tự &#39;b&#39; và &#39;e&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abbababa&quot;, k = 1
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Thử mọi cách xóa $k$ ký tự là không khả thi khi $n \le 1000$. Chuỗi có thể trở thành palindrome sau khi xóa nhiều nhất $k$ ký tự khi và chỉ khi LPS của nó có độ dài ít nhất $n-k$.
>
> Dùng interval DP: $f[i][j]$ là độ dài LPS của $s[i..j]$. Nếu hai đầu bằng nhau, thu hẹp khoảng rồi cộng thêm $2$; nếu không, chọn kết quả tốt hơn khi bỏ một trong hai đầu. Có thể trả về ngay khi $f[i][j]+k \ge n$.
>
> Điền bảng theo độ dài khoảng tăng dần (tương ứng với $i$ giảm dần) để các khoảng ngắn hơn đã có giá trị khi tính chuyển trạng thái.

<!-- thinking:end -->

Bài toán yêu cầu xóa nhiều nhất $k$ ký tự để phần chuỗi còn lại là palindrome. Ta có thể chuyển bài toán này thành tìm longest palindromic subsequence (LPS).

Ta định nghĩa $f[i][j]$ là độ dài longest palindromic subsequence trong substring $s[i..j]$. Ban đầu, với mọi $i$, ta có $f[i][i] = 1$ vì một ký tự đơn lẻ luôn là palindrome.

Nếu $s[i] = s[j]$, ta có $f[i][j] = f[i+1][j-1] + 2$, vì có thể thêm cả $s[i]$ và $s[j]$ vào longest palindromic subsequence của $s[i+1..j-1]$.

Nếu $s[i] \neq s[j]$, ta có $f[i][j] = \max(f[i+1][j], f[i][j-1])$, vì cần bỏ $s[i]$ hoặc $s[j]$ để phần substring còn lại có palindrome dài nhất.

Cuối cùng, ta kiểm tra có giá trị nào thỏa $f[i][j] + k \geq n$ hay không, trong đó $n$ là độ dài chuỗi $s$. Nếu có, ta có thể xóa nhiều nhất $k$ ký tự để phần chuỗi còn lại là palindrome.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isValidPalindrome(self, s: str, k: int) -> bool:
        n = len(s)
        f = [[0] * n for _ in range(n)]
        for i in range(n):
            f[i][i] = 1
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                if s[i] == s[j]:
                    f[i][j] = f[i + 1][j - 1] + 2
                else:
                    f[i][j] = max(f[i + 1][j], f[i][j - 1])
                if f[i][j] + k >= n:
                    return True
        return False
```

#### Java

```java
class Solution {
    public boolean isValidPalindrome(String s, int k) {
        int n = s.length();
        int[][] f = new int[n][n];
        for (int i = 0; i < n; ++i) {
            f[i][i] = 1;
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = f[i + 1][j - 1] + 2;
                } else {
                    f[i][j] = Math.max(f[i + 1][j], f[i][j - 1]);
                }
                if (f[i][j] + k >= n) {
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
    bool isValidPalindrome(string s, int k) {
        int n = s.length();
        int f[n][n];
        memset(f, 0, sizeof f);
        for (int i = 0; i < n; ++i) {
            f[i][i] = 1;
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (s[i] == s[j]) {
                    f[i][j] = f[i + 1][j - 1] + 2;
                } else {
                    f[i][j] = max(f[i + 1][j], f[i][j - 1]);
                }
                if (f[i][j] + k >= n) {
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
func isValidPalindrome(s string, k int) bool {
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
			if f[i][j]+k >= n {
				return true
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function isValidPalindrome(s: string, k: number): boolean {
    const n = s.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: n }, () => 0));
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
            if (f[i][j] + k >= n) {
                return true;
            }
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_valid_palindrome(s: String, k: i32) -> bool {
        let s = s.as_bytes();
        let n = s.len();
        let mut f = vec![vec![0; n]; n];

        for i in 0..n {
            f[i][i] = 1;
        }

        for i in (0..n - 2).rev() {
            for j in i + 1..n {
                if s[i] == s[j] {
                    f[i][j] = f[i + 1][j - 1] + 2;
                } else {
                    f[i][j] = std::cmp::max(f[i + 1][j], f[i][j - 1]);
                }

                if f[i][j] + k >= (n as i32) {
                    return true;
                }
            }
        }

        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
