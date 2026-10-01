---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [08.01. Three Steps Problem](https://leetcode.cn/problems/three-steps-problem-lcci)

[中文文档](/lcci/08.01.Three%20Steps%20Problem/README.md)

## Mô tả

<!-- description:start -->

<p>Một đứa trẻ đang chạy lên cầu thang có n bậc và mỗi lần có thể bước 1 bậc, 2 bậc hoặc 3 bậc. Hãy cài đặt một phương thức để đếm số cách có thể chạy lên cầu thang. Kết quả có thể rất lớn, vì vậy hãy trả về phần dư khi chia cho 1000000007.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong> Đầu vào</strong>: n = 3

<strong> Đầu ra</strong>: 4

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong> Đầu vào</strong>: n = 5

<strong> Đầu ra</strong>: 13

</pre>

<p><strong>Lưu ý:</strong></p>

1. `1 <= n <= 1000000`

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Có thể bước $1$, $2$ hoặc $3$ bậc. Đệ quy trực tiếp trên ba lựa chọn sẽ tính lại cùng một hậu tố nhiều lần.
>
> Số cách tại $i$ chỉ phụ thuộc vào $i-1,i-2,i-3$, vì vậy chỉ cần một truy hồi tuyến tính kèm phép modulo.
>
> Ba biến cuộn $a,b,c$ lưu các số hạng liên tiếp; sau $n-1$ lần cập nhật thì $a$ là $f(n)$. Không gian phụ là hằng số.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số cách đi đến bậc thứ $i$, ban đầu $f[1]=1$, $f[2]=2$, $f[3]=4$. Đáp án là $f[n]$.

Công thức truy hồi là $f[i] = f[i-1] + f[i-2] + f[i-3]$.

Vì $f[i]$ chỉ liên quan đến $f[i-1]$, $f[i-2]$, $f[i-3]$, ta có thể dùng ba biến $a$, $b$, $c$ để lưu các giá trị của $f[i-1]$, $f[i-2]$, $f[i-3]$, giảm độ phức tạp không gian xuống $O(1)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên đã cho. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToStep(self, n: int) -> int:
        a, b, c = 1, 2, 4
        mod = 10**9 + 7
        for _ in range(n - 1):
            a, b, c = b, c, (a + b + c) % mod
        return a
```

#### Java

```java
class Solution {
    public int waysToStep(int n) {
        final int mod = (int) 1e9 + 7;
        int a = 1, b = 2, c = 4;
        for (int i = 1; i < n; ++i) {
            int t = a;
            a = b;
            b = c;
            c = (((a + b) % mod) + t) % mod;
        }
        return a;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int waysToStep(int n) {
        const int mod = 1e9 + 7;
        int a = 1, b = 2, c = 4;
        for (int i = 1; i < n; ++i) {
            int t = a;
            a = b;
            b = c;
            c = (((a + b) % mod) + t) % mod;
        }
        return a;
    }
};
```

#### Go

```go
func waysToStep(n int) int {
	const mod int = 1e9 + 7
	a, b, c := 1, 2, 4
	for i := 1; i < n; i++ {
		a, b, c = b, c, (a+b+c)%mod
	}
	return a
}
```

#### Rust

```rust
impl Solution {
    pub fn ways_to_step(n: i32) -> i32 {
        let (mut a, mut b, mut c) = (1, 2, 4);
        let m = 1000000007;
        for _ in 1..n {
            let t = a;
            a = b;
            b = c;
            c = (((a + b) % m) + t) % m;
        }
        a
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */
var waysToStep = function (n) {
    let [a, b, c] = [1, 2, 4];
    const mod = 1e9 + 7;
    for (let i = 1; i < n; ++i) {
        [a, b, c] = [b, c, (a + b + c) % mod];
    }
    return a;
};
```

#### C

```c
int waysToStep(int n) {
    const int mod = 1e9 + 7;
    int a = 1, b = 2, c = 4;
    for (int i = 1; i < n; ++i) {
        int t = a;
        a = b;
        b = c;
        c = (((a + b) % mod) + t) % mod;
    }
    return a;
}
```

#### Swift

```swift
class Solution {
    func waysToStep(_ n: Int) -> Int {
        let mod = Int(1e9) + 7
        var a = 1, b = 2, c = 4
        if n == 1 { return a }
        if n == 2 { return b }
        if n == 3 { return c }

        for _ in 1..<n {
            let t = a
            a = b
            b = c
            c = ((a + b) % mod + t) % mod
        }
        return a
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lũy thừa nhanh ma trận để tăng tốc đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Quét tuyến tính có độ phức tạp $\Theta(n)$ và phần lớn thời gian được dùng cho các phép cộng modulo khi $n$ lớn.
>
> Truy hồi bậc $3$ là tích của state $1\times 3$ với ma trận $3\times 3$, nên có thể tính bằng lũy thừa ma trận trong $O(\log n)$ phép nhân.

<!-- thinking:end -->

Ta đặt $F(n)$ biểu diễn một ma trận $1 \times 3$ $\begin{bmatrix} F_{n - 1} & F_{n - 2} & F_{n - 3} \end{bmatrix}$, trong đó $F_{n - 1}$, $F_{n - 2}$ và $F_{n - 3}$ lần lượt biểu diễn số cách đi đến bậc thứ $n - 1$, $n - 2$ và $n - 3$.

Ta muốn suy ra $F(n)$ dựa trên $F(n-1) = \begin{bmatrix} F_{n - 2} & F_{n - 3} & F_{n - 4} \end{bmatrix}$. Nói cách khác, ta cần một ma trận $base$ sao cho $F(n - 1) \times base = F(n)$, tức là:

$$
\begin{bmatrix}
F_{n - 2} & F_{n - 3} & F_{n - 4}
\end{bmatrix} \times base = \begin{bmatrix} F_{n - 1} & F_{n - 2} & F_{n - 3} \end{bmatrix}
$$

Vì $F_n = F_{n - 1} + F_{n - 2} + F_{n - 3}$, ma trận $base$ là:

$$
\begin{bmatrix}
 1 & 1 & 0 \\
 1 & 0 & 1 \\
 1 & 0 & 0
\end{bmatrix}
$$

Ta định nghĩa ma trận ban đầu $res = \begin{bmatrix} 1 & 1 & 0 \end{bmatrix}$, khi đó $F_n$ bằng tổng tất cả phần tử trong ma trận kết quả của $res$ nhân với $base^{n - 4}$. Có thể giải bằng lũy thừa nhanh ma trận.

Độ phức tạp thời gian là $O(\log n)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
import numpy as np


class Solution:
    def waysToStep(self, n: int) -> int:
        if n < 4:
            return 2 ** (n - 1)
        mod = 10**9 + 7
        factor = np.asmatrix([(1, 1, 0), (1, 0, 1), (1, 0, 0)], np.dtype("O"))
        res = np.asmatrix([(4, 2, 1)], np.dtype("O"))
        n -= 4
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

    public int waysToStep(int n) {
        if (n < 4) {
            return (int) Math.pow(2, n - 1);
        }
        long[][] a = {{1, 1, 0}, {1, 0, 1}, {1, 0, 0}};
        long[][] res = pow(a, n - 4);
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
                    c[i][j] = (c[i][j] + a[i][k] * b[k][j] % mod) % mod;
                }
            }
        }
        return c;
    }

    private long[][] pow(long[][] a, int n) {
        long[][] res = {{4, 2, 1}};
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
    int waysToStep(int n) {
        if (n < 4) {
            return pow(2, n - 1);
        }
        vector<vector<ll>> a = {{1, 1, 0}, {1, 0, 1}, {1, 0, 0}};
        vector<vector<ll>> res = qpow(a, n - 4);
        ll ans = 0;
        for (ll x : res[0]) {
            ans = (ans + x) % mod;
        }
        return ans;
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
                    c[i][j] = (c[i][j] + a[i][k] * b[k][j] % mod) % mod;
                }
            }
        }
        return c;
    }

    vector<vector<ll>> qpow(vector<vector<ll>>& a, int n) {
        vector<vector<ll>> res = {{4, 2, 1}};
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

func waysToStep(n int) (ans int) {
	if n < 4 {
		return int(math.Pow(2, float64(n-1)))
	}
	a := [][]int{{1, 1, 0}, {1, 0, 1}, {1, 0, 0}}
	res := pow(a, n-4)
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
				c[i][j] = (c[i][j] + a[i][k]*b[k][j]%mod) % mod
			}
		}
	}
	return c
}

func pow(a [][]int, n int) [][]int {
	res := [][]int{{4, 2, 1}}
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

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number}
 */

const mod = 1e9 + 7;

var waysToStep = function (n) {
    if (n < 4) {
        return Math.pow(2, n - 1);
    }
    const a = [
        [1, 1, 0],
        [1, 0, 1],
        [1, 0, 0],
    ];
    let ans = 0;
    const res = pow(a, n - 4);
    for (const x of res[0]) {
        ans = (ans + x) % mod;
    }
    return ans;
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
    let res = [[4, 2, 1]];
    while (n) {
        if (n & 1) {
            res = mul(res, a);
        }
        a = mul(a, a);
        n >>= 1;
    }
    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
