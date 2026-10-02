---
comments: true
difficulty: Medium
rating: 2039
source: Weekly Contest 155 Q2
tags:
    - Math
    - Binary Search
    - Combinatorics
    - Greatest Common Divisor
    - Number Theory
    - Inclusion-Exclusion
    - Euclidean Algorithm
    - Least Common Multiple
---

<!-- problem:start -->

# [1201. Ugly Number III](https://leetcode.com/problems/ugly-number-iii)

[中文文档](/solution/1200-1299/1201.Ugly%20Number%20III/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số ugly</strong> là số nguyên dương chia hết cho <code>a</code>, <code>b</code> hoặc <code>c</code>.</p>

<p>Cho bốn số nguyên <code>n</code>, <code>a</code>, <code>b</code> và <code>c</code>, hãy trả về <strong>số ugly</strong> thứ <code>n<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 3, a = 2, b = 3, c = 5
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Các số ugly là 2, 3, 4, 5, 6, 8, 9, 10... Số thứ 3 là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 4, a = 2, b = 3, c = 4
<strong>Output:</strong> 6
<strong>Giải thích:</strong> Các số ugly là 2, 3, 4, 6, 8, 9, 10, 12... Số thứ 4 là 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 5, a = 2, b = 11, c = 13
<strong>Output:</strong> 10
<strong>Giải thích:</strong> Các số ugly là 2, 4, 6, 8, 10, 11, 12, 13... Số thứ 5 là 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, a, b, c &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= a * b * c &lt;= 10<sup>18</sup></code></li>
	<li>Đảm bảo kết quả nằm trong phạm vi <code>[1, 2 * 10<sup>9</sup>]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + nguyên lý bao hàm – loại trừ

<!-- thinking:start -->

> **Tư duy**
>
> Tạo lần lượt từng số ugly cần xét khoảng $n$ ứng viên, trong khi $n$ có thể lên đến $10^9$.
>
> Số lượng số ugly $\le x$ là hàm đơn điệu theo $x$, nên số ugly thứ $n$ là giá trị $x$ nhỏ nhất sao cho số lượng này ít nhất bằng $n$. Có thể tính số lượng trong $O(1)$ bằng nguyên lý bao hàm – loại trừ: cộng số bội của $a,b,c$, trừ các bội chung từng cặp, rồi cộng lại các bội chung của cả ba.
>
> Ta tìm kiếm nhị phân $x$ trong $[1, 2\times 10^9]$ và dùng nguyên lý bao hàm – loại trừ để kiểm tra giá trị ở giữa. Vì đề bài giới hạn miền chứa đáp án, ta không cần liệt kê từng số ugly.

<!-- thinking:end -->

Ta có thể đưa bài toán về dạng: tìm số nguyên dương $x$ nhỏ nhất sao cho có đúng $n$ số ugly nhỏ hơn hoặc bằng $x$.

Với số nguyên dương $x$, có $\left\lfloor \frac{x}{a} \right\rfloor$ số chia hết cho $a$, $\left\lfloor \frac{x}{b} \right\rfloor$ số chia hết cho $b$, $\left\lfloor \frac{x}{c} \right\rfloor$ số chia hết cho $c$, $\left\lfloor \frac{x}{lcm(a, b)} \right\rfloor$ số chia hết cho cả $a$ và $b$, $\left\lfloor \frac{x}{lcm(a, c)} \right\rfloor$ số chia hết cho cả $a$ và $c$, $\left\lfloor \frac{x}{lcm(b, c)} \right\rfloor$ số chia hết cho cả $b$ và $c$, và $\left\lfloor \frac{x}{lcm(a, b, c)} \right\rfloor$ số chia hết cho $a$, $b$ và $c$ đồng thời. Theo nguyên lý bao hàm – loại trừ, số lượng số ugly nhỏ hơn hoặc bằng $x$ là:

$$
\left\lfloor \frac{x}{a} \right\rfloor + \left\lfloor \frac{x}{b} \right\rfloor + \left\lfloor \frac{x}{c} \right\rfloor - \left\lfloor \frac{x}{lcm(a, b)} \right\rfloor - \left\lfloor \frac{x}{lcm(a, c)} \right\rfloor - \left\lfloor \frac{x}{lcm(b, c)} \right\rfloor + \left\lfloor \frac{x}{lcm(a, b, c)} \right\rfloor
$$

Ta có thể dùng tìm kiếm nhị phân để tìm số nguyên dương $x$ nhỏ nhất sao cho có đúng $n$ số ugly nhỏ hơn hoặc bằng $x$.

Đặt biên trái của tìm kiếm nhị phân là $l=1$ và biên phải là $r=2 \times 10^9$, trong đó $2 \times 10^9$ là giá trị lớn nhất theo giới hạn của đề bài. Ở mỗi bước, ta tìm giá trị giữa $mid$. Nếu số lượng số ugly nhỏ hơn hoặc bằng $mid$ lớn hơn hoặc bằng $n$, số nguyên dương $x$ nhỏ nhất nằm trong đoạn $[l,mid]$; nếu không, nó nằm trong đoạn $[mid+1,r]$. Trong quá trình tìm kiếm nhị phân, ta liên tục cập nhật số lượng số ugly nhỏ hơn hoặc bằng $mid$ cho đến khi tìm được $x$ nhỏ nhất.

Độ phức tạp thời gian là $O(\log m)$, với $m = 2 \times 10^9$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nthUglyNumber(self, n: int, a: int, b: int, c: int) -> int:
        ab = lcm(a, b)
        bc = lcm(b, c)
        ac = lcm(a, c)
        abc = lcm(a, b, c)
        l, r = 1, 2 * 10**9
        while l < r:
            mid = (l + r) >> 1
            if (
                mid // a
                + mid // b
                + mid // c
                - mid // ab
                - mid // bc
                - mid // ac
                + mid // abc
                >= n
            ):
                r = mid
            else:
                l = mid + 1
        return l
```

#### Java

```java
class Solution {
    public int nthUglyNumber(int n, int a, int b, int c) {
        long ab = lcm(a, b);
        long bc = lcm(b, c);
        long ac = lcm(a, c);
        long abc = lcm(ab, c);
        long l = 1, r = 2000000000;
        while (l < r) {
            long mid = (l + r) >> 1;
            if (mid / a + mid / b + mid / c - mid / ab - mid / bc - mid / ac + mid / abc >= n) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return (int) l;
    }

    private long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    private long lcm(long a, long b) {
        return a * b / gcd(a, b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int nthUglyNumber(int n, int a, int b, int c) {
        long long ab = lcm(a, b);
        long long bc = lcm(b, c);
        long long ac = lcm(a, c);
        long long abc = lcm(ab, c);
        long long l = 1, r = 2000000000;
        while (l < r) {
            long long mid = (l + r) >> 1;
            if (mid / a + mid / b + mid / c - mid / ab - mid / bc - mid / ac + mid / abc >= n) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    long long lcm(long long a, long long b) {
        return a * b / gcd(a, b);
    }

    long long gcd(long long a, long long b) {
        return b == 0 ? a : gcd(b, a % b);
    }
};
```

#### Go

```go
func nthUglyNumber(n int, a int, b int, c int) int {
	ab, bc, ac := lcm(a, b), lcm(b, c), lcm(a, c)
	abc := lcm(ab, c)
	var l, r int = 1, 2e9
	for l < r {
		mid := (l + r) >> 1
		if mid/a+mid/b+mid/c-mid/ab-mid/bc-mid/ac+mid/abc >= n {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return l
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}

func lcm(a, b int) int {
	return a * b / gcd(a, b)
}
```

#### TypeScript

```ts
function nthUglyNumber(n: number, a: number, b: number, c: number): number {
    const ab = lcm(BigInt(a), BigInt(b));
    const bc = lcm(BigInt(b), BigInt(c));
    const ac = lcm(BigInt(a), BigInt(c));
    const abc = lcm(BigInt(a), bc);
    let l = 1n;
    let r = BigInt(2e9);
    while (l < r) {
        const mid = (l + r) >> 1n;
        const count =
            mid / BigInt(a) +
            mid / BigInt(b) +
            mid / BigInt(c) -
            mid / ab -
            mid / bc -
            mid / ac +
            mid / abc;
        if (count >= BigInt(n)) {
            r = mid;
        } else {
            l = mid + 1n;
        }
    }
    return Number(l);
}

function gcd(a: bigint, b: bigint): bigint {
    return b === 0n ? a : gcd(b, a % b);
}

function lcm(a: bigint, b: bigint): bigint {
    return (a * b) / gcd(a, b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
