---
comments: true
difficulty: Medium
rating: 1301
source: Weekly Contest 500 Q2
tags:
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3918. Sum of Primes Between Number and Its Reverse](https://leetcode.com/problems/sum-of-primes-between-number-and-its-reverse)

[中文文档](/solution/3900-3999/3918.Sum%20of%20Primes%20Between%20Number%20and%20Its%20Reverse/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Gọi <code>r</code> là số nguyên thu được bằng cách đảo ngược các chữ số của <code>n</code>.</p>

<p>Trả về <strong>tổng</strong> của tất cả <span data-keyword="prime-number">số nguyên tố</span> nằm giữa <code>min(n, r)</code> và <code>max(n, r)</code>, bao gồm cả hai đầu mút.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 13</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">132</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Số đảo ngược của 13 là 31. Do đó, khoảng là <code>[13, 31]</code>.</li>
	<li>Các số nguyên tố trong khoảng này là 13, 17, 19, 23, 29 và 31.</li>
	<li>Tổng các số nguyên tố này là <code>13 + 17 + 19 + 23 + 29 + 31 = 132</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">17</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Số đảo ngược của 10 là 1. Do đó, khoảng là <code>[1, 10]</code>.</li>
	<li>Các số nguyên tố trong khoảng này là 2, 3, 5 và 7.</li>
	<li>Tổng các số nguyên tố này là <code>2 + 3 + 5 + 7 = 17</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Số đảo ngược của 8 là 8. Do đó, khoảng là <code>[8, 8]</code>.</li>
	<li>Không có số nguyên tố nào trong khoảng này, nên tổng bằng 0.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý số nguyên tố

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 1000$, số đảo ngược có nhiều nhất bốn chữ số và độ dài khoảng nhiều nhất khoảng $1000$. Việc chia thử cho từng phần tử là đủ nhanh, nhưng lặp lại phép kiểm tra nguyên tố sẽ lãng phí.
>
> Sàng tất cả số nguyên tố đến $1000$ một lần, sau đó cộng những số nằm trong $[\min(n,r),\max(n,r)]$.
>
> Sàng có độ phức tạp $O(M\log\log M)$, còn truy vấn có độ phức tạp tuyến tính theo độ dài khoảng.

<!-- thinking:end -->

Ta nhận thấy số đảo ngược $r$ của $n$ không vượt quá 1000, nên có thể tiền xử lý tất cả số nguyên tố đến 1000.

Tiếp theo, ta tính $low = \min(n, r)$ và $high = \max(n, r)$, rồi duyệt qua tất cả số nguyên trong khoảng $[low, high]$. Nếu một số là số nguyên tố, ta cộng số đó vào đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(M)$, trong đó $M$ là cận trên dùng để tiền xử lý số nguyên tố, ở đây là 1000.

<!-- tabs:start -->

#### Python3

```python
limit = 1000
is_prime = [True] * (limit + 1)
is_prime[0] = is_prime[1] = False
for i in range(2, int(limit**0.5) + 1):
    if is_prime[i]:
        for j in range(i * i, limit + 1, i):
            is_prime[j] = False


class Solution:
    def sumOfPrimesInRange(self, n: int) -> int:
        r = int(str(n)[::-1])
        low = min(n, r)
        high = max(n, r)
        return sum(x for x in range(low, high + 1) if is_prime[x])
```

#### Java

```java
class Solution {
    private static final int LIMIT = 1000;
    private static final boolean[] isPrime = new boolean[LIMIT + 1];
    static {
        for (int i = 0; i <= LIMIT; i++) {
            isPrime[i] = true;
        }
        isPrime[0] = isPrime[1] = false;
        for (int i = 2; i * i <= LIMIT; i++) {
            if (isPrime[i]) {
                for (int j = i * i; j <= LIMIT; j += i) {
                    isPrime[j] = false;
                }
            }
        }
    }
    public int sumOfPrimesInRange(int n) {
        int r = Integer.parseInt(new StringBuilder(String.valueOf(n)).reverse().toString());
        int low = Math.min(n, r);
        int high = Math.max(n, r);
        int ans = 0;
        for (int x = low; x <= high; x++) {
            if (isPrime[x]) {
                ans += x;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
const int MX = 1000;
bool isPrime[MX + 1];

auto init = [] {
    for (int i = 0; i <= MX; ++i) isPrime[i] = true;
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i * i <= MX; ++i) {
        if (isPrime[i]) {
            for (int j = i * i; j <= MX; j += i) {
                isPrime[j] = false;
            }
        }
    }
    return 0;
}();

class Solution {
public:
    int sumOfPrimesInRange(int n) {
        int r = 0;
        int tmp = n;
        while (tmp) {
            r = r * 10 + tmp % 10;
            tmp /= 10;
        }
        int low = min(n, r);
        int high = max(n, r);
        int ans = 0;
        for (int x = low; x <= high; ++x) {
            if (isPrime[x]) ans += x;
        }
        return ans;
    }
};
```

#### Go

```go
var isPrime [1001]bool

func init() {
	for i := 0; i <= 1000; i++ {
		isPrime[i] = true
	}
	isPrime[0], isPrime[1] = false, false
	for i := 2; i*i <= 1000; i++ {
		if isPrime[i] {
			for j := i * i; j <= 1000; j += i {
				isPrime[j] = false
			}
		}
	}
}

func sumOfPrimesInRange(n int) (ans int) {
	r := 0
	for x := n; x > 0; x /= 10 {
		r = r*10 + x%10
	}
	low := min(n, r)
	high := max(n, r)
	for x := low; x <= high; x++ {
		if isPrime[x] {
			ans += x
		}
	}
	return
}
```

#### TypeScript

```ts
const LIMIT = 1000;
const isPrime: boolean[] = new Array(LIMIT + 1).fill(true);
isPrime[0] = isPrime[1] = false;
for (let i = 2; i * i <= LIMIT; i++) {
    if (isPrime[i]) {
        for (let j = i * i; j <= LIMIT; j += i) {
            isPrime[j] = false;
        }
    }
}

function sumOfPrimesInRange(n: number): number {
    const r = parseInt(n.toString().split('').reverse().join(''));
    const low = Math.min(n, r);
    const high = Math.max(n, r);
    let sum = 0;
    for (let x = low; x <= high; x++) {
        if (isPrime[x]) sum += x;
    }
    return sum;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
