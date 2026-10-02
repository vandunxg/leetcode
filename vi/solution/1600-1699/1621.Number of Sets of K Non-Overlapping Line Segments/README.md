---
comments: true
difficulty: Medium
rating: 2198
source: Biweekly Contest 37 Q3
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
    - Prefix Sum
---

<!-- problem:start -->

# [1621. Number of Sets of K Non-Overlapping Line Segments](https://leetcode.com/problems/number-of-sets-of-k-non-overlapping-line-segments)

[中文文档](/solution/1600-1699/1621.Number%20of%20Sets%20of%20K%20Non-Overlapping%20Line%20Segments/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>n</code> điểm trên trục số, trong đó điểm thứ <code>i<sup>th</sup></code> (từ <code>0</code> đến <code>n-1</code>) nằm tại <code>x = i</code>. Hãy tìm số cách vẽ <strong>đúng</strong> <code>k</code> đoạn thẳng <strong>không chồng lấn</strong> sao cho mỗi đoạn bao phủ từ hai điểm trở lên. Hai đầu mút của mỗi đoạn phải có <strong>tọa độ nguyên</strong>. <code>k</code> đoạn thẳng <strong>không bắt buộc</strong> phải bao phủ cả <code>n</code> điểm và <strong>được phép</strong> dùng chung đầu mút.</p>

<p>Trả về <em>số cách vẽ </em><code>k</code><em> đoạn thẳng không chồng lấn</em><em>.</em> Vì số này có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1621.Number%20of%20Sets%20of%20K%20Non-Overlapping%20Line%20Segments/images/ex1.png" style="width: 179px; height: 222px;" />
<pre>
<strong>Input:</strong> n = 4, k = 2
<strong>Output:</strong> 5
<strong>Explanation:</strong> Hai đoạn thẳng được biểu diễn bằng màu đỏ và xanh.
The image above shows the 5 different ways {(0,2),(2,3)}, {(0,1),(1,3)}, {(0,1),(2,3)}, {(1,2),(2,3)}, {(0,1),(1,2)}.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 3, k = 1
<strong>Output:</strong> 3
<strong>Explanation:</strong> Có 3 cách là {(0,1)}, {(0,2)}, {(1,2)}.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 30, k = 7
<strong>Output:</strong> 796297179
<strong>Explanation:</strong> Tổng số cách có thể vẽ 7 đoạn thẳng là 3796297200. Lấy số này modulo 10<sup>9</sup> + 7 ta được 796297179.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= n-1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Cần chọn đúng $k$ đoạn không chồng lấn trên $n$ điểm, và các đoạn kề nhau có thể chung một đầu mút. Việc liệt kê hai đầu mút của từng đoạn sẽ quá lớn ngay cả khi $n,k\le 1000$, đồng thời vẫn phải duy trì thứ tự và tính rời nhau của các đoạn.
>
> Các đoạn nằm trên một đường thẳng, nên ta có thể xử lý các điểm từ trái sang phải và chia trạng thái theo việc điểm hiện tại có phải đầu mút phải của một đoạn hay không. Gọi $f[i][j]$ là số cách đặt $j$ đoạn trên $i$ điểm đầu tiên mà không kết thúc tại $i$, còn $g[i][j]$ là số cách kết thúc tại $i$.
>
> Khi đó, chuyển trạng thái chỉ cần hai loại trạng thái tại $i-1$: nếu không kết thúc tại i, ta kế thừa mọi cách đặt $j$ đoạn; nếu kết thúc tại $i$, ta hoặc kéo dài đoạn đã kết thúc tại $i-1$, hoặc bắt đầu một đoạn mới độ dài $1$ bao phủ $i-1$ và $i$.

<!-- thinking:end -->

Gọi $f[i][j]$ là số cách tạo $j$ đoạn bằng $i$ điểm đầu tiên sao cho đoạn cuối không kết thúc tại $i$, và $g[i][j]$ là số cách mà đoạn cuối kết thúc tại $i$. Ban đầu $f[1][0]=1$.

Với $f[i][j]$, đoạn thứ $j$ không kết thúc tại $i$, nên $i-1$ điểm đầu tiên đã chứa $j$ đoạn:

$$
f[i][j] = f[i-1][j] + g[i - 1][j]
$$

Với $g[i][j]$, đoạn thứ $j$ kết thúc tại $i$. Có hai nguồn được cộng lại: kéo dài đoạn thứ $j$ đã kết thúc tại $i-1$ (độ dài lớn hơn $1$), hoặc bắt đầu một đoạn mới bao phủ $i-1$ và $i$ sau khi đặt $j-1$ đoạn trên $i-1$ điểm đầu tiên (độ dài $1$). Khi $j=0$ không có đầu mút phải, nên bỏ qua nguồn thứ hai. Do đó với $j \ge 1$:

$$
g[i][j] = g[i - 1][j] + f[i - 1][j - 1] + g[i - 1][j - 1]
$$

Đáp án là $f[n][k]+g[n][k]$.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(n \times k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSets(self, n: int, k: int) -> int:
        mod = 10**9 + 7
        f = [[0] * (k + 1) for _ in range(n + 1)]
        g = [[0] * (k + 1) for _ in range(n + 1)]
        f[1][0] = 1
        for i in range(2, n + 1):
            for j in range(k + 1):
                f[i][j] = (f[i - 1][j] + g[i - 1][j]) % mod
                g[i][j] = g[i - 1][j]
                if j:
                    g[i][j] += f[i - 1][j - 1] + g[i - 1][j - 1]
                    g[i][j] %= mod
        return (f[n][k] + g[n][k]) % mod
```

#### Java

```java
class Solution {
    public int numberOfSets(int n, int k) {
        final int mod = (int) 1e9 + 7;
        int[][] f = new int[n + 1][k + 1];
        int[][] g = new int[n + 1][k + 1];
        f[1][0] = 1;
        for (int i = 2; i <= n; ++i) {
            for (int j = 0; j <= k; ++j) {
                f[i][j] = (f[i - 1][j] + g[i - 1][j]) % mod;
                g[i][j] = g[i - 1][j];
                if (j > 0) {
                    g[i][j] = (g[i][j] + f[i - 1][j - 1]) % mod;
                    g[i][j] = (g[i][j] + g[i - 1][j - 1]) % mod;
                }
            }
        }
        return (f[n][k] + g[n][k]) % mod;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSets(int n, int k) {
        const int mod = 1e9 + 7;
        vector<vector<int>> f(n + 1, vector<int>(k + 1));
        vector<vector<int>> g(n + 1, vector<int>(k + 1));
        f[1][0] = 1;
        for (int i = 2; i <= n; ++i) {
            for (int j = 0; j <= k; ++j) {
                f[i][j] = (f[i - 1][j] + g[i - 1][j]) % mod;
                g[i][j] = g[i - 1][j];
                if (j) {
                    g[i][j] = (g[i][j] + f[i - 1][j - 1]) % mod;
                    g[i][j] = (g[i][j] + g[i - 1][j - 1]) % mod;
                }
            }
        }
        return (f[n][k] + g[n][k]) % mod;
    }
};
```

#### Go

```go
func numberOfSets(n int, k int) int {
	const mod int = 1e9 + 7
	f := make([][]int, n+1)
	g := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
		g[i] = make([]int, k+1)
	}
	f[1][0] = 1
	for i := 2; i <= n; i++ {
		for j := 0; j <= k; j++ {
			f[i][j] = (f[i-1][j] + g[i-1][j]) % mod
			g[i][j] = g[i-1][j]
			if j > 0 {
				g[i][j] = (g[i][j] + f[i-1][j-1]) % mod
				g[i][j] = (g[i][j] + g[i-1][j-1]) % mod
			}
		}
	}
	return (f[n][k] + g[n][k]) % mod
}
```

#### TypeScript

```ts
function numberOfSets(n: number, k: number): number {
    const mod = 10 ** 9 + 7;
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(k + 1).fill(0));
    const g: number[][] = Array.from({ length: n + 1 }, () => Array(k + 1).fill(0));
    f[1][0] = 1;
    for (let i = 2; i <= n; ++i) {
        for (let j = 0; j <= k; ++j) {
            f[i][j] = (f[i - 1][j] + g[i - 1][j]) % mod;
            g[i][j] = g[i - 1][j];
            if (j) {
                g[i][j] = (g[i][j] + f[i - 1][j - 1]) % mod;
                g[i][j] = (g[i][j] + g[i - 1][j - 1]) % mod;
            }
        }
    }
    return (f[n][k] + g[n][k]) % mod;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_sets(n: i32, k: i32) -> i32 {
        const MOD: i64 = 1_000_000_007;
        let n = n as usize;
        let k = k as usize;
        let mut f = vec![vec![0i64; k + 1]; n + 1];
        let mut g = vec![vec![0i64; k + 1]; n + 1];
        f[1][0] = 1;
        for i in 2..=n {
            for j in 0..=k {
                f[i][j] = (f[i - 1][j] + g[i - 1][j]) % MOD;
                g[i][j] = g[i - 1][j];
                if j > 0 {
                    g[i][j] = (g[i][j] + f[i - 1][j - 1]) % MOD;
                    g[i][j] = (g[i][j] + g[i - 1][j - 1]) % MOD;
                }
            }
        }
        ((f[n][k] + g[n][k]) % MOD) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
