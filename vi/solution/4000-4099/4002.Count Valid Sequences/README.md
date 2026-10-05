---
comments: true
difficulty: Medium
rating: 1912
source: Weekly Contest 512 Q3
tags:
    - Math
    - Combinatorics
---

<!-- problem:start -->

# [4002. Count Valid Sequences](https://leetcode.com/problems/count-valid-sequences)

[中文文档](/solution/4000-4099/4002.Count%20Valid%20Sequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <strong>dương</strong> <code>n</code> và <code>k</code>.</p>

<p>Một <strong>chuỗi hợp lệ</strong> là chuỗi gồm <code>k</code> số nguyên dương sao cho:</p>

<ul>
	<li><strong>Tổng</strong> của mọi số trong chuỗi bằng <code>n</code>.</li>
	<li><strong>Tích</strong> của mọi số trong chuỗi là <strong>chẵn</strong>.</li>
</ul>

<p>Hãy trả về số lượng chuỗi hợp lệ. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup>​​​​​​​ + 7</code>.</p>

<p>Hai chuỗi được xem là <strong>khác nhau</strong> nếu chúng khác nhau tại bất kỳ chỉ số nào. Ví dụ, <code>[1, 1, 2]</code> và <code>[1, 2, 1]</code> được xem là hai chuỗi khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi độ dài <code>k = 3</code> có tổng bằng 5 là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Chuỗi</th>
			<th style="border: 1px solid black;">Tích</th>
			<th style="border: 1px solid black;">Tính chẵn lẻ</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1, 1, 3]</code></td>
			<td style="border: 1px solid black;"><code>1 * 1 * 3 = 3</code></td>
			<td style="border: 1px solid black;">Lẻ</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1, 2, 2]</code></td>
			<td style="border: 1px solid black;"><code>1 * 2 * 2 = 4</code></td>
			<td style="border: 1px solid black;">Chẵn</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[2, 1, 2]</code></td>
			<td style="border: 1px solid black;"><code>2 * 1 * 2 = 4</code></td>
			<td style="border: 1px solid black;">Chẵn</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[2, 2, 1]</code></td>
			<td style="border: 1px solid black;"><code>2 * 2 * 1 = 4</code></td>
			<td style="border: 1px solid black;">Chẵn</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1, 3, 1]</code></td>
			<td style="border: 1px solid black;"><code>1 * 3 * 1 = 3</code></td>
			<td style="border: 1px solid black;">Lẻ</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[3, 1, 1]</code></td>
			<td style="border: 1px solid black;"><code>3 * 1 * 1 = 3</code></td>
			<td style="border: 1px solid black;">Lẻ</td>
		</tr>
	</tbody>
</table>

<p>Có 3 chuỗi có tích chẵn, nên đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi độ dài <code>k = 2</code> có tổng bằng 3 là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Chuỗi</th>
			<th style="border: 1px solid black;">Tích</th>
			<th style="border: 1px solid black;">Tính chẵn lẻ</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;"><code>1 * 2 = 2</code></td>
			<td style="border: 1px solid black;">Chẵn</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[2, 1]</code></td>
			<td style="border: 1px solid black;"><code>2 * 1 = 2</code></td>
			<td style="border: 1px solid black;">Chẵn</td>
		</tr>
	</tbody>
</table>

<p>Có 2 chuỗi có tích chẵn, nên đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi duy nhất có thể có độ dài <code>k = 5</code> và tổng bằng 5 là <code>[1, 1, 1, 1, 1]</code>, có tích lẻ. Vì vậy, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổ hợp

<!-- thinking:start -->

> **Tư duy**
>
> Số cách biểu diễn có thứ tự $n$ thành $k$ phần dương là $\binom{n-1}{k-1}$. Tích chẵn khi và chỉ khi có ít nhất một phần chẵn, nên ta lấy tổng số cách trừ đi các cách mà mọi phần đều lẻ.
>
> Các cách mà mọi phần đều lẻ chỉ tồn tại khi $n$ và $k$ cùng tính chẵn lẻ. Đặt mỗi phần là $2a_i+1$ thì $\sum a_i=(n-k)/2$, tương ứng với $\binom{(n+k)/2-1}{k-1}$; nếu không thì số hạng này bằng $0$.
>
> Với $n$ và $k$ có thể lên tới $5\times 10^5$, mỗi hệ số nhị thức được tính bằng giai thừa và giai thừa nghịch đảo đã tiền xử lý. Sau đó, mỗi truy vấn chỉ cần trừ theo modulo $10^9+7$ trong $O(1)$.

<!-- thinking:end -->

Số cách có thứ tự để viết $n$ thành tổng của $k$ số nguyên dương là $\binom{n-1}{k-1}$. Tích chẵn nghĩa là "có ít nhất một số chẵn"; phần bù là "tất cả đều lẻ".

Do đó, đáp án là:

$$
\binom{n-1}{k-1} - \textit{(number of all-odd sequences)}
$$

Nếu mọi số đều lẻ, viết số thứ $i$ là $2a_i + 1$ ($a_i \ge 0$). Khi đó:

$$
\sum_{i=1}^{k}(2a_i + 1) = n \implies \sum_{i=1}^{k} a_i = \frac{n-k}{2}
$$

