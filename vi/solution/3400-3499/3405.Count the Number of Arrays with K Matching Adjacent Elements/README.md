---
comments: true
difficulty: Hard
rating: 2309
source: Weekly Contest 430 Q4
tags:
    - Math
    - Combinatorics
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [3405. Count the Number of Arrays with K Matching Adjacent Elements](https://leetcode.com/problems/count-the-number-of-arrays-with-k-matching-adjacent-elements)

[中文文档](/solution/3400-3499/3405.Count%20the%20Number%20of%20Arrays%20with%20K%20Matching%20Adjacent%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code>n</code>, <code>m</code>, <code>k</code>. Một <strong>mảng tốt</strong> <code>arr</code> có kích thước <code>n</code> được định nghĩa như sau:</p>

<ul>
    <li>Mỗi phần tử trong <code>arr</code> nằm trong đoạn <strong>bao gồm cả hai đầu mút</strong> <code>[1, m]</code>.</li>
    <li>Có <em>chính xác</em> <code>k</code> chỉ số <code>i</code> (với <code>1 &lt;= i &lt; n</code>) thỏa mãn điều kiện <code>arr[i - 1] == arr[i]</code>.</li>
</ul>

<p>Hãy trả về số lượng <strong>mảng tốt</strong> có thể tạo được.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo </strong><code>10<sup>9 </sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, m = 2, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Có 4 mảng tốt. Đó là <code>[1, 1, 2]</code>, <code>[1, 2, 2]</code>, <code>[2, 1, 1]</code> và <code>[2, 2, 1]</code>.</li>
    <li>Do đó, đáp án là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, m = 2, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các mảng tốt là <code>[1, 1, 1, 2]</code>, <code>[1, 1, 2, 2]</code>, <code>[1, 2, 2, 2]</code>, <code>[2, 1, 1, 1]</code>, <code>[2, 2, 1, 1]</code> và <code>[2, 2, 2, 1]</code>.</li>
    <li>Do đó, đáp án là 6.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, m = 2, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các mảng tốt là <code>[1, 2, 1, 2, 1]</code> và <code>[2, 1, 2, 1, 2]</code>. Do đó, đáp án là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= m &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= k &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổ hợp + Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Với $n,m\le 10^5$, ta không thể liệt kê tất cả các mảng. Một mảng độ dài $n$ có $n-1$ cặp phần tử kề nhau; chính xác $k$ cặp trong số đó bằng nhau và các cặp còn lại phải khác nhau.
>
> Các cặp bằng nhau chia mảng thành $n-k$ đoạn hằng. Chọn $k$ trong $n-1$ vị trí nối không bị cắt có $C_{n-1}^{k}$ cách.
>
> Đoạn đầu tiên có $m$ lựa chọn; mỗi đoạn tiếp theo chỉ cần khác đoạn trước, nên có $m-1$ lựa chọn. Đáp án là $C_{n-1}^{k}\cdot m\cdot(m-1)^{n-k-1}$. Sau khi lập các bảng giai thừa và nghịch đảo, ta chỉ cần tính tổ hợp và lũy thừa modulo.

<!-- thinking:end -->

Với một mảng độ dài $n$, có $n - 1$ cặp phần tử kề nhau. Ta cần chọn $k$ trong số $n - 1$ cặp kề nhau này sao cho hai phần tử của mỗi cặp trong số $k$ cặp này bằng nhau, còn $n - 1 - k$ cặp kề nhau còn lại có hai phần tử khác nhau.

Điều này tương đương với việc chia mảng $n - 1 - k$ lần, tạo thành $n - k$ đoạn mà mọi phần tử trong cùng một đoạn đều bằng nhau. Số cách chia là $C_{n - 1}^{n - 1 - k} = C_{n - 1}^{k}$.

Với đoạn đầu tiên, ta có thể chọn bất kỳ phần tử nào trong $[1, m]$. Với $n - k - 1$ đoạn còn lại, ta chỉ cần đảm bảo phần tử trong mỗi đoạn khác với đoạn trước đó, nên mỗi đoạn có $m - 1$ lựa chọn. Tổng cộng có $m \times (m - 1)^{n - k - 1}$ cách chọn.

Kết hợp hai phần trên, ta có đáp án:

$$
C_{n - 1}^{k} \times m \times (m - 1)^{n - k - 1} \bmod (10^9 + 7)
$$

Trong phần cài đặt, ta có thể tính trước các giai thừa và nghịch đảo, đồng thời dùng lũy thừa nhanh để tính tổ hợp.

Bỏ qua thời gian và không gian tiền xử lý, độ phức tạp thời gian là $O(\log (n - k))$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
mx = 10**5 + 10
mod = 10**9 + 7
f = [1] + [0] * mx
g = [1] + [0] * mx

for i in range(1, mx):
    f[i] = f[i - 1] * i % mod
    g[i] = pow(f[i], mod - 2, mod)


def comb(m: int, n: int) -> int:
    return f[m] * g[n] * g[m - n] % mod


class Solution:
    def countGoodArrays(self, n: int, m: int, k: int) -> int:
        return comb(n - 1, k) * m * pow(m - 1, n - k - 1, mod) % mod
```

#### Java

```java
class Solution {
    private static final int N = (int) 1e5 + 10;
    private static final int MOD = (int) 1e9 + 7;
    private static final long[] f = new long[N];
    private static final long[] g = new long[N];

    static {
        f[0] = 1;
        g[0] = 1;
        for (int i = 1; i < N; ++i) {
            f[i] = f[i - 1] * i % MOD;
            g[i] = qpow(f[i], MOD - 2);
        }
    }

    public static long qpow(long a, int k) {
        long res = 1;
        while (k != 0) {
            if ((k & 1) == 1) {
                res = res * a % MOD;
            }
            k >>= 1;
            a = a * a % MOD;
        }
        return res;
    }

    public static long comb(int m, int n) {
        return (int) f[m] * g[n] % MOD * g[m - n] % MOD;
    }

    public int countGoodArrays(int n, int m, int k) {
        return (int) (comb(n - 1, k) * m % MOD * qpow(m - 1, n - k - 1) % MOD);
    }
}
```

#### C++

```cpp
const int MX = 1e5 + 10;
const int MOD = 1e9 + 7;
long long f[MX];
long long g[MX];

long long qpow(long a, int k) {
    long res = 1;
    while (k != 0) {
        if ((k & 1) == 1) {
            res = res * a % MOD;
        }
        k >>= 1;
        a = a * a % MOD;
    }
    return res;
}

int init = []() {
    f[0] = g[0] = 1;
    for (int i = 1; i < MX; ++i) {
        f[i] = f[i - 1] * i % MOD;
        g[i] = qpow(f[i], MOD - 2);
    }
    return 0;
}();

long long comb(int m, int n) {
    return f[m] * g[n] % MOD * g[m - n] % MOD;
}

class Solution {
public:
    int countGoodArrays(int n, int m, int k) {
        return comb(n - 1, k) * m % MOD * qpow(m - 1, n - k - 1) % MOD;
    }
};
```

#### Go

```go
const MX = 1e5 + 10
const MOD = 1e9 + 7

var f [MX]int64
var g [MX]int64

func qpow(a int64, k int) int64 {
    res := int64(1)
    for k != 0 {
        if k&1 == 1 {
            res = res * a % MOD
        }
        a = a * a % MOD
        k >>= 1
    }
    return res
}

func init() {
    f[0], g[0] = 1, 1
    for i := 1; i < MX; i++ {
        f[i] = f[i-1] * int64(i) % MOD
        g[i] = qpow(f[i], MOD-2)
    }
}

func comb(m, n int) int64 {
    return f[m] * g[n] % MOD * g[m-n] % MOD
}

func countGoodArrays(n int, m int, k int) int {
    ans := comb(n-1, k) * int64(m) % MOD * qpow(int64(m-1), n-k-1) % MOD
    return int(ans)
}
```

#### TypeScript

```ts
const MX = 1e5 + 10;
const MOD = BigInt(1e9 + 7);

const f: bigint[] = Array(MX).fill(1n);
const g: bigint[] = Array(MX).fill(1n);

function qpow(a: bigint, k: number): bigint {
    let res = 1n;
    while (k !== 0) {
        if ((k & 1) === 1) {
            res = (res * a) % MOD;
        }
        a = (a * a) % MOD;
        k >>= 1;
    }
    return res;
}

(function init() {
    for (let i = 1; i < MX; ++i) {
        f[i] = (f[i - 1] * BigInt(i)) % MOD;
        g[i] = qpow(f[i], Number(MOD - 2n));
    }
})();

function comb(m: number, n: number): bigint {
    return (((f[m] * g[n]) % MOD) * g[m - n]) % MOD;
}

export function countGoodArrays(n: number, m: number, k: number): number {
    const ans = (((comb(n - 1, k) * BigInt(m)) % MOD) * qpow(BigInt(m - 1), n - k - 1)) % MOD;
    return Number(ans);
}
```

#### Rust

```rust
impl Solution {
    pub fn count_good_arrays(n: i32, m: i32, k: i32) -> i32 {
        const N: usize = 1e5 as usize + 10;
        const MOD: i64 = 1_000_000_007;
        use std::sync::OnceLock;

        static F: OnceLock<Vec<i64>> = OnceLock::new();
        static G: OnceLock<Vec<i64>> = OnceLock::new();

        fn qpow(mut a: i64, mut k: i64, m: i64) -> i64 {
            let mut res = 1;
            while k != 0 {
                if k & 1 == 1 {
                    res = res * a % m;
                }
                a = a * a % m;
                k >>= 1;
            }
            res
        }

        fn init() -> (&'static Vec<i64>, &'static Vec<i64>) {
            F.get_or_init(|| {
                let mut f = vec![1i64; N];
                for i in 1..N {
                    f[i] = f[i - 1] * i as i64 % MOD;
                }
                f
            });

            G.get_or_init(|| {
                let f = F.get().unwrap();
                let mut g = vec![1i64; N];
                for i in 1..N {
                    g[i] = qpow(f[i], MOD - 2, MOD);
                }
                g
            });

            (F.get().unwrap(), G.get().unwrap())
        }

        fn comb(f: &[i64], g: &[i64], m: usize, n: usize) -> i64 {
            f[m] * g[n] % MOD * g[m - n] % MOD
        }

        let (f, g) = init();
        let n = n as usize;
        let m = m as i64;
        let k = k as usize;

        let c = comb(f, g, n - 1, k);
        let pow = qpow(m - 1, (n - 1 - k) as i64, MOD);
        (c * m % MOD * pow % MOD) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
