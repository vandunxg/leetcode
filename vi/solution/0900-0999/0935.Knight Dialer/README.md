---
comments: true
difficulty: Medium
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [935. Knight Dialer](https://leetcode.com/problems/knight-dialer)

[中文文档](/solution/0900-0999/0935.Knight%20Dialer/README.md)

## Mô tả

<!-- description:start -->

<p>Quân mã trong cờ vua có <strong>cách di chuyển đặc biệt</strong>: đi hai ô theo chiều dọc rồi một ô theo chiều ngang, hoặc hai ô theo chiều ngang rồi một ô theo chiều dọc (cả hai cách đều tạo thành hình chữ <strong>L</strong>). Hình minh họa các nước đi có thể của quân mã:</p>

<p>Quân mã có thể di chuyển như minh họa trên bàn cờ dưới đây:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0935.Knight%20Dialer/images/chess.jpg" style="width: 402px; height: 402px;" />
<p>Cho một quân mã và bàn phím số như hình dưới. Quân mã <strong>chỉ có thể đứng trên ô có chữ số</strong> (tức ô màu xanh).</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0935.Knight%20Dialer/images/phone.jpg" style="width: 242px; height: 322px;" />
<p>Cho số nguyên <code>n</code>, hãy trả về số lượng số điện thoại khác nhau có độ dài <code>n</code> mà ta có thể bấm.</p>

<p>Ban đầu, bạn có thể đặt quân mã lên <strong>bất kỳ ô số nào</strong>, sau đó thực hiện <code>n - 1</code> lần nhảy để bấm số điện thoại dài <code>n</code>. Mọi lần nhảy đều phải là nước đi hợp lệ của quân mã.</p>

<p>Vì đáp án có thể rất lớn, <strong>hãy trả về đáp án modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Ta cần bấm một số dài 1, nên chỉ cần đặt quân mã lên bất kỳ ô số nào trong 10 ô.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Các số hợp lệ có thể bấm là [04, 06, 16, 18, 27, 29, 34, 38, 40, 43, 49, 60, 61, 67, 72, 76, 81, 83, 92, 94]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3131
<strong>Đầu ra:</strong> 136006598
<strong>Giải thích:</strong> Lưu ý phép modulo.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Công thức truy hồi

<!-- thinking:start -->

> **Tư duy**
>
> Quân mã nhảy $n-1$ lần trên bàn phím, với $n\le 5000$. Mỗi chữ số có một tập chữ số đi trước cố định, nên số chuỗi dài $i$ kết thúc bằng chữ số $d$ chỉ phụ thuộc vào bước nhảy trước đó. Dùng mảng độ rộng $10$ cập nhật luân phiên qua $n-1$ bước rồi tính tổng modulo $10^9+7$.

<!-- thinking:end -->

Theo đề bài, ta cần tính số lượng số điện thoại khác nhau có độ dài $n$. Mỗi chữ số chỉ có thể đi sau một số chữ số cố định, được liệt kê như sau:

| Chữ số hiện tại | Các chữ số đứng trước |
| ------------- | --------------- |
| 0             | 4, 6            |
| 1             | 6, 8            |
| 2             | 7, 9            |
| 3             | 4, 8            |
| 4             | 0, 3, 9         |
| 5             |                 |
| 6             | 0, 1, 7         |
| 7             | 2, 6            |
| 8             | 1, 3            |
| 9             | 2, 4            |

Ta có thể dùng công thức truy hồi để tính số lượng số điện thoại khác nhau có độ dài $n$. Gọi $f[i]$ là số lượng số điện thoại khác nhau có độ dài $i$. Ban đầu, $f[1] = 1$. Số điện thoại dài $i$ được tính dựa trên các số điện thoại dài $i - 1$, từ đó ta có các công thức truy hồi:

$$
\begin{aligned}
g[0] & = f[4] + f[6] \\
g[1] & = f[6] + f[8] \\
g[2] & = f[7] + f[9] \\
g[3] & = f[4] + f[8] \\
g[4] & = f[0] + f[3] + f[9] \\
g[6] & = f[0] + f[1] + f[7] \\
g[7] & = f[2] + f[6] \\
g[8] & = f[1] + f[3] \\
g[9] & = f[2] + f[4]
\end{aligned}
$$

Sau đó, cập nhật $f$ thành $g$ và tiếp tục tính số điện thoại có độ dài kế tiếp cho đến khi đạt độ dài $n$.

Cuối cùng, tính tổng các phần tử trong $f$ rồi lấy modulo $10^9 + 7$ để nhận đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài số điện thoại. Độ phức tạp không gian là $O(|\Sigma|)$, với $\Sigma$ là tập chữ số; trong bài này, $|\Sigma| = 10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def knightDialer(self, n: int) -> int:
        f = [1] * 10
        for _ in range(n - 1):
            g = [0] * 10
            g[0] = f[4] + f[6]
            g[1] = f[6] + f[8]
            g[2] = f[7] + f[9]
            g[3] = f[4] + f[8]
            g[4] = f[0] + f[3] + f[9]
            g[6] = f[0] + f[1] + f[7]
            g[7] = f[2] + f[6]
            g[8] = f[1] + f[3]
            g[9] = f[2] + f[4]
            f = g
        return sum(f) % (10**9 + 7)
```

#### Java

```java
class Solution {
    public int knightDialer(int n) {
        final int mod = (int) 1e9 + 7;
        long[] f = new long[10];
        Arrays.fill(f, 1);
        while (--n > 0) {
            long[] g = new long[10];
            g[0] = (f[4] + f[6]) % mod;
            g[1] = (f[6] + f[8]) % mod;
            g[2] = (f[7] + f[9]) % mod;
            g[3] = (f[4] + f[8]) % mod;
            g[4] = (f[0] + f[3] + f[9]) % mod;
            g[6] = (f[0] + f[1] + f[7]) % mod;
            g[7] = (f[2] + f[6]) % mod;
            g[8] = (f[1] + f[3]) % mod;
            g[9] = (f[2] + f[4]) % mod;
            f = g;
        }
        return (int) (Arrays.stream(f).sum() % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int knightDialer(int n) {
        const int mod = 1e9 + 7;
        vector<long long> f(10, 1);
        while (--n) {
            vector<long long> g(10);
            g[0] = (f[4] + f[6]) % mod;
            g[1] = (f[6] + f[8]) % mod;
            g[2] = (f[7] + f[9]) % mod;
            g[3] = (f[4] + f[8]) % mod;
            g[4] = (f[0] + f[3] + f[9]) % mod;
            g[6] = (f[0] + f[1] + f[7]) % mod;
            g[7] = (f[2] + f[6]) % mod;
            g[8] = (f[1] + f[3]) % mod;
            g[9] = (f[2] + f[4]) % mod;
            f = g;
        }
        return accumulate(f.begin(), f.end(), 0LL) % mod;
    }
};
```

#### Go

```go
func knightDialer(n int) (ans int) {
	f := make([]int, 10)
	for i := range f {
		f[i] = 1
	}
	const mod int = 1e9 + 7
	for i := 1; i < n; i++ {
		g := make([]int, 10)
		g[0] = (f[4] + f[6]) % mod
		g[1] = (f[6] + f[8]) % mod
		g[2] = (f[7] + f[9]) % mod
		g[3] = (f[4] + f[8]) % mod
		g[4] = (f[0] + f[3] + f[9]) % mod
		g[6] = (f[0] + f[1] + f[7]) % mod
		g[7] = (f[2] + f[6]) % mod
		g[8] = (f[1] + f[3]) % mod
		g[9] = (f[2] + f[4]) % mod
		f = g
	}
	for _, x := range f {
		ans = (ans + x) % mod
	}
	return
}
```

#### TypeScript

```ts
function knightDialer(n: number): number {
    const mod = 1e9 + 7;
    const f: number[] = Array(10).fill(1);
    while (--n) {
        const g: number[] = Array(10).fill(0);
        g[0] = (f[4] + f[6]) % mod;
        g[1] = (f[6] + f[8]) % mod;
        g[2] = (f[7] + f[9]) % mod;
        g[3] = (f[4] + f[8]) % mod;
        g[4] = (f[0] + f[3] + f[9]) % mod;
        g[6] = (f[0] + f[1] + f[7]) % mod;
        g[7] = (f[2] + f[6]) % mod;
        g[8] = (f[1] + f[3]) % mod;
        g[9] = (f[2] + f[4]) % mod;
        f.splice(0, 10, ...g);
    }
    return f.reduce((a, b) => (a + b) % mod);
}
```

#### C#

```cs
public class Solution {
    public int KnightDialer(int n) {
        const int mod = 1000000007;
        long[] f = new long[10];
        for (int i = 0; i < 10; i++) {
            f[i] = 1;
        }

        while (--n > 0) {
            long[] g = new long[10];
            g[0] = (f[4] + f[6]) % mod;
            g[1] = (f[6] + f[8]) % mod;
            g[2] = (f[7] + f[9]) % mod;
            g[3] = (f[4] + f[8]) % mod;
            g[4] = (f[0] + f[3] + f[9]) % mod;
            g[6] = (f[0] + f[1] + f[7]) % mod;
            g[7] = (f[2] + f[6]) % mod;
            g[8] = (f[1] + f[3]) % mod;
            g[9] = (f[2] + f[4]) % mod;
            f = g;
        }

        return (int)(f.Sum() % mod);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lũy thừa ma trận để tăng tốc công thức truy hồi

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 có độ phức tạp tuyến tính theo $n$. Cùng công thức truy hồi này có thể biểu diễn bằng phép biến đổi tuyến tính cố định dưới dạng ma trận $10\times 10$; lũy thừa ma trận tính $T(n)$ chỉ với $O(\log n)$ phép nhân.

<!-- thinking:end -->

Gọi $T(n)$ là ma trận $1 \times 10$ $\begin{bmatrix} F_0 & F_1 & F_2 \cdots F_9 \end{bmatrix}$, trong đó $F_i$ là số lượng số điện thoại kết thúc bằng chữ số $i$. Ta muốn suy ra $T(n)$ từ $T(n - 1)$. Nói cách khác, cần một ma trận $\textit{base}$ sao cho $T(n - 1) \times \textit{base} = T(n)$, tức là:

$$
\begin{bmatrix}
F_0 & F_1 & F_2 \cdots F_9
\end{bmatrix} \times \textit{base} = \begin{bmatrix} F_0' & F_1' & F_2' \cdots F_9' \end{bmatrix}
$$

Vì $F_i' = \sum_{j} F_j$, trong đó $j$ là chữ số có thể đứng trước $i$, nên cột đầu tiên của ma trận $\textit{base}$ là:

$$
\begin{bmatrix}
0 \\
0 \\
0 \\
0 \\
1 \\
0 \\
1 \\
0 \\
0 \\
0
\end{bmatrix}
$$

Tương tự, ta suy ra toàn bộ ma trận $\textit{base}$ như sau:

$$
\begin{bmatrix}
0 & 0 & 0 & 0 & 1 & 0 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 1 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 1 \\
0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 1 & 0 \\
1 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 1 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
0 & 1 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 1 & 0 & 0 & 0 & 0 & 0
\end{bmatrix}
$$

Ta đặt ma trận ban đầu $res = \begin{bmatrix} 1 & 1 & 1 \cdots 1 \end{bmatrix}$, rồi nhân nó với ma trận $\textit{base}$ lũy thừa $n - 1$ để thu được $T(n)$. Cuối cùng, tính tổng các phần tử của $T(n)$ rồi lấy modulo $10^9 + 7$ để nhận đáp án. Có thể tính ma trận $\textit{base}^{n - 1}$ bằng lũy thừa ma trận với độ phức tạp thời gian $O(\log n)$.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(|\Sigma|^2)$, với $\Sigma$ là tập chữ số; trong bài này, $|\Sigma| = 10$.

<!-- tabs:start -->

#### Python3

```python
import numpy as np

base = [
    (0, 0, 0, 0, 1, 0, 1, 0, 0, 0),
    (0, 0, 0, 0, 0, 0, 1, 0, 1, 0),
    (0, 0, 0, 0, 0, 0, 0, 1, 0, 1),
    (0, 0, 0, 0, 1, 0, 0, 0, 1, 0),
    (1, 0, 0, 1, 0, 0, 0, 0, 0, 1),
    (0, 0, 0, 0, 0, 0, 0, 0, 0, 0),
    (1, 1, 0, 0, 0, 0, 0, 1, 0, 0),
    (0, 0, 1, 0, 0, 0, 1, 0, 0, 0),
    (0, 1, 0, 1, 0, 0, 0, 0, 0, 0),
    (0, 0, 1, 0, 1, 0, 0, 0, 0, 0),
]


class Solution:
    def knightDialer(self, n: int) -> int:
        factor = np.asmatrix(base, np.dtype("O"))
        res = np.asmatrix([[1] * 10], np.dtype("O"))
        n -= 1
        mod = 10**9 + 7
        while n:
            if n & 1:
                res = res * factor % mod
            factor = factor * factor % mod
            n >>= 1
        return res.sum() % mod
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;
    private final int[][] base = {{0, 0, 0, 0, 1, 0, 1, 0, 0, 0}, {0, 0, 0, 0, 0, 0, 1, 0, 1, 0},
        {0, 0, 0, 0, 0, 0, 0, 1, 0, 1}, {0, 0, 0, 0, 1, 0, 0, 0, 1, 0},
        {1, 0, 0, 1, 0, 0, 0, 0, 0, 1}, {0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
        {1, 1, 0, 0, 0, 0, 0, 1, 0, 0}, {0, 0, 1, 0, 0, 0, 1, 0, 0, 0},
        {0, 1, 0, 1, 0, 0, 0, 0, 0, 0}, {0, 0, 1, 0, 1, 0, 0, 0, 0, 0}};

    public int knightDialer(int n) {
        int[][] res = pow(base, n - 1);
        int ans = 0;
        for (int x : res[0]) {
            ans = (ans + x) % mod;
        }
        return ans;
    }

    private int[][] mul(int[][] a, int[][] b) {
        int m = a.length, n = b[0].length;
        int[][] c = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.length; ++k) {
                    c[i][j] = (int) ((c[i][j] + 1L * a[i][k] * b[k][j] % mod) % mod);
                }
            }
        }
        return c;
    }

    private int[][] pow(int[][] a, int n) {
        int[][] res = new int[1][a.length];
        Arrays.fill(res[0], 1);
        while (n > 0) {
            if ((n & 1) == 1) {
                res = mul(res, a);
            }
            a = mul(a, a);
            n >>= 1;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int knightDialer(int n) {
        const int mod = 1e9 + 7;
        vector<vector<int>> base = {
            {0, 0, 0, 0, 1, 0, 1, 0, 0, 0},
            {0, 0, 0, 0, 0, 0, 1, 0, 1, 0},
            {0, 0, 0, 0, 0, 0, 0, 1, 0, 1},
            {0, 0, 0, 0, 1, 0, 0, 0, 1, 0},
            {1, 0, 0, 1, 0, 0, 0, 0, 0, 1},
            {0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
            {1, 1, 0, 0, 0, 0, 0, 1, 0, 0},
            {0, 0, 1, 0, 0, 0, 1, 0, 0, 0},
            {0, 1, 0, 1, 0, 0, 0, 0, 0, 0},
            {0, 0, 1, 0, 1, 0, 0, 0, 0, 0}};
        vector<vector<int>> res = pow(base, n - 1, mod);
        return accumulate(res[0].begin(), res[0].end(), 0LL) % mod;
    }

private:
    vector<vector<int>> mul(const vector<vector<int>>& a, const vector<vector<int>>& b, int mod) {
        int m = a.size(), n = b[0].size();
        vector<vector<int>> c(m, vector<int>(n, 0));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.size(); ++k) {
                    c[i][j] = (c[i][j] + (1LL * a[i][k] * b[k][j]) % mod) % mod;
                }
            }
        }
        return c;
    }

    vector<vector<int>> pow(vector<vector<int>>& a, int n, int mod) {
        int size = a.size();
        vector<vector<int>> res(1, vector<int>(size, 1));
        while (n > 0) {
            if (n % 2 == 1) {
                res = mul(res, a, mod);
            }
            a = mul(a, a, mod);
            n /= 2;
        }
        return res;
    }
};
```

#### Go

```go
const mod = 1e9 + 7

func knightDialer(n int) int {
	base := [][]int{
		{0, 0, 0, 0, 1, 0, 1, 0, 0, 0},
		{0, 0, 0, 0, 0, 0, 1, 0, 1, 0},
		{0, 0, 0, 0, 0, 0, 0, 1, 0, 1},
		{0, 0, 0, 0, 1, 0, 0, 0, 1, 0},
		{1, 0, 0, 1, 0, 0, 0, 0, 0, 1},
		{0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
		{1, 1, 0, 0, 0, 0, 0, 1, 0, 0},
		{0, 0, 1, 0, 0, 0, 1, 0, 0, 0},
		{0, 1, 0, 1, 0, 0, 0, 0, 0, 0},
		{0, 0, 1, 0, 1, 0, 0, 0, 0, 0},
	}

	res := pow(base, n-1)
	ans := 0
	for _, x := range res[0] {
		ans = (ans + x) % mod
	}
	return ans
}

func mul(a, b [][]int) [][]int {
	m := len(a)
	n := len(b[0])
	c := make([][]int, m)
	for i := range c {
		c[i] = make([]int, n)
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			for k := 0; k < len(b); k++ {
				c[i][j] = (c[i][j] + a[i][k]*b[k][j]) % mod
			}
		}
	}
	return c
}

func pow(a [][]int, n int) [][]int {
	size := len(a)
	res := make([][]int, 1)
	res[0] = make([]int, size)
	for i := 0; i < size; i++ {
		res[0][i] = 1
	}

	for n > 0 {
		if n%2 == 1 {
			res = mul(res, a)
		}
		a = mul(a, a)
		n /= 2
	}

	return res
}
```

#### TypeScript

```ts
const mod = 1e9 + 7;

function knightDialer(n: number): number {
    const base: number[][] = [
        [0, 0, 0, 0, 1, 0, 1, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 1, 0, 1, 0],
        [0, 0, 0, 0, 0, 0, 0, 1, 0, 1],
        [0, 0, 0, 0, 1, 0, 0, 0, 1, 0],
        [1, 0, 0, 1, 0, 0, 0, 0, 0, 1],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [1, 1, 0, 0, 0, 0, 0, 1, 0, 0],
        [0, 0, 1, 0, 0, 0, 1, 0, 0, 0],
        [0, 1, 0, 1, 0, 0, 0, 0, 0, 0],
        [0, 0, 1, 0, 1, 0, 0, 0, 0, 0],
    ];

    const res = pow(base, n - 1);
    let ans = 0;
    for (const x of res[0]) {
        ans = (ans + x) % mod;
    }
    return ans;
}

function mul(a: number[][], b: number[][]): number[][] {
    const m = a.length;
    const n = b[0].length;
    const c: number[][] = Array.from({ length: m }, () => Array(n).fill(0));

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            for (let k = 0; k < b.length; k++) {
                c[i][j] =
                    (c[i][j] + Number((BigInt(a[i][k]) * BigInt(b[k][j])) % BigInt(mod))) % mod;
            }
        }
    }
    return c;
}

function pow(a: number[][], n: number): number[][] {
    const size = a.length;
    let res: number[][] = Array.from({ length: 1 }, () => Array(size).fill(1));

    while (n > 0) {
        if (n % 2 === 1) {
            res = mul(res, a);
        }
        a = mul(a, a);
        n = Math.floor(n / 2);
    }

    return res;
}
```

#### C#

```cs
public class Solution {
    private const int mod = 1000000007;
    private readonly int[][] baseMatrix = {
        new int[] {0, 0, 0, 0, 1, 0, 1, 0, 0, 0},
        new int[] {0, 0, 0, 0, 0, 0, 1, 0, 1, 0},
        new int[] {0, 0, 0, 0, 0, 0, 0, 1, 0, 1},
        new int[] {0, 0, 0, 0, 1, 0, 0, 0, 1, 0},
        new int[] {1, 0, 0, 1, 0, 0, 0, 0, 0, 1},
        new int[] {0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
        new int[] {1, 1, 0, 0, 0, 0, 0, 1, 0, 0},
        new int[] {0, 0, 1, 0, 0, 0, 1, 0, 0, 0},
        new int[] {0, 1, 0, 1, 0, 0, 0, 0, 0, 0},
        new int[] {0, 0, 1, 0, 1, 0, 0, 0, 0, 0}
    };

    public int KnightDialer(int n) {
        int[][] res = Pow(baseMatrix, n - 1);
        int ans = 0;
        foreach (var x in res[0]) {
            ans = (ans + x) % mod;
        }
        return ans;
    }

    private int[][] Mul(int[][] a, int[][] b) {
        int m = a.Length, n = b[0].Length;
        int[][] c = new int[m][];
        for (int i = 0; i < m; i++) {
            c[i] = new int[n];
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < b.Length; k++) {
                    c[i][j] = (int)((c[i][j] + (long)a[i][k] * b[k][j]) % mod);
                }
            }
        }
        return c;
    }

    private int[][] Pow(int[][] a, int n) {
        int size = a.Length;
        int[][] res = new int[1][];
        res[0] = new int[size];
        for (int i = 0; i < size; i++) {
            res[0][i] = 1;
        }

        while (n > 0) {
            if (n % 2 == 1) {
                res = Mul(res, a);
            }
            a = Mul(a, a);
            n /= 2;
        }

        return res;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
