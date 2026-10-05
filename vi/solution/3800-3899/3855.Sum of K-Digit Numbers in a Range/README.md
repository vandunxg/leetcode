---
comments: true
difficulty: Hard
rating: 2085
source: Biweekly Contest 177 Q4
tags:
    - Math
    - Divide and Conquer
    - Combinatorics
    - Number Theory
---

<!-- problem:start -->

# [3855. Sum of K-Digit Numbers in a Range](https://leetcode.com/problems/sum-of-k-digit-numbers-in-a-range)

[中文文档](/solution/3800-3899/3855.Sum%20of%20K-Digit%20Numbers%20in%20a%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <code>l</code>, <code>r</code> và <code>k</code>.</p>

<p>Xét tất cả các số nguyên gồm <strong>chính xác</strong> <code>k</code> chữ số, trong đó mỗi chữ số được chọn độc lập từ đoạn <code>[l, r]</code> (bao gồm cả hai đầu mút). Nếu 0 thuộc đoạn, các số 0 ở đầu được phép.</p>

<p>Hãy trả về một số nguyên biểu thị <b>tổng của tất cả các số như vậy.</b>​​​​​​​ Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 1, r = 2, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">66</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tất cả các số được tạo bởi <code>k = 2</code> chữ số trong đoạn <code>[1, 2]</code> là <code>11, 12, 21, 22</code>.</li>
	<li>Tổng là <code>11 + 12 + 21 + 22 = 66</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 0, r = 1, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">444</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tất cả các số được tạo bởi <code>k = 3</code> chữ số trong đoạn <code>[0, 1]</code> là <code>000, 001, 010, 011, 100, 101, 110, 111</code>​​​​​​​.</li>
	<li>Các số này khi bỏ các số 0 ở đầu là <code>0, 1, 10, 11, 100, 101, 110, 111</code>.</li>
	<li>Tổng là 444.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 5, r = 5, k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">555555520</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>5555555555 là số hợp lệ duy nhất gồm <code>k = 10</code> chữ số trong đoạn <code>[5, 5]</code>.</li>
	<li>Tổng là <code>5555555555 % (10<sup>9</sup> + 7) = 555555520</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= l &lt;= r &lt;= 9</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Tính tổng tất cả các chuỗi gồm $k$ chữ số, với các chữ số thuộc $[l,r]$ và cho phép số 0 ở đầu. Vì $k \le 10^9$, ta không thể liệt kê chúng.
>
> Các vị trí có tính đối xứng: tại vị trí $10^i$, mỗi chữ số được phép xuất hiện $n^{k-1}$ lần, với $n=r-l+1$.
>
> Tổng các chữ số là $(l+r)n/2$, nhân với nhân tử hình học $(10^k-1)/9$. Lũy thừa nhanh xử lý các số mũ; phép chia cho $9$ dùng nghịch đảo modulo.
>
> Mọi phép tính đều được thực hiện modulo $10^9+7$.

<!-- thinking:end -->

Ta duyệt từng chữ số $x$ từ vị trí thấp nhất đến vị trí cao nhất. Giả sử vị trí hiện tại là chữ số thứ $i$ (đánh số từ 0), đóng góp của nó vào số là $x \cdot 10^i$. $k - 1$ chữ số còn lại, mỗi chữ số có $r - l + 1$ cách chọn, nên đóng góp của vị trí hiện tại là $x \cdot 10^i \cdot (r - l + 1)^{k - 1}$. Vì $x$ chạy trong đoạn $[l, r]$, tổng tất cả các giá trị của $x$ là $\frac{(l + r) \cdot (r - l + 1)}{2}$. Do đó, tổng của tất cả các số như vậy là:

$$
\begin{aligned}
&\sum_{i = 0}^{k - 1} \frac{(l + r) \cdot (r - l + 1)}{2} \cdot (r - l + 1)^{k - 1} \cdot 10^i \\
= &\frac{(l + r) \cdot (r - l + 1)}{2} \cdot (r - l + 1)^{k - 1} \cdot \frac{10^k - 1}{9}
\end{aligned}
$$

Vì $k$ có thể lên đến $10^9$, ta dùng lũy thừa nhanh (lũy thừa nhị phân) để tính $(r - l + 1)^{k - 1}$ và $10^k$. Phép chia cho $9$ được xử lý bằng nghịch đảo modulo của $9$ theo định lý nhỏ Fermat.

Độ phức tạp thời gian là $O(\log k)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfNumbers(self, l: int, r: int, k: int) -> int:
        mod = 10**9 + 7

        n = r - l + 1

        # ((l + r) * (r - l + 1) // 2) % mod
        total = (l + r) * n // 2 % mod

        # pow(r - l + 1, k - 1, mod)
        part1 = pow(n % mod, k - 1, mod)

        # (pow(10, k, mod) - 1)
        part2 = (pow(10, k, mod) - 1) % mod

        # Fermat inverse of 9
        inv9 = pow(9, mod - 2, mod)

        ans = total
        ans = ans * part1 % mod
        ans = ans * part2 % mod
        ans = ans * inv9 % mod

        return ans
```

#### Java

```java
class Solution {
    public int sumOfNumbers(int l, int r, int k) {
        final int mod = 1_000_000_007;

        long n = r - l + 1L;

        // ((l + r) * (r - l + 1) // 2) % mod
        long sum = (long) (l + r) * n / 2 % mod;

        // pow(r - l + 1, k - 1, mod)
        long part1 = qpow(n % mod, k - 1, mod);

        // (pow(10, k, mod) - 1)
        long part2 = (qpow(10, k, mod) - 1 + mod) % mod;

        // pow(9, mod - 2, mod)  (Fermat inverse of 9)
        long inv9 = qpow(9, mod - 2, mod);

        long ans = sum;
        ans = ans * part1 % mod;
        ans = ans * part2 % mod;
        ans = ans * inv9 % mod;

        return (int) ans;
    }

    private int qpow(long a, int n, int mod) {
        long ans = 1;
        a %= mod;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfNumbers(int l, int r, int k) {
        const int mod = 1'000'000'007;

        long long n = 1LL * r - l + 1;

        // ((l + r) * (r - l + 1) / 2) % mod
        long long sum = 1LL * (l + r) * n / 2 % mod;

        // pow(r - l + 1, k - 1, mod)
        long long part1 = qpow(n % mod, k - 1, mod);

        // (pow(10, k, mod) - 1)
        long long part2 = (qpow(10, k, mod) - 1 + mod) % mod;

        // Fermat inverse of 9
        long long inv9 = qpow(9, mod - 2, mod);

        long long ans = sum;
        ans = ans * part1 % mod;
        ans = ans * part2 % mod;
        ans = ans * inv9 % mod;

        return (int) ans;
    }

private:
    long long qpow(long long a, long long n, int mod) {
        long long ans = 1;
        a %= mod;
        while (n > 0) {
            if (n & 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
            n >>= 1;
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfNumbers(l int, r int, k int) int {
	const mod int64 = 1_000_000_007

	n := int64(r - l + 1)

	// ((l + r) * (r - l + 1) / 2) % mod
	sum := int64(l+r) * n / 2 % mod

	// pow(r - l + 1, k - 1, mod)
	part1 := qpow(n%mod, int64(k-1), mod)

	// (pow(10, k, mod) - 1)
	part2 := (qpow(10, int64(k), mod) - 1 + mod) % mod

	// Fermat inverse of 9
	inv9 := qpow(9, mod-2, mod)

	ans := sum
	ans = ans * part1 % mod
	ans = ans * part2 % mod
	ans = ans * inv9 % mod

	return int(ans)
}

func qpow(a int64, n int64, mod int64) int64 {
	a %= mod
	var ans int64 = 1
	for n > 0 {
		if n&1 == 1 {
			ans = ans * a % mod
		}
		a = a * a % mod
		n >>= 1
	}
	return ans
}
```

#### TypeScript

```ts
function sumOfNumbers(l: number, r: number, k: number): number {
    const mod = 1_000_000_007n;

    const n = BigInt(r - l + 1);

    // ((l + r) * (r - l + 1) / 2) % mod
    const sum = ((BigInt(l + r) * n) / 2n) % mod;

    // pow(r - l + 1, k - 1, mod)
    const part1 = qpow(n % mod, BigInt(k - 1), mod);

    // (pow(10, k, mod) - 1)
    const part2 = (qpow(10n, BigInt(k), mod) - 1n + mod) % mod;

    // Fermat inverse of 9
    const inv9 = qpow(9n, mod - 2n, mod);

    let ans = sum;
    ans = (ans * part1) % mod;
    ans = (ans * part2) % mod;
    ans = (ans * inv9) % mod;

    return Number(ans);
}

function qpow(a: bigint, n: bigint, mod: bigint): bigint {
    a %= mod;
    let ans = 1n;
    while (n > 0n) {
        if ((n & 1n) === 1n) {
            ans = (ans * a) % mod;
        }
        a = (a * a) % mod;
        n >>= 1n;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
