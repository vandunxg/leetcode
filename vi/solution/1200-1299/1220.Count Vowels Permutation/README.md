---
comments: true
difficulty: Hard
rating: 1729
source: Weekly Contest 157 Q4
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [1220. Count Vowels Permutation](https://leetcode.com/problems/count-vowels-permutation)

[中文文档](/solution/1200-1299/1220.Count%20Vowels%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy đếm số chuỗi độ dài <code>n</code> có thể tạo theo các quy tắc sau:</p>

<ul>
	<li>Mỗi ký tự là một nguyên âm viết thường&nbsp;(<code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code>, <code>&#39;u&#39;</code>)</li>
	<li>Sau nguyên âm&nbsp;<code>&#39;a&#39;</code> chỉ có thể là <code>&#39;e&#39;</code>.</li>
	<li>Sau nguyên âm&nbsp;<code>&#39;e&#39;</code> chỉ có thể là <code>&#39;a&#39;</code>&nbsp;hoặc <code>&#39;i&#39;</code>.</li>
	<li>Sau nguyên âm&nbsp;<code>&#39;i&#39;</code> <strong>không thể</strong> là một <code>&#39;i&#39;</code> khác.</li>
	<li>Sau nguyên âm&nbsp;<code>&#39;o&#39;</code> chỉ có thể là <code>&#39;i&#39;</code> hoặc&nbsp;<code>&#39;u&#39;</code>.</li>
	<li>Sau nguyên âm&nbsp;<code>&#39;u&#39;</code> chỉ có thể là <code>&#39;a&#39;</code>.</li>
</ul>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả modulo&nbsp;<code>10^9 + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các chuỗi có thể tạo là: &quot;a&quot;, &quot;e&quot;, &quot;i&quot; , &quot;o&quot; và &quot;u&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Các chuỗi có thể tạo là: &quot;ae&quot;, &quot;ea&quot;, &quot;ei&quot;, &quot;ia&quot;, &quot;ie&quot;, &quot;io&quot;, &quot;iu&quot;, &quot;oi&quot;, &quot;ou&quot; và &quot;ua&quot;.
</pre>

<p><strong class="example">Ví dụ 3:&nbsp;</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 68</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10^4</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Các chuỗi nguyên âm độ dài $n$ phải tuân theo quy tắc về ký tự đứng liền sau. Vì $n$ có thể lên tới $2\times 10^4$, ta không thể liệt kê từng chuỗi. Chỉ cần đếm số chuỗi theo nguyên âm cuối, vì mỗi nguyên âm có một tập ký tự đứng trước hợp lệ cố định.
>
> Mảng $f$ có độ dài $5$ lưu số chuỗi theo nguyên âm cuối; ta cập nhật mảng này $n-1$ lần dựa trên các ký tự đứng trước hợp lệ. Mỗi lần chỉ cần trạng thái của độ dài trước đó. Ta lấy modulo $10^9+7$.

<!-- thinking:end -->

Dựa trên mô tả bài toán, ta liệt kê các nguyên âm có thể đứng sau mỗi nguyên âm:

```bash
a [e]
e [a|i]
i [a|e|o|u]
o [i|u]
u [a]
```

Từ đó, ta suy ra các nguyên âm có thể đứng trước mỗi nguyên âm:

```bash
[e|i|u]	a
[a|i]	e
[e|o]	i
[i]	o
[i|o]	u
```

Ta định nghĩa $f[i]$ là số chuỗi ở độ dài hiện tại kết thúc bằng nguyên âm thứ $i$. Nếu độ dài bằng $1$, thì $f[i]=1$.

Khi độ dài lớn hơn $1$, ta định nghĩa $g[i]$ là số chuỗi ở độ dài hiện tại kết thúc bằng nguyên âm thứ $i$. Khi đó, có thể tính $g[i]$ từ $f$ như sau:

$$
g[i]=
\begin{cases}
f[1]+f[2]+f[4] & i=0 \\
f[0]+f[2] & i=1 \\
f[1]+f[3] & i=2 \\
f[2] & i=3 \\
f[2]+f[3] & i=4
\end{cases}
$$

Đáp án là $\sum_{i=0}^{4}f[i]$. Vì đáp án có thể rất lớn, ta cần lấy modulo $10^9+7$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$, trong đó $n$ là độ dài chuỗi, còn $C$ là số nguyên âm. Trong bài này, $C=5$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countVowelPermutation(self, n: int) -> int:
        f = [1] * 5
        mod = 10**9 + 7
        for _ in range(n - 1):
            g = [0] * 5
            g[0] = (f[1] + f[2] + f[4]) % mod
            g[1] = (f[0] + f[2]) % mod
            g[2] = (f[1] + f[3]) % mod
            g[3] = f[2]
            g[4] = (f[2] + f[3]) % mod
            f = g
        return sum(f) % mod
```

#### Java

```java
class Solution {
    public int countVowelPermutation(int n) {
        long[] f = new long[5];
        Arrays.fill(f, 1);
        final int mod = (int) 1e9 + 7;
        for (int i = 1; i < n; ++i) {
            long[] g = new long[5];
            g[0] = (f[1] + f[2] + f[4]) % mod;
            g[1] = (f[0] + f[2]) % mod;
            g[2] = (f[1] + f[3]) % mod;
            g[3] = f[2];
            g[4] = (f[2] + f[3]) % mod;
            f = g;
        }
        long ans = 0;
        for (long x : f) {
            ans = (ans + x) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countVowelPermutation(int n) {
        using ll = long long;
        vector<ll> f(5, 1);
        const int mod = 1e9 + 7;
        for (int i = 1; i < n; ++i) {
            vector<ll> g(5);
            g[0] = (f[1] + f[2] + f[4]) % mod;
            g[1] = (f[0] + f[2]) % mod;
            g[2] = (f[1] + f[3]) % mod;
            g[3] = f[2];
            g[4] = (f[2] + f[3]) % mod;
            f = move(g);
        }
        return accumulate(f.begin(), f.end(), 0LL) % mod;
    }
};
```

#### Go

```go
func countVowelPermutation(n int) (ans int) {
	const mod int = 1e9 + 7
	f := make([]int, 5)
	for i := range f {
		f[i] = 1
	}
	for i := 1; i < n; i++ {
		g := make([]int, 5)
		g[0] = (f[1] + f[2] + f[4]) % mod
		g[1] = (f[0] + f[2]) % mod
		g[2] = (f[1] + f[3]) % mod
		g[3] = f[2] % mod
		g[4] = (f[2] + f[3]) % mod
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
function countVowelPermutation(n: number): number {
    const f: number[] = Array(5).fill(1);
    const mod = 1e9 + 7;
    for (let i = 1; i < n; ++i) {
        const g: number[] = Array(5).fill(0);
        g[0] = (f[1] + f[2] + f[4]) % mod;
        g[1] = (f[0] + f[2]) % mod;
        g[2] = (f[1] + f[3]) % mod;
        g[3] = f[2];
        g[4] = (f[2] + f[3]) % mod;
        f.splice(0, 5, ...g);
    }
    return f.reduce((a, b) => (a + b) % mod);
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var countVowelPermutation = function (n) {
    const mod = 1e9 + 7;
    const f = Array(5).fill(1);
    for (let i = 1; i < n; ++i) {
        const g = Array(5).fill(0);
        g[0] = (f[1] + f[2] + f[4]) % mod;
        g[1] = (f[0] + f[2]) % mod;
        g[2] = (f[1] + f[3]) % mod;
        g[3] = f[2];
        g[4] = (f[2] + f[3]) % mod;
        f.splice(0, 5, ...g);
    }
    return f.reduce((a, b) => (a + b) % mod);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lũy thừa ma trận để tăng tốc phép truy hồi

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 thực hiện $O(n)$ lần cập nhật trên một cấu trúc kích thước cố định. Truy hồi với năm trạng thái là một phép biến đổi tuyến tính. Biểu diễn phép biến đổi này bằng ma trận $5\times 5$, rồi dùng lũy thừa nhị phân để tính vector thứ $n$ chỉ trong $O(\log n)$ phép nhân; cách này hiệu quả hơn khi $n$ lớn.

<!-- thinking:end -->

Độ phức tạp thời gian là $O(C^3 \times \log n)$ và độ phức tạp không gian là $O(C^2)$, trong đó $C$ là số nguyên âm. Trong bài này, $C=5$.

<!-- tabs:start -->

#### Python3

```python
import numpy as np


class Solution:
    def countVowelPermutation(self, n: int) -> int:
        mod = 10**9 + 7
        factor = np.asmatrix(
            [
                (0, 1, 0, 0, 0),
                (1, 0, 1, 0, 0),
                (1, 1, 0, 1, 1),
                (0, 0, 1, 0, 1),
                (1, 0, 0, 0, 0),
            ],
            np.dtype("O"),
        )
        res = np.asmatrix([(1, 1, 1, 1, 1)], np.dtype("O"))
        n -= 1
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

    public int countVowelPermutation(int n) {
        long[][] a
            = {{0, 1, 0, 0, 0}, {1, 0, 1, 0, 0}, {1, 1, 0, 1, 1}, {0, 0, 1, 0, 1}, {1, 0, 0, 0, 0}};
        long[][] res = pow(a, n - 1);
        long ans = 0;
        for (long x : res[0]) {
            ans = (ans + x) % mod;
        }
        return (int) ans;
    }

    private long[][] mul(long[][] a, long[][] b) {
        int m = a.length, n = b[0].length;
        long[][] c = new long[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.length; ++k) {
                    c[i][j] = (c[i][j] + a[i][k] * b[k][j]) % mod;
                }
            }
        }
        return c;
    }

    private long[][] pow(long[][] a, int n) {
        long[][] res = new long[1][a.length];
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
    int countVowelPermutation(int n) {
        vector<vector<ll>> a = {
            {0, 1, 0, 0, 0},
            {1, 0, 1, 0, 0},
            {1, 1, 0, 1, 1},
            {0, 0, 1, 0, 1},
            {1, 0, 0, 0, 0}};
        vector<vector<ll>> res = pow(a, n - 1);
        return accumulate(res[0].begin(), res[0].end(), 0LL) % mod;
    }

private:
    using ll = long long;
    const int mod = 1e9 + 7;

    vector<vector<ll>> mul(vector<vector<ll>>& a, vector<vector<ll>>& b) {
        int m = a.size(), n = b[0].size();
        vector<vector<ll>> c(m, vector<ll>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.size(); ++k) {
                    c[i][j] = (c[i][j] + a[i][k] * b[k][j]) % mod;
                }
            }
        }
        return c;
    }

    vector<vector<ll>> pow(vector<vector<ll>>& a, int n) {
        vector<vector<ll>> res;
        res.push_back({1, 1, 1, 1, 1});
        while (n) {
            if (n & 1) {
                res = mul(res, a);
            }
            a = mul(a, a);
            n >>= 1;
        }
        return res;
    }
};
```

#### Go

```go
const mod = 1e9 + 7

func countVowelPermutation(n int) (ans int) {
	a := [][]int{
		{0, 1, 0, 0, 0},
		{1, 0, 1, 0, 0},
		{1, 1, 0, 1, 1},
		{0, 0, 1, 0, 1},
		{1, 0, 0, 0, 0}}
	res := pow(a, n-1)
	for _, x := range res[0] {
		ans = (ans + x) % mod
	}
	return
}

func mul(a, b [][]int) [][]int {
	m, n := len(a), len(b[0])
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
	res := [][]int{{1, 1, 1, 1, 1}}
	for n > 0 {
		if n&1 == 1 {
			res = mul(res, a)
		}
		a = mul(a, a)
		n >>= 1
	}
	return res
}
```

#### TypeScript

```ts
const mod = 1e9 + 7;

function countVowelPermutation(n: number): number {
    const a: number[][] = [
        [0, 1, 0, 0, 0],
        [1, 0, 1, 0, 0],
        [1, 1, 0, 1, 1],
        [0, 0, 1, 0, 1],
        [1, 0, 0, 0, 0],
    ];
    const res = pow(a, n - 1);
    return res[0].reduce((a, b) => (a + b) % mod);
}

function mul(a: number[][], b: number[][]): number[][] {
    const [m, n] = [a.length, b[0].length];
    const c = Array.from({ length: m }, () => Array.from({ length: n }, () => 0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            for (let k = 0; k < b.length; ++k) {
                c[i][j] =
                    (c[i][j] + Number((BigInt(a[i][k]) * BigInt(b[k][j])) % BigInt(mod))) % mod;
            }
        }
    }
    return c;
}

function pow(a: number[][], n: number): number[][] {
    let res: number[][] = [[1, 1, 1, 1, 1]];
    while (n) {
        if (n & 1) {
            res = mul(res, a);
        }
        a = mul(a, a);
        n >>>= 1;
    }
    return res;
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */

const mod = 1e9 + 7;

var countVowelPermutation = function (n) {
    const a = [
        [0, 1, 0, 0, 0],
        [1, 0, 1, 0, 0],
        [1, 1, 0, 1, 1],
        [0, 0, 1, 0, 1],
        [1, 0, 0, 0, 0],
    ];
    const res = pow(a, n - 1);
    return res[0].reduce((a, b) => (a + b) % mod);
};

function mul(a, b) {
    const [m, n] = [a.length, b[0].length];
    const c = Array.from({ length: m }, () => Array.from({ length: n }, () => 0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            for (let k = 0; k < b.length; ++k) {
                c[i][j] =
                    (c[i][j] + Number((BigInt(a[i][k]) * BigInt(b[k][j])) % BigInt(mod))) % mod;
            }
        }
    }
    return c;
}

function pow(a, n) {
    let res = [[1, 1, 1, 1, 1]];
    while (n) {
        if (n & 1) {
            res = mul(res, a);
        }
        a = mul(a, a);
        n >>>= 1;
    }
    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
