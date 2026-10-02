---
comments: true
difficulty: Hard
tags:
    - Math
    - Binary Search
    - Inclusion-Exclusion
    - Least Common Multiple
---

<!-- problem:start -->

# [878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number)

[中文文档](/solution/0800-0899/0878.Nth%20Magical%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên dương được gọi là <em>thần kỳ</em> nếu chia hết cho <code>a</code> hoặc <code>b</code>.</p>

<p>Cho ba số nguyên <code>n</code>, <code>a</code> và <code>b</code>, hãy trả về số thần kỳ thứ <code>n<sup>th</sup></code>. Vì đáp án có thể rất lớn, <strong>hãy trả về kết quả modulo </strong><code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, a = 2, b = 3
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, a = 2, b = 3
<strong>Đầu ra:</strong> 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>2 &lt;= a, b &lt;= 4 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm số nguyên dương thứ $n$ chia hết cho $a$ hoặc $b$, với $n$ có thể lên đến $10^9$. Liệt kê các bội số quá chậm. Số lượng số thần kỳ $\le x$ là $x/a+x/b-x/\mathrm{lcm}(a,b)$ và tăng đơn điệu theo $x$.
>
> Dùng tìm kiếm nhị phân để tìm $x$ nhỏ nhất sao cho số lượng đạt ít nhất $n$, rồi lấy kết quả modulo $10^9+7$. Cận $(a+b)\cdot n$ là đủ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nthMagicalNumber(self, n: int, a: int, b: int) -> int:
        mod = 10**9 + 7
        c = lcm(a, b)
        r = (a + b) * n
        return bisect_left(range(r), x=n, key=lambda x: x // a + x // b - x // c) % mod
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int nthMagicalNumber(int n, int a, int b) {
        int c = a * b / gcd(a, b);
        long l = 0, r = (long) (a + b) * n;
        while (l < r) {
            long mid = l + r >>> 1;
            if (mid / a + mid / b - mid / c >= n) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return (int) (l % MOD);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    const int mod = 1e9 + 7;

    int nthMagicalNumber(int n, int a, int b) {
        int c = lcm(a, b);
        ll l = 0, r = 1ll * (a + b) * n;
        while (l < r) {
            ll mid = l + r >> 1;
            if (mid / a + mid / b - mid / c >= n)
                r = mid;
            else
                l = mid + 1;
        }
        return l % mod;
    }
};
```

#### Go

```go
func nthMagicalNumber(n int, a int, b int) int {
	c := a * b / gcd(a, b)
	const mod int = 1e9 + 7
	r := (a + b) * n
	return sort.Search(r, func(x int) bool { return x/a+x/b-x/c >= n }) % mod
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
