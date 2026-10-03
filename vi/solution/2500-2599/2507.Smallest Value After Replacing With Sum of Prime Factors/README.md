---
comments: true
difficulty: Medium
rating: 1499
source: Weekly Contest 324 Q2
tags:
    - Math
    - Number Theory
    - Primality Test
    - Sieve
    - Simulation
    - Sieve of Eratosthenes
    - Prime Factorization
---

<!-- problem:start -->

# [2507. Smallest Value After Replacing With Sum of Prime Factors](https://leetcode.com/problems/smallest-value-after-replacing-with-sum-of-prime-factors)

[中文文档](/solution/2500-2599/2507.Smallest%20Value%20After%20Replacing%20With%20Sum%20of%20Prime%20Factors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code>.</p>

<p>Liên tục thay <code>n</code> bằng tổng các <strong>thừa số nguyên tố</strong> của nó.</p>

<ul>
	<li>Lưu ý rằng nếu một thừa số nguyên tố chia hết <code>n</code> nhiều lần thì thừa số đó cũng phải được cộng vào tổng bấy nhiêu lần mà nó chia hết <code>n</code>.</li>
</ul>

<p>Trả về <em>giá trị nhỏ nhất mà </em><code>n</code><em> nhận được.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 15
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Ban đầu, n = 15.
15 = 3 * 5, nên thay n bằng 3 + 5 = 8.
8 = 2 * 2 * 2, nên thay n bằng 2 + 2 + 2 = 6.
6 = 2 * 3, nên thay n bằng 2 + 3 = 5.
5 là giá trị nhỏ nhất mà n nhận được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ban đầu, n = 3.
3 là giá trị nhỏ nhất mà n nhận được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng brute force

<!-- thinking:start -->

> **Tư duy**
>
> Thay $n$ bằng tổng các thừa số nguyên tố của nó cho đến khi giá trị không đổi. Mô phỏng trực tiếp phù hợp với $n\le 10^5$: tổng này nhỏ hơn giá trị ban đầu đối với hợp số và bằng $n$ đối với số nguyên tố, nên quá trình sẽ kết thúc.
>
> Phân tích thừa số của giá trị hiện tại bằng phép chia thử và cộng các thừa số. Nếu tổng bằng giá trị ban đầu thì đã đạt điểm bất động; nếu không, tiếp tục quá trình. Mỗi lần phân tích có độ phức tạp $O(\sqrt{n})$ và số lần lặp không nhiều.

<!-- thinking:end -->

Theo đề bài, ta có thể thực hiện quá trình phân tích thừa số nguyên tố, tức là liên tục phân tích một số thành các thừa số nguyên tố cho đến khi không thể phân tích tiếp. Trong quá trình này, cộng các thừa số nguyên tố mỗi khi phân tích được, rồi thực hiện lại quá trình một cách đệ quy hoặc lặp.

Độ phức tạp thời gian là $O(\sqrt{n})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestValue(self, n: int) -> int:
        while 1:
            t, s, i = n, 0, 2
            while i <= n // i:
                while n % i == 0:
                    n //= i
                    s += i
                i += 1
            if n > 1:
                s += n
            if s == t:
                return t
            n = s
```

#### Java

```java
class Solution {
    public int smallestValue(int n) {
        while (true) {
            int t = n, s = 0;
            for (int i = 2; i <= n / i; ++i) {
                while (n % i == 0) {
                    s += i;
                    n /= i;
                }
            }
            if (n > 1) {
                s += n;
            }
            if (s == t) {
                return s;
            }
            n = s;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestValue(int n) {
        while (1) {
            int t = n, s = 0;
            for (int i = 2; i <= n / i; ++i) {
                while (n % i == 0) {
                    s += i;
                    n /= i;
                }
            }
            if (n > 1) s += n;
            if (s == t) return s;
            n = s;
        }
    }
};
```

#### Go

```go
func smallestValue(n int) int {
	for {
		t, s := n, 0
		for i := 2; i <= n/i; i++ {
			for n%i == 0 {
				s += i
				n /= i
			}
		}
		if n > 1 {
			s += n
		}
		if s == t {
			return s
		}
		n = s
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
