---
comments: true
difficulty: Medium
rating: 1760
source: Biweekly Contest 179 Q2
tags:
    - Math
    - Combinatorics
---

<!-- problem:start -->

# [3881. Direction Assignments with Exactly K Visible People](https://leetcode.com/problems/direction-assignments-with-exactly-k-visible-people)

[中文文档](/solution/3800-3899/3881.Direction%20Assignments%20with%20Exactly%20K%20Visible%20People/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <code>n</code>, <code>pos</code> và <code>k</code>.</p>

<p>Có <code>n</code> người đứng thành một hàng, được đánh chỉ số từ 0 đến <code>n - 1</code>. Mỗi người <strong>độc lập</strong> chọn một hướng:</p>

<ul>
	<li><code>&#39;L&#39;</code>: chỉ <strong>được nhìn thấy</strong> bởi những người ở bên <strong>phải</strong> họ</li>
	<li><code>&#39;R&#39;</code>: chỉ <strong>được nhìn thấy</strong> bởi những người ở bên <strong>trái</strong> họ</li>
</ul>
Người ở chỉ số <code>pos</code> nhìn thấy những người khác như sau:

<ul>
	<li>Người ở chỉ số <code>i &lt; pos</code> được nhìn thấy khi và chỉ khi họ chọn <code>&#39;L&#39;</code>.</li>
	<li>Người ở chỉ số <code>i &gt; pos</code> được nhìn thấy khi và chỉ khi họ chọn <code>&#39;R&#39;</code>.</li>
</ul>

<p>Hãy trả về số cách gán hướng có thể có sao cho người ở chỉ số <code>pos</code> nhìn thấy <strong>chính xác</strong> <code>k</code> người.</p>

<p>Vì đáp án có thể lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, pos = 1, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Chỉ số 0 nằm bên trái <code>pos = 1</code>, còn chỉ số 2 nằm bên phải <code>pos = 1</code>.</li>
	<li>Để nhìn thấy <code>k = 0</code> người, chỉ số 0 phải chọn <code>&#39;R&#39;</code> và chỉ số 2 phải chọn <code>&#39;L&#39;</code>, khiến cả hai đều không được nhìn thấy.</li>
	<li>Người ở chỉ số 1 có thể chọn <code>&#39;L&#39;</code> hoặc <code>&#39;R&#39;</code> vì lựa chọn này không ảnh hưởng đến số lượng. Do đó, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, pos = 2, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chỉ số 0 và chỉ số 1 nằm bên trái <code>pos = 2</code>, không có chỉ số nào ở bên phải.</li>
	<li>Để nhìn thấy <code>k = 1</code> người, chính xác một trong hai chỉ số 0 và 1 phải chọn <code>&#39;L&#39;</code>, chỉ số còn lại chọn <code>&#39;R&#39;</code>.</li>
	<li>Có 2 cách chọn người được nhìn thấy từ bên trái.</li>
	<li>Người ở chỉ số 2 có thể chọn <code>&#39;L&#39;</code> hoặc <code>&#39;R&#39;</code> vì lựa chọn này không ảnh hưởng đến số lượng. Do đó, đáp án là <code>2 + 2 = 4</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1, pos = 0, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không có chỉ số nào ở bên trái hoặc bên phải <code>pos = 0</code>.</li>
	<li>Để nhìn thấy <code>k = 0</code> người, không cần thêm điều kiện nào.</li>
	<li>Người ở chỉ số 0 có thể chọn <code>&#39;L&#39;</code> hoặc <code>&#39;R&#39;</code>. Do đó, đáp án là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= pos, k &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổ hợp + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người chọn L hoặc R sao cho chỉ số $\textit{pos}$ nhìn thấy chính xác $k$ người. Vì $n \le 10^5$, không thể xét $2^n$ cách gán.
>
> Một người bên trái được nhìn thấy khi họ chọn L, một người bên phải được nhìn thấy khi họ chọn R, không phụ thuộc vào hướng của chính $\textit{pos}$; người ở $\textit{pos}$ vẫn có hai lựa chọn.
>
> Liệt kê $a$ người được nhìn thấy ở bên trái; khi đó bên phải cần $k-a$ người, đóng góp $2\binom{\textit{pos}}{a}\binom{n-\textit{pos}-1}{k-a}$.
>
> Giai thừa và nghịch đảo giúp tính mỗi hệ số nhị thức trong $O(1)$.

<!-- thinking:end -->

Có $\textit{pos}$ người ở bên trái vị trí $\textit{pos}$ và $n - \textit{pos} - 1$ người ở bên phải.

Ta liệt kê số người được nhìn thấy ở bên trái là $a$, khi đó số người được nhìn thấy ở bên phải là $b = k - a$. Nếu cả $a$ và $b$ đều hợp lệ, đáp án tăng thêm $2 \cdot \binom{\textit{pos}}{a} \cdot \binom{n - \textit{pos} - 1}{b}$. Thừa số $2$ xuất hiện vì người ở chỉ số $\textit{pos}$ có thể quay mặt sang 'L' hoặc 'R'.

Với hệ số nhị thức $\binom{n}{k}$, ta có thể tính trước các giai thừa và nghịch đảo modulo để tính nhanh.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên đầu vào $n$. Độ phức tạp không gian là $O(n)$ để lưu các giai thừa và nghịch đảo modulo.

<!-- tabs:start -->

#### Python3

```python
N = 100001
MOD = 10**9 + 7
f = [1] * N
g = [1] * N
for i in range(1, N):
    f[i] = f[i - 1] * i % MOD
    g[i] = pow(f[i], MOD - 2, MOD)


def comb(n, k):
    return f[n] * g[k] * g[n - k] % MOD


class Solution:
    def countVisiblePeople(self, n: int, pos: int, k: int) -> int:
        l, r = pos, n - pos - 1
        ans = 0
        for a in range(min(k, l) + 1):
            b = k - a
            if b <= r:
                ans += 2 * comb(l, a) * comb(r, b)
                ans %= MOD
        return ans
```

#### Java

```java
class Solution {
    private static final int N = 100001;
    private static final int MOD = (int) 1e9 + 7;
    private static final long[] F = new long[N];
    private static final long[] G = new long[N];

    static {
        F[0] = 1;
        G[0] = 1;
        for (int i = 1; i < N; ++i) {
            F[i] = F[i - 1] * i % MOD;
            G[i] = qmi(F[i], MOD - 2, MOD);
        }
    }

    public static long qmi(long a, long k, long p) {
        long res = 1;
        while (k != 0) {
            if ((k & 1) == 1) {
                res = res * a % p;
            }
            k >>= 1;
            a = a * a % p;
        }
        return res;
    }

    public static long comb(int n, int k) {
        return (F[n] * G[k] % MOD) * G[n - k] % MOD;
    }

    public int countVisiblePeople(int n, int pos, int k) {
        int l = pos, r = n - pos - 1;
        long ans = 0;

        for (int a = 0; a <= Math.min(k, l); ++a) {
            int b = k - a;
            if (b <= r) {
                ans = (ans + 2 * comb(l, a) % MOD * comb(r, b) % MOD) % MOD;
            }
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
int N = 100001;
int MOD = 1e9 + 7;
long long f[100001];
long long g[100001];

long long qmi(long long a, long long k, long long p) {
    long long res = 1;
    while (k != 0) {
        if ((k & 1) == 1) {
            res = res * a % p;
        }
        k >>= 1;
        a = a * a % p;
    }
    return res;
}

int init = []() {
    f[0] = 1;
    g[0] = 1;
    for (int i = 1; i < N; ++i) {
        f[i] = f[i - 1] * i % MOD;
        g[i] = qmi(f[i], MOD - 2, MOD);
    }
    return 0;
}();

long long comb(int n, int k) {
    return f[n] * g[k] % MOD * g[n - k] % MOD;
}

class Solution {
public:
    int countVisiblePeople(int n, int pos, int k) {
        int l = pos, r = n - pos - 1;
        long long ans = 0;

        for (int a = 0; a <= min(k, l); ++a) {
            int b = k - a;
            if (b <= r) {
                ans = (ans + 2 * comb(l, a) % MOD * comb(r, b) % MOD) % MOD;
            }
        }
        return ans;
    }
};
```

#### Go

```go
package main

const N = 100001
const MOD = 1e9 + 7

var f = make([]int, N)
var g = make([]int, N)

func qmi(a, k, p int) int {
	res := 1
	for k != 0 {
		if k&1 == 1 {
			res = res * a % p
		}
		k >>= 1
		a = a * a % p
	}
	return res
}

func init() {
	f[0], g[0] = 1, 1
	for i := 1; i < N; i++ {
		f[i] = f[i-1] * i % MOD
		g[i] = qmi(f[i], MOD-2, MOD)
	}
}

func comb(n, k int) int {
	return f[n] * g[k] % MOD * g[n-k] % MOD
}

func countVisiblePeople(n int, pos int, k int) int {
	l, r := pos, n-pos-1
	ans := 0

	for a := 0; a <= min(k, l); a++ {
		b := k - a
		if b <= r {
			ans = (ans + 2*comb(l, a)%MOD*comb(r, b)%MOD) % MOD
		}
	}
	return ans
}
```

#### TypeScript

```ts
const N = 100001;
const MOD = 1000000007n;

const f: bigint[] = Array(N).fill(0n);
const g: bigint[] = Array(N).fill(0n);

function qmi(a: bigint, k: bigint, p: bigint): bigint {
    let res = 1n;
    while (k > 0n) {
        if (k & 1n) res = (res * a) % p;
        k >>= 1n;
        a = (a * a) % p;
    }
    return res;
}

f[0] = 1n;
g[0] = 1n;
for (let i = 1; i < N; i++) {
    f[i] = (f[i - 1] * BigInt(i)) % MOD;
    g[i] = qmi(f[i], MOD - 2n, MOD);
}

function comb(n: number, k: number): bigint {
    return (((f[n] * g[k]) % MOD) * g[n - k]) % MOD;
}

function countVisiblePeople(n: number, pos: number, k: number): number {
    const l = pos,
        r = n - pos - 1;
    let ans = 0n;

    for (let a = 0; a <= Math.min(k, l); a++) {
        const b = k - a;
        if (b <= r) {
            ans = (ans + ((((2n * comb(l, a)) % MOD) * comb(r, b)) % MOD)) % MOD;
        }
    }

    return Number(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
