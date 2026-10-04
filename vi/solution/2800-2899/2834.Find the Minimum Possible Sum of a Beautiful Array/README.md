---
comments: true
difficulty: Medium
rating: 1409
source: Weekly Contest 360 Q2
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [2834. Find the Minimum Possible Sum of a Beautiful Array](https://leetcode.com/problems/find-the-minimum-possible-sum-of-a-beautiful-array)

[中文文档](/solution/2800-2899/2834.Find%20the%20Minimum%20Possible%20Sum%20of%20a%20Beautiful%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>n</code> và <code>target</code>.</p>

<p>Một mảng <code>nums</code> được gọi là <strong>đẹp</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>nums.length == n</code>.</li>
	<li><code>nums</code> gồm các số nguyên <strong>đôi một phân biệt</strong> <strong>dương</strong>.</li>
	<li>Không tồn tại hai chỉ số <strong>phân biệt</strong> <code>i</code> và <code>j</code> trong phạm vi <code>[0, n - 1]</code> sao cho <code>nums[i] + nums[j] == target</code>.</li>
</ul>

<p>Trả về <em><strong>tổng nhỏ nhất</strong> có thể có của một mảng đẹp modulo </em><code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, target = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta thấy nums = [1,3] là một mảng đẹp.
- Mảng nums có độ dài n = 2.
- Mảng nums gồm các số nguyên dương đôi một phân biệt.
- Không tồn tại hai chỉ số phân biệt i và j sao cho nums[i] + nums[j] == 3.
Có thể chứng minh rằng 4 là tổng nhỏ nhất có thể có của một mảng đẹp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, target = 3
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Ta thấy nums = [1,3,4] là một mảng đẹp.
- Mảng nums có độ dài n = 3.
- Mảng nums gồm các số nguyên dương đôi một phân biệt.
- Không tồn tại hai chỉ số phân biệt i và j sao cho nums[i] + nums[j] == 3.
Có thể chứng minh rằng 8 là tổng nhỏ nhất có thể có của một mảng đẹp.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, target = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta thấy nums = [1] là một mảng đẹp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= target &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mảng đẹp không cho phép bất kỳ cặp nào có tổng bằng $target$ và phải có tổng nhỏ nhất. Chọn tham lam bắt đầu từ $1$ có thể lấy mọi số nguyên đến $\lfloor target/2\rfloor$; nếu cần thêm phần tử, ta tiếp tục từ $target$. Cả hai đoạn đều là cấp số cộng, nên chỉ cần dùng công thức tổng quát.

<!-- thinking:end -->

Ta có thể xây dựng mảng `nums` một cách tham lam, bắt đầu từ $x = 1$, mỗi lần chọn $x$ và loại trừ $target - x$.

Đặt $m = \left\lfloor \frac{target}{2} \right\rfloor$.

Nếu $x <= m$, các số ta có thể chọn là $1, 2, \cdots, n$, nên tổng của mảng là $\left\lfloor \frac{(1+n)n}{2} \right\rfloor$.

Nếu $x > m$, các số ta có thể chọn là $1, 2, \cdots, m$, tổng cộng $m$ số, và $n - m$ số bắt đầu từ $target$, nên tổng của mảng là $\left\lfloor \frac{(1+m)m}{2} \right\rfloor + \left\lfloor \frac{(target + target + n - m - 1)(n-m)}{2} \right\rfloor$.

Lưu ý rằng cần lấy phần dư của kết quả theo $10^9 + 7$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumPossibleSum(self, n: int, target: int) -> int:
        mod = 10**9 + 7
        m = target // 2
        if n <= m:
            return ((1 + n) * n // 2) % mod
        return ((1 + m) * m // 2 + (target + target + n - m - 1) * (n - m) // 2) % mod
```

#### Java

```java
class Solution {
    public int minimumPossibleSum(int n, int target) {
        final int mod = (int) 1e9 + 7;
        int m = target / 2;
        if (n <= m) {
            return (int) ((1L + n) * n / 2 % mod);
        }
        long a = (1L + m) * m / 2 % mod;
        long b = ((1L * target + target + n - m - 1) * (n - m) / 2) % mod;
        return (int) ((a + b) % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumPossibleSum(int n, int target) {
        const int mod = 1e9 + 7;
        int m = target / 2;
        if (n <= m) {
            return (1LL + n) * n / 2 % mod;
        }
        long long a = (1LL + m) * m / 2 % mod;
        long long b = (1LL * target + target + n - m - 1) * (n - m) / 2 % mod;
        return (a + b) % mod;
    }
};
```

#### Go

```go
func minimumPossibleSum(n int, target int) int {
	const mod int = 1e9 + 7
	m := target / 2
	if n <= m {
		return (n + 1) * n / 2 % mod
	}
	a := (m + 1) * m / 2 % mod
	b := (target + target + n - m - 1) * (n - m) / 2 % mod
	return (a + b) % mod
}
```

#### TypeScript

```ts
function minimumPossibleSum(n: number, target: number): number {
    const mod = 10 ** 9 + 7;
    const m = target >> 1;
    if (n <= m) {
        return (((1 + n) * n) / 2) % mod;
    }
    return (((1 + m) * m) / 2 + ((target + target + n - m - 1) * (n - m)) / 2) % mod;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_possible_sum(n: i32, target: i32) -> i32 {
        const MOD: i64 = 1_000_000_007;
        let n = n as i64;
        let target = target as i64;
        let m = target / 2;
        if n <= m {
            return ((1 + n) * n / 2 % MOD) as i32;
        }
        let a = (1 + m) * m / 2 % MOD;
        let b = (target + target + n - m - 1) * (n - m) / 2 % MOD;
        ((a + b) % MOD) as i32
    }
}
```

#### C#

```cs
public class Solution {
    public int MinimumPossibleSum(int n, int target) {
        const int mod = (int) 1e9 + 7;
        int m = target / 2;
        if (n <= m) {
            return (int) ((1L + n) * n / 2 % mod);
        }
        long a = (1L + m) * m / 2 % mod;
        long b = ((1L * target + target + n - m - 1) * (n - m) / 2) % mod;
        return (int) ((a + b) % mod);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
