---
comments: true
difficulty: Hard
rating: 2070
source: Weekly Contest 234 Q4
tags:
    - Recursion
    - Math
    - Number Theory
---

<!-- problem:start -->

# [1808. Maximize Number of Nice Divisors](https://leetcode.com/problems/maximize-number-of-nice-divisors)

[中文文档](/solution/1800-1899/1808.Maximize%20Number%20of%20Nice%20Divisors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>primeFactors</code>. Hãy xây dựng một số nguyên dương <code>n</code> thỏa mãn các điều kiện sau:</p>

<ul>
  <li>Số lượng thừa số nguyên tố của <code>n</code> (không nhất thiết phân biệt) <strong>không vượt quá</strong> <code>primeFactors</code>.</li>
  <li>Số lượng ước tốt của <code>n</code> được tối đa hóa. Lưu ý rằng một ước của <code>n</code> là <strong>ước tốt</strong> nếu nó chia hết cho mọi thừa số nguyên tố của <code>n</code>. Ví dụ, nếu <code>n = 12</code>, các thừa số nguyên tố là <code>[2,2,3]</code>, khi đó <code>6</code> và <code>12</code> là các ước tốt, còn <code>3</code> và <code>4</code> thì không.</li>
</ul>

<p>Hãy trả về <em>số lượng ước tốt của</em> <code>n</code>. Vì số này có thể quá lớn, hãy trả về nó <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Lưu ý rằng số nguyên tố là một số tự nhiên lớn hơn <code>1</code> và không phải là tích của hai số tự nhiên nhỏ hơn. Các thừa số nguyên tố của một số <code>n</code> là danh sách các số nguyên tố có tích bằng <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> primeFactors = 5
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> 200 là một giá trị hợp lệ của n.
Nó có 5 thừa số nguyên tố: [2,2,2,5,5], và có 6 ước tốt: [10,20,40,50,100,200].
Không có giá trị nào khác của n có nhiều hơn 6 ước tốt với nhiều nhất 5 thừa số nguyên tố.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> primeFactors = 8
<strong>Đầu ra:</strong> 18
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= primeFactors &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Biến đổi bài toán + Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Một ước tốt phải chứa mỗi thừa số nguyên tố ít nhất một lần, nên số lượng ước bằng tích các số mũ, với tổng không vượt quá $\textit{primeFactors}$. Việc liệt kê các cách phân hoạch tăng quá nhanh.
>
> Bài toán trở thành chia một số nguyên thành các phần dương sao cho tích lớn nhất. Cách chia kinh điển dùng càng nhiều số $3$ càng tốt và tránh phần dư $1$ (thay $3+1$ bằng $2+2$). Trả về chính $n$ khi $n<4$; nếu không, xét $n\bmod 3$ và tính lũy thừa của $3$ bằng lũy thừa nhanh theo modulo $10^9+7$.

<!-- thinking:end -->

Ta phân tích $n$ thành các thừa số nguyên tố, tức là $n = a_1^{k_1} \times a_2^{k_2} \times\cdots \times a_m^{k_m}$, trong đó $a_i$ là một thừa số nguyên tố và $k_i$ là số mũ của thừa số nguyên tố $a_i$. Vì số lượng thừa số nguyên tố của $n$ không vượt quá `primeFactors`, nên $k_1 + k_2 + \cdots + k_m \leq primeFactors$.

Theo mô tả bài toán, ta biết một ước tốt của $n$ phải chia hết cho mọi thừa số nguyên tố, nghĩa là một ước tốt của $n$ phải chứa $a_1 \times a_2 \times \cdots \times a_m$ làm thừa số. Khi đó số lượng ước tốt $k= k_1 \times k_2 \times \cdots \times k_m$, tức là $k$ là tích của $k_1, k_2, \cdots, k_m$. Để tối đa hóa số lượng ước tốt, ta cần chia `primeFactors` thành $k_1, k_2, \cdots, k_m$ sao cho $k_1 \times k_2 \times \cdots \times k_m$ lớn nhất. Vì vậy, bài toán được chuyển thành: chia số nguyên `primeFactors` thành tích của một số số nguyên để tối đa hóa tích đó.

Tiếp theo, ta chỉ cần xét các trường hợp khác nhau.

- Nếu $primeFactors \lt 4$, trả về trực tiếp `primeFactors`.
- Nếu $primeFactors$ là bội của $3$, ta chia `primeFactors` thành các phần bằng $3$, tức là $3^{\frac{primeFactors}{3}}$.
- Nếu $primeFactors$ chia cho $3$ dư $1$, ta chia `primeFactors` thành $\frac{primeFactors}{3} - 1$ phần bằng $3$, rồi nhân với $4$, tức là $3^{\frac{primeFactors}{3} - 1} \times 4$.
- Nếu $primeFactors$ chia cho $3$ dư $2$, ta chia `primeFactors` thành $\frac{primeFactors}{3}$ phần bằng $3$, rồi nhân với $2$, tức là $3^{\frac{primeFactors}{3}} \times 2$.

Trong quá trình trên, ta dùng lũy thừa nhanh để tính modulo.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxNiceDivisors(self, primeFactors: int) -> int:
        mod = 10**9 + 7
        if primeFactors < 4:
            return primeFactors
        if primeFactors % 3 == 0:
            return pow(3, primeFactors // 3, mod) % mod
        if primeFactors % 3 == 1:
            return 4 * pow(3, primeFactors // 3 - 1, mod) % mod
        return 2 * pow(3, primeFactors // 3, mod) % mod
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;

    public int maxNiceDivisors(int primeFactors) {
        if (primeFactors < 4) {
            return primeFactors;
        }
        if (primeFactors % 3 == 0) {
            return qpow(3, primeFactors / 3);
        }
        if (primeFactors % 3 == 1) {
            return (int) (4L * qpow(3, primeFactors / 3 - 1) % mod);
        }
        return 2 * qpow(3, primeFactors / 3) % mod;
    }

    private int qpow(long a, long n) {
        long ans = 1;
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
    int maxNiceDivisors(int primeFactors) {
        if (primeFactors < 4) {
            return primeFactors;
        }
        const int mod = 1e9 + 7;
        auto qpow = [&](long long a, long long n) {
            long long ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return (int) ans;
        };
        if (primeFactors % 3 == 0) {
            return qpow(3, primeFactors / 3);
        }
        if (primeFactors % 3 == 1) {
            return qpow(3, primeFactors / 3 - 1) * 4L % mod;
        }
        return qpow(3, primeFactors / 3) * 2 % mod;
    }
};
```

#### Go

```go
func maxNiceDivisors(primeFactors int) int {
	if primeFactors < 4 {
		return primeFactors
	}
	const mod = 1e9 + 7
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
	if primeFactors%3 == 0 {
		return qpow(3, primeFactors/3)
	}
	if primeFactors%3 == 1 {
		return qpow(3, primeFactors/3-1) * 4 % mod
	}
	return qpow(3, primeFactors/3) * 2 % mod
}
```

#### JavaScript

```js
/**
 * @param {number} primeFactors
 * @return {number}
 */
var maxNiceDivisors = function (primeFactors) {
    if (primeFactors < 4) {
        return primeFactors;
    }
    const mod = 1e9 + 7;
    const qpow = (a, n) => {
        let ans = 1;
        for (; n; n >>= 1) {
            if (n & 1) {
                ans = Number((BigInt(ans) * BigInt(a)) % BigInt(mod));
            }
            a = Number((BigInt(a) * BigInt(a)) % BigInt(mod));
        }
        return ans;
    };
    const k = Math.floor(primeFactors / 3);
    if (primeFactors % 3 === 0) {
        return qpow(3, k);
    }
    if (primeFactors % 3 === 1) {
        return (4 * qpow(3, k - 1)) % mod;
    }
    return (2 * qpow(3, k)) % mod;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