Các chuỗi mà mọi số đều lẻ chỉ tồn tại khi $n$ và $k$ cùng tính chẵn lẻ (tức $n + k$ là số chẵn), và số lượng là $\binom{\frac{n+k}{2}-1}{k-1}$; nếu không thì số lượng là $0$.

Sau khi tính trước giai thừa và nghịch đảo modulo, mỗi tổ hợp được tính trong $O(1)$. Trả về đáp án theo modulo $10^9+7$.

Độ phức tạp thời gian tiền xử lý là $O(N + \log M)$ và độ phức tạp không gian là $O(N)$, trong đó $N = 5 \times 10^5$ và $M = 10^9+7$. Mỗi truy vấn có độ phức tạp $O(1)$.

<!-- tabs:start -->

#### Python3

```python
MX = 5 * 10**5 + 1
MOD = 10**9 + 7
f = [1] * MX
g = [1] * MX
for i in range(1, MX):
    f[i] = f[i - 1] * i % MOD
    g[i] = pow(f[i], MOD - 2, MOD)


def comb(n: int, k: int) -> int:
    return f[n] * g[k] * g[n - k] % MOD


class Solution:
    def countValidSequences(self, n: int, k: int) -> int:
        ans = comb(n - 1, k - 1)
        if (n + k) % 2 == 0:
            ans = (ans - comb((n + k) // 2 - 1, k - 1)) % MOD
        return ans
```

#### Java

```java
class Solution {
    static final int MX = 500001;
    static final long MOD = 1000000007L;
    static long[] f = new long[MX];
    static long[] g = new long[MX];

    static {
        f[0] = 1;
        g[0] = 1;
        for (int i = 1; i < MX; i++) {
            f[i] = f[i - 1] * i % MOD;
            g[i] = pow(f[i], MOD - 2);
        }
    }

    static long pow(long a, long b) {
        long res = 1;
        while (b > 0) {
            if ((b & 1) == 1) {
                res = res * a % MOD;
            }
            a = a * a % MOD;
            b >>= 1;
        }
        return res;
    }

    static long comb(int n, int k) {
        return f[n] * g[k] % MOD * g[n - k] % MOD;
    }

    public int countValidSequences(int n, int k) {
        long ans = comb(n - 1, k - 1);
        if ((n + k) % 2 == 0) {
            ans = (ans - comb((n + k) / 2 - 1, k - 1) + MOD) % MOD;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
const int MX = 500001;
const long long MOD = 1000000007LL;

long long f[MX];
long long g[MX];

long long qpow(long long a, long long b) {
    long long res = 1;
    while (b > 0) {
        if (b & 1) {
            res = res * a % MOD;
        }
        a = a * a % MOD;
        b >>= 1;
    }
    return res;
}

int init = []() {
    f[0] = 1;
    g[0] = 1;

    for (int i = 1; i < MX; i++) {
        f[i] = f[i - 1] * i % MOD;
        g[i] = qpow(f[i], MOD - 2);
    }

    return 0;
}();

long long comb(int n, int k) {
    return f[n] * g[k] % MOD * g[n - k] % MOD;
}

class Solution {
public:
    int countValidSequences(int n, int k) {
        long long ans = comb(n - 1, k - 1);

        if ((n + k) % 2 == 0) {
            ans = (ans - comb((n + k) / 2 - 1, k - 1) + MOD) % MOD;
        }

        return (int) ans;
    }
};
```

#### Go

```go
const MX = 500001
const MOD int64 = 1000000007

var f [MX]int64
var g [MX]int64

func init() {
	f[0] = 1
	g[0] = 1

	for i := 1; i < MX; i++ {
		f[i] = f[i-1] * int64(i) % MOD
		g[i] = pow(f[i], MOD-2)
	}
}

func pow(a, b int64) int64 {
	res := int64(1)
	for b > 0 {
		if b&1 == 1 {
			res = res * a % MOD
		}
		a = a * a % MOD
		b >>= 1
	}
	return res
}

func comb(n, k int) int64 {
	return f[n] * g[k] % MOD * g[n-k] % MOD
}

func countValidSequences(n int, k int) int {
	ans := comb(n-1, k-1)

	if (n+k)%2 == 0 {
		ans = (ans - comb((n+k)/2-1, k-1) + MOD) % MOD
	}

	return int(ans)
}
```

#### TypeScript

```ts
const MX = 500001;
const MOD = 1000000007n;

const f: bigint[] = new Array(MX).fill(1n);
const g: bigint[] = new Array(MX).fill(1n);

function pow(a: bigint, b: bigint): bigint {
    let res = 1n;
    while (b > 0n) {
        if (b & 1n) {
            res = (res * a) % MOD;
        }
        a = (a * a) % MOD;
        b >>= 1n;
    }
    return res;
}

for (let i = 1; i < MX; i++) {
    f[i] = (f[i - 1] * BigInt(i)) % MOD;
    g[i] = pow(f[i], MOD - 2n);
}

function comb(n: number, k: number): bigint {
    return (((f[n] * g[k]) % MOD) * g[n - k]) % MOD;
}

function countValidSequences(n: number, k: number): number {
    let ans = comb(n - 1, k - 1);

    if ((n + k) % 2 === 0) {
        ans = (ans - comb((n + k) / 2 - 1, k - 1) + MOD) % MOD;
    }

    return Number(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
