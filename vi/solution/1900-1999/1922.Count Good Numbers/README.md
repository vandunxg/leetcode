---
comments: true
difficulty: Medium
rating: 1674
source: Weekly Contest 248 Q3
tags:
    - Recursion
    - Math
---

<!-- problem:start -->

# [1922. Count Good Numbers](https://leetcode.com/problems/count-good-numbers)

[中文文档](/solution/1900-1999/1922.Count%20Good%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi chữ số được gọi là <strong>good</strong> nếu các chữ số <strong>(đánh chỉ số từ 0)</strong> tại các chỉ số <strong>chẵn</strong> là <strong>số chẵn</strong>, còn các chữ số tại các chỉ số <strong>lẻ</strong> là <strong>số nguyên tố</strong> (<code>2</code>, <code>3</code>, <code>5</code> hoặc <code>7</code>).</p>

<ul>
	<li>Ví dụ, <code>&quot;2582&quot;</code> là good vì các chữ số (<code>2</code> và <code>8</code>) ở vị trí chẵn là số chẵn, còn các chữ số (<code>5</code> và <code>2</code>) ở vị trí lẻ là số nguyên tố. Tuy nhiên, <code>&quot;3245&quot;</code> <strong>không</strong> phải là good vì <code>3</code> nằm ở chỉ số chẵn nhưng không phải là số chẵn.</li>
</ul>

<p>Cho một số nguyên <code>n</code>, hãy trả về <em><strong>tổng số</strong> chuỗi chữ số good có độ dài </em><code>n</code>. Vì đáp án có thể rất lớn, hãy <strong>trả về kết quả chia lấy dư cho </strong><code>10<sup>9</sup> + 7</code>.</p>

<p>Một <strong>chuỗi chữ số</strong> là chuỗi gồm các chữ số từ <code>0</code> đến <code>9</code> và có thể chứa các số 0 ở đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các số good có độ dài 1 là &quot;0&quot;, &quot;2&quot;, &quot;4&quot;, &quot;6&quot;, &quot;8&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> 400
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 50
<strong>Đầu ra:</strong> 564908303
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Các chỉ số chẵn có $5$ lựa chọn và các chỉ số lẻ có $4$ lựa chọn, độc lập với nhau. Không thể lặp $n\le 10^{15}$ lần.
>
> Số lượng cần tìm là $5^{\lceil n/2\rceil}\cdot 4^{\lfloor n/2\rfloor}$, được tính bằng lũy thừa nhanh modulo trong $O(\log n)$.

<!-- thinking:end -->

Với một "good number" có độ dài $n$, có $\lceil \frac{n}{2} \rceil = \lfloor \frac{n + 1}{2} \rfloor$ vị trí có chỉ số chẵn, và mỗi vị trí có thể được điền bằng $5$ chữ số khác nhau ($0, 2, 4, 6, 8$). Có $\lfloor \frac{n}{2} \rfloor$ vị trí có chỉ số lẻ, và mỗi vị trí có thể được điền bằng $4$ chữ số khác nhau ($2, 3, 5, 7$). Do đó, tổng số "good numbers" có độ dài $n$ là:

$$
ans = 5^{\lceil \frac{n}{2} \rceil} \times 4^{\lfloor \frac{n}{2} \rfloor}
$$

Ta có thể dùng lũy thừa nhanh để tính $5^{\lceil \frac{n}{2} \rceil}$ và $4^{\lfloor \frac{n}{2} \rfloor}$. Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodNumbers(self, n: int) -> int:
        mod = 10**9 + 7
        return pow(5, (n + 1) >> 1, mod) * pow(4, n >> 1, mod) % mod
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;

    public int countGoodNumbers(long n) {
        return (int) (qpow(5, (n + 1) >> 1) * qpow(4, n >> 1) % mod);
    }

    private long qpow(long x, long n) {
        long res = 1;
        while (n != 0) {
            if ((n & 1) == 1) {
                res = res * x % mod;
            }
            x = x * x % mod;
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
    int countGoodNumbers(long long n) {
        const int mod = 1e9 + 7;
        auto qpow = [](long long x, long long n) -> long long {
            long long res = 1;
            while (n) {
                if ((n & 1) == 1) {
                    res = res * x % mod;
                }
                x = x * x % mod;
                n >>= 1;
            }
            return res;
        };
        return qpow(5, (n + 1) >> 1) * qpow(4, n >> 1) % mod;
    }
};
```

#### Go

```go
const mod int64 = 1e9 + 7

func countGoodNumbers(n int64) int {
	return int(myPow(5, (n+1)>>1) * myPow(4, n>>1) % mod)
}

func myPow(x, n int64) int64 {
	var res int64 = 1
	for n != 0 {
		if (n & 1) == 1 {
			res = res * x % mod
		}
		x = x * x % mod
		n >>= 1
	}
	return res
}
```

#### TypeScript

```ts
function countGoodNumbers(n: number): number {
    const mod = 1000000007n;
    const qpow = (x: bigint, n: bigint): bigint => {
        let res = 1n;
        while (n > 0n) {
            if (n & 1n) {
                res = (res * x) % mod;
            }
            x = (x * x) % mod;
            n >>= 1n;
        }
        return res;
    };
    const a = qpow(5n, BigInt(n + 1) / 2n);
    const b = qpow(4n, BigInt(n) / 2n);
    return Number((a * b) % mod);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
