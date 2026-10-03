---
comments: true
difficulty: Medium
rating: 1966
source: Weekly Contest 254 Q3
tags:
    - Greedy
    - Recursion
    - Math
---

<!-- problem:start -->

# [1969. Minimum Non-Zero Product of the Array Elements](https://leetcode.com/problems/minimum-non-zero-product-of-the-array-elements)

[中文文档](/solution/1900-1999/1969.Minimum%20Non-Zero%20Product%20of%20the%20Array%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>p</code>. Xét một mảng <code>nums</code> (<strong>đánh chỉ số từ 1</strong>) gồm các số nguyên trong đoạn <strong>bao gồm cả hai đầu</strong> <code>[1, 2<sup>p</sup> - 1]</code> dưới dạng biểu diễn nhị phân. Bạn có thể thực hiện thao tác sau <strong>bao nhiêu lần tùy ý</strong>:</p>

<ul>
	<li>Chọn hai phần tử <code>x</code> và <code>y</code> từ <code>nums</code>.</li>
	<li>Chọn một bit trong <code>x</code> và đổi chỗ bit đó với bit tương ứng trong <code>y</code>. Bit tương ứng là bit ở <strong>cùng vị trí</strong> trong số nguyên còn lại.</li>
</ul>

<p>Ví dụ, nếu <code>x = 11<u>0</u>1</code> và <code>y = 00<u>1</u>1</code>, sau khi đổi chỗ bit <code>2<sup>nd</sup></code> tính từ bên phải, ta có <code>x = 11<u>1</u>1</code> và <code>y = 00<u>0</u>1</code>.</p>

<p>Hãy tìm tích <strong>khác không nhỏ nhất</strong> của <code>nums</code> sau khi thực hiện thao tác trên <strong>bao nhiêu lần tùy ý</strong>. Trả về <em>tích này</em><em> <strong>lấy modulo</strong> </em><code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý:</strong> Đáp án phải là tích nhỏ nhất <strong>trước</strong> khi thực hiện phép modulo.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> p = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> nums = [1].
Chỉ có một phần tử nên tích bằng chính phần tử đó.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> p = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> nums = [01, 10, 11].
Bất kỳ phép đổi chỗ nào cũng sẽ tạo ra tích bằng 0 hoặc giữ nguyên tích.
Vì vậy, tích mảng 1 * 2 * 3 = 6 đã là nhỏ nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> p = 3
<strong>Đầu ra:</strong> 1512
<strong>Giải thích:</strong> nums = [001, 010, 011, 100, 101, 110, 111]
- Trong thao tác đầu tiên, ta có thể đổi chỗ bit ngoài cùng bên trái của phần tử thứ hai và thứ năm.
    - Mảng kết quả là [001, <u>1</u>10, 011, 100, <u>0</u>01, 110, 111].
- Trong thao tác thứ hai, ta có thể đổi chỗ bit ở giữa của phần tử thứ ba và thứ tư.
    - Mảng kết quả là [001, 110, 0<u>0</u>1, 1<u>1</u>0, 001, 110, 111].
Tích của mảng là 1 * 6 * 1 * 6 * 1 * 6 * 7 = 1512, đây là tích nhỏ nhất có thể.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= p &lt;= 60</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể đổi chỗ các bit giữa hai số mà vẫn giữ nguyên tổng. Với một tổng cố định, tích được tối thiểu khi các giá trị được phân cực nhiều nhất có thể mà không tạo ra số 0.
>
> Giữ nguyên $2^p-1$ và ghép các phần tử còn lại thành các cặp $(1,2^p-2)$, lặp lại $2^{p-1}-1$ lần. Tích là $(2^p-1)(2^p-2)^{2^{p-1}-1}$, được tính bằng lũy thừa nhanh theo modulo.

<!-- thinking:end -->

Ta nhận thấy mỗi thao tác không làm thay đổi tổng các phần tử. Khi tổng các phần tử không đổi, để tối thiểu hóa tích, ta nên làm cho hiệu giữa các phần tử lớn nhất có thể.

Vì phần tử lớn nhất là $2^p - 1$, dù đổi chỗ với phần tử nào thì nó cũng không làm tăng hiệu. Do đó, ta không cần xét trường hợp đổi chỗ với phần tử lớn nhất.

Với các phần tử còn lại trong $[1,..2^p-2]$, ta lần lượt ghép phần tử đầu tiên với phần tử cuối cùng, tức là ghép $x$ với $2^p-1-x$. Sau một số thao tác, mỗi cặp phần tử trở thành $(1, 2^p-2)$. Tích cuối cùng là $(2^p-1) \times (2^p-2)^{2^{p-1}-1}$.

Độ phức tạp thời gian là $O(p)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minNonZeroProduct(self, p: int) -> int:
        mod = 10**9 + 7
        return (2**p - 1) * pow(2**p - 2, 2 ** (p - 1) - 1, mod) % mod
```

#### Java

```java
class Solution {
    public int minNonZeroProduct(int p) {
        final int mod = (int) 1e9 + 7;
        long a = ((1L << p) - 1) % mod;
        long b = qpow(((1L << p) - 2) % mod, (1L << (p - 1)) - 1, mod);
        return (int) (a * b % mod);
    }

    private long qpow(long a, long n, int mod) {
        long ans = 1;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNonZeroProduct(int p) {
        using ll = long long;
        const int mod = 1e9 + 7;
        auto qpow = [](ll a, ll n) {
            ll ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return ans;
        };
        ll a = ((1LL << p) - 1) % mod;
        ll b = qpow(((1LL << p) - 2) % mod, (1L << (p - 1)) - 1);
        return a * b % mod;
    }
};
```

#### Go

```go
func minNonZeroProduct(p int) int {
	const mod int = 1e9 + 7
	qpow := func(a, n int) int {
		ans := 1
		for ; n > 0; n >>= 1 {
			if n&1 == 1 {
				ans = ans * a % mod
			}
			a = a * a % mod
		}
		return ans
	}
	a := ((1 << p) - 1) % mod
	b := qpow(((1<<p)-2)%mod, (1<<(p-1))-1)
	return a * b % mod
}
```

#### TypeScript

```ts
function minNonZeroProduct(p: number): number {
    const mod = BigInt(1e9 + 7);

    const qpow = (a: bigint, n: bigint): bigint => {
        let ans = BigInt(1);
        for (; n; n >>= BigInt(1)) {
            if (n & BigInt(1)) {
                ans = (ans * a) % mod;
            }
            a = (a * a) % mod;
        }
        return ans;
    };
    const a = (2n ** BigInt(p) - 1n) % mod;
    const b = qpow((2n ** BigInt(p) - 2n) % mod, 2n ** (BigInt(p) - 1n) - 1n);
    return Number((a * b) % mod);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
