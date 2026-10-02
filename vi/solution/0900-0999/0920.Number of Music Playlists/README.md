---
comments: true
difficulty: Hard
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [920. Number of Music Playlists](https://leetcode.com/problems/number-of-music-playlists)

[中文文档](/solution/0900-0999/0920.Number%20of%20Music%20Playlists/README.md)

## Mô tả

<!-- description:start -->

<p>Trình phát nhạc của bạn có <code>n</code> bài hát khác nhau. Trong chuyến đi, bạn muốn nghe tổng cộng <code>goal</code> bài (không nhất thiết phải khác nhau). Để tránh nhàm chán, bạn sẽ tạo playlist thỏa mãn:</p>

<ul>
	<li>Mỗi bài hát được phát <strong>ít nhất một lần</strong>.</li>
	<li>Một bài hát chỉ được phát lại sau khi đã phát <code>k</code> bài hát khác.</li>
</ul>

<p>Cho <code>n</code>, <code>goal</code> và <code>k</code>, hãy trả về <em>số playlist có thể tạo</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, goal = 3, k = 1
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có 6 playlist khả thi: [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2] và [3, 2, 1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, goal = 3, k = 0
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có 6 playlist khả thi: [1, 1, 2], [1, 2, 1], [2, 1, 1], [2, 2, 1], [2, 1, 2] và [1, 2, 2].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, goal = 3, k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 playlist khả thi: [1, 2, 1] và [2, 1, 2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= k &lt; n &lt;= goal &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Playlist dài $goal$ phải sử dụng đủ đúng $n$ bài hát khác nhau, và một bài chỉ được phát lại sau khi có ít nhất $k$ bài hát khác được phát. Vì $goal,n\le 100$, có thể dùng DP. $f[i][j]$ là số playlist dài $i$ sử dụng $j$ bài khác nhau: có $n-j+1$ cách chọn bài mới, và có $j-k$ cách chọn bài đã nghe khi $j>k$. Đáp án là $f[goal][n]$.

<!-- thinking:end -->

Định nghĩa $f[i][j]$ là số playlist dài $i$ có đúng $j$ bài hát khác nhau. Ta có $f[0][0] = 1$ và đáp án là $f[goal][n]$.

Để tính $f[i][j]$, ta có thể chọn một bài chưa nghe; khi đó trạng thái trước là $f[i - 1][j - 1]$ và có $n - (j - 1) = n - j + 1$ lựa chọn. Vì vậy, $f[i][j] += f[i - 1][j - 1] \times (n - j + 1)$. Ta cũng có thể chọn một bài đã nghe; khi đó trạng thái trước là $f[i - 1][j]$ và có $j - k$ lựa chọn. Do đó, $f[i][j] += f[i - 1][j] \times (j - k)$, với $j \geq k$.

Suy ra công thức chuyển trạng thái:

$$
f[i][j] = \begin{cases}
1 & i = 0, j = 0 \\
f[i - 1][j - 1] \times (n - j + 1) + f[i - 1][j] \times (j - k) & i \geq 1, j \geq 1
\end{cases}
$$

Đáp án cuối cùng là $f[goal][n]$.

Độ phức tạp thời gian là $O(goal \times n)$ và độ phức tạp không gian là $O(goal \times n)$, trong đó $goal$ và $n$ là các tham số của đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numMusicPlaylists(self, n: int, goal: int, k: int) -> int:
        mod = 10**9 + 7
        f = [[0] * (n + 1) for _ in range(goal + 1)]
        f[0][0] = 1
        for i in range(1, goal + 1):
            for j in range(1, n + 1):
                f[i][j] = f[i - 1][j - 1] * (n - j + 1)
                if j > k:
                    f[i][j] += f[i - 1][j] * (j - k)
                f[i][j] %= mod
        return f[goal][n]
```

#### Java

```java
class Solution {
    public int numMusicPlaylists(int n, int goal, int k) {
        final int mod = (int) 1e9 + 7;
        long[][] f = new long[goal + 1][n + 1];
        f[0][0] = 1;
        for (int i = 1; i <= goal; ++i) {
            for (int j = 1; j <= n; ++j) {
                f[i][j] = f[i - 1][j - 1] * (n - j + 1);
                if (j > k) {
                    f[i][j] += f[i - 1][j] * (j - k);
                }
                f[i][j] %= mod;
            }
        }
        return (int) f[goal][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numMusicPlaylists(int n, int goal, int k) {
        const int mod = 1e9 + 7;
        long long f[goal + 1][n + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= goal; ++i) {
            for (int j = 1; j <= n; ++j) {
                f[i][j] = f[i - 1][j - 1] * (n - j + 1);
                if (j > k) {
                    f[i][j] += f[i - 1][j] * (j - k);
                }
                f[i][j] %= mod;
            }
        }
        return f[goal][n];
    }
};
```

#### Go

```go
func numMusicPlaylists(n int, goal int, k int) int {
	const mod = 1e9 + 7
	f := make([][]int, goal+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	f[0][0] = 1
	for i := 1; i <= goal; i++ {
		for j := 1; j <= n; j++ {
			f[i][j] = f[i-1][j-1] * (n - j + 1)
			if j > k {
				f[i][j] += f[i-1][j] * (j - k)
			}
			f[i][j] %= mod
		}
	}
	return f[goal][n]
}
```

#### TypeScript

```ts
function numMusicPlaylists(n: number, goal: number, k: number): number {
    const mod = 1e9 + 7;
    const f = new Array(goal + 1).fill(0).map(() => new Array(n + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= goal; ++i) {
        for (let j = 1; j <= n; ++j) {
            f[i][j] = f[i - 1][j - 1] * (n - j + 1);
            if (j > k) {
                f[i][j] += f[i - 1][j] * (j - k);
            }
            f[i][j] %= mod;
        }
    }
    return f[goal][n];
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn num_music_playlists(n: i32, goal: i32, k: i32) -> i32 {
        let mut dp: Vec<Vec<i64>> = vec![vec![0; n as usize + 1]; goal as usize + 1];

        // Initialize the dp vector
        dp[0][0] = 1;

        // Begin the dp process
        for i in 1..=goal as usize {
            for j in 1..=n as usize {
                // Choose the song that has not been chosen before
                // We have n - (j - 1) songs to choose
                dp[i][j] += dp[i - 1][j - 1] * ((n - ((j as i32) - 1)) as i64);

                // Choose the song that has been chosen before
                // We have j - k songs to choose if j > k
                if (j as i32) > k {
                    dp[i][j] += dp[i - 1][j] * (((j as i32) - k) as i64);
                }

                // Update dp[i][j]
                dp[i][j] %= ((1e9 as i32) + 7) as i64;
            }
        }

        dp[goal as usize][n as usize] as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming (tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Lớp $i$ chỉ phụ thuộc vào $f[i-1][j-1]$ và $f[i-1][j]$, nên có thể nén bảng thành một chiều, giảm bộ nhớ phụ xuống $O(n)$.

<!-- thinking:end -->

Ta nhận thấy $f[i][j]$ chỉ phụ thuộc vào $f[i - 1][j - 1]$ và $f[i - 1][j]$. Vì vậy, có thể dùng rolling array để tối ưu không gian, giảm độ phức tạp không gian xuống $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numMusicPlaylists(self, n: int, goal: int, k: int) -> int:
        mod = 10**9 + 7
        f = [0] * (goal + 1)
        f[0] = 1
        for i in range(1, goal + 1):
            g = [0] * (goal + 1)
            for j in range(1, n + 1):
                g[j] = f[j - 1] * (n - j + 1)
                if j > k:
                    g[j] += f[j] * (j - k)
                g[j] %= mod
            f = g
        return f[n]
```

#### Java

```java
class Solution {
    public int numMusicPlaylists(int n, int goal, int k) {
        final int mod = (int) 1e9 + 7;
        long[] f = new long[n + 1];
        f[0] = 1;
        for (int i = 1; i <= goal; ++i) {
            long[] g = new long[n + 1];
            for (int j = 1; j <= n; ++j) {
                g[j] = f[j - 1] * (n - j + 1);
                if (j > k) {
                    g[j] += f[j] * (j - k);
                }
                g[j] %= mod;
            }
            f = g;
        }
        return (int) f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numMusicPlaylists(int n, int goal, int k) {
        const int mod = 1e9 + 7;
        vector<long long> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= goal; ++i) {
            vector<long long> g(n + 1);
            for (int j = 1; j <= n; ++j) {
                g[j] = f[j - 1] * (n - j + 1);
                if (j > k) {
                    g[j] += f[j] * (j - k);
                }
                g[j] %= mod;
            }
            f = move(g);
        }
        return f[n];
    }
};
```

#### Go

```go
func numMusicPlaylists(n int, goal int, k int) int {
	const mod = 1e9 + 7
	f := make([]int, goal+1)
	f[0] = 1
	for i := 1; i <= goal; i++ {
		g := make([]int, goal+1)
		for j := 1; j <= n; j++ {
			g[j] = f[j-1] * (n - j + 1)
			if j > k {
				g[j] += f[j] * (j - k)
			}
			g[j] %= mod
		}
		f = g
	}
	return f[n]
}
```

#### TypeScript

```ts
function numMusicPlaylists(n: number, goal: number, k: number): number {
    const mod = 1e9 + 7;
    let f = new Array(goal + 1).fill(0);
    f[0] = 1;
    for (let i = 1; i <= goal; ++i) {
        const g = new Array(goal + 1).fill(0);
        for (let j = 1; j <= n; ++j) {
            g[j] = f[j - 1] * (n - j + 1);
            if (j > k) {
                g[j] += f[j] * (j - k);
            }
            g[j] %= mod;
        }
        f = g;
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
