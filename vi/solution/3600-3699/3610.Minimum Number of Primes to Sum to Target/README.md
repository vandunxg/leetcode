---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
    - Number Theory
---

<!-- problem:start -->

# [3610. Minimum Number of Primes to Sum to Target 🔒](https://leetcode.com/problems/minimum-number-of-primes-to-sum-to-target)

[中文文档](/solution/3600-3699/3610.Minimum%20Number%20of%20Primes%20to%20Sum%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>n</code> và <code>m</code>.</p>

<p>Bạn cần chọn một multiset gồm các <strong><span data-keyword="prime-number">số nguyên tố</span></strong> từ <strong>nhóm đầu tiên gồm</strong> <code>m</code> số nguyên tố sao cho tổng của các số nguyên tố được chọn <strong>chính xác</strong> bằng <code>n</code>. Bạn có thể sử dụng mỗi số nguyên tố <strong>nhiều lần</strong>.</p>

<p>Trả về số lượng <strong>nhỏ nhất</strong> các số nguyên tố cần dùng để có tổng bằng <code>n</code>, hoặc -1 nếu không thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10, m = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>2 số nguyên tố đầu tiên là [2, 3]. Có thể tạo tổng 10 bằng 2 + 2 + 3 + 3, cần 4 số nguyên tố.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 15, m = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>5 số nguyên tố đầu tiên là [2, 3, 5, 7, 11]. Có thể tạo tổng 15 bằng 5 + 5 + 5, cần 3 số nguyên tố.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7, m = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>6 số nguyên tố đầu tiên là [2, 3, 5, 7, 11, 13]. Có thể tạo tổng 7 trực tiếp bằng số nguyên tố 7, nên chỉ cần 1 số nguyên tố.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 1000</code></li>
    <li><code>1 &lt;= m &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Việc tạo ra $n$ từ $m$ số nguyên tố đầu tiên với khả năng lặp lại là một bài toán ba lô không giới hạn. Không cần tìm kiếm tổ hợp: do $n,m\le 1000$, ta có thể dùng một công thức truy hồi $O(mn)$.
>
> Dùng sàng để lấy $1000$ số nguyên tố đầu tiên. Gọi $f[i]$ là số lượng số nguyên tố ít nhất có tổng bằng $i$, với $f[0]=0$ và các giá trị còn lại là $\infty$.
>
> Với mỗi số nguyên tố $p$, duyệt $i$ theo chiều tăng dần và đặt $f[i]=\min(f[i],f[i-p]+1)$ để có thể sử dụng lại cùng một số nguyên tố. Nếu $f[n]$ vẫn là $\infty$, trả về $-1$.

<!-- thinking:end -->

Trước tiên, ta tiền xử lý để lấy $1000$ số nguyên tố đầu tiên, sau đó dùng quy hoạch động để giải bài toán.

Đặt $f[i]$ là số lượng số nguyên tố nhỏ nhất cần dùng để có tổng bằng $i$. Ban đầu, đặt $f[0] = 0$ và các giá trị $f[i] = \infty$ còn lại. Với mỗi số nguyên tố $p$, ta cập nhật $f[i]$ từ $f[i - p]$ như sau:

$$
f[i] = \min(f[i], f[i - p] + 1)
$$

Nếu $f[n]$ vẫn là $\infty$, điều đó có nghĩa là không thể tạo ra $n$ từ $m$ số nguyên tố đầu tiên, nên trả về -1; ngược lại, trả về $f[n]$.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(n + M)$, trong đó $M$ là số lượng số nguyên tố được tiền xử lý (ở đây là $1000$).

<!-- tabs:start -->

#### Python3

```python
primes = []
x = 2
M = 1000
while len(primes) < M:
    is_prime = True
    for p in primes:
        if p * p > x:
            break
        if x % p == 0:
            is_prime = False
            break
    if is_prime:
        primes.append(x)
    x += 1


class Solution:
    def minNumberOfPrimes(self, n: int, m: int) -> int:
        min = lambda x, y: x if x < y else y
        f = [0] + [inf] * n
        for x in primes[:m]:
            for i in range(x, n + 1):
                f[i] = min(f[i], f[i - x] + 1)
        return f[n] if f[n] < inf else -1
```

#### Java

```java
class Solution {
    static List<Integer> primes = new ArrayList<>();
    static {
        int x = 2;
        int M = 1000;
        while (primes.size() < M) {
            boolean is_prime = true;
            for (int p : primes) {
                if (p * p > x) {
                    break;
                }
                if (x % p == 0) {
                    is_prime = false;
                    break;
                }
            }
            if (is_prime) {
                primes.add(x);
            }
            x++;
        }
    }

    public int minNumberOfPrimes(int n, int m) {
        int[] f = new int[n + 1];
        final int inf = 1 << 30;
        Arrays.fill(f, inf);
        f[0] = 0;
        for (int x : primes.subList(0, m)) {
            for (int i = x; i <= n; i++) {
                f[i] = Math.min(f[i], f[i - x] + 1);
            }
        }
        return f[n] < inf ? f[n] : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNumberOfPrimes(int n, int m) {
        static vector<int> primes;
        if (primes.empty()) {
            int x = 2;
            int M = 1000;
            while ((int) primes.size() < M) {
                bool is_prime = true;
                for (int p : primes) {
                    if (p * p > x) break;
                    if (x % p == 0) {
                        is_prime = false;
                        break;
                    }
                }
                if (is_prime) primes.push_back(x);
                x++;
            }
        }

        vector<int> f(n + 1, INT_MAX);
        f[0] = 0;
        for (int x : vector<int>(primes.begin(), primes.begin() + m)) {
            for (int i = x; i <= n; ++i) {
                if (f[i - x] != INT_MAX) {
                    f[i] = min(f[i], f[i - x] + 1);
                }
            }
        }
        return f[n] < INT_MAX ? f[n] : -1;
    }
};
```

#### Go

```go
var primes []int

func init() {
    x := 2
    M := 1000
    for len(primes) < M {
        is_prime := true
        for _, p := range primes {
            if p*p > x {
                break
            }
            if x%p == 0 {
                is_prime = false
                break
            }
        }
        if is_prime {
            primes = append(primes, x)
        }
        x++
    }
}

func minNumberOfPrimes(n int, m int) int {
    const inf = int(1e9)
    f := make([]int, n+1)
    for i := 1; i <= n; i++ {
        f[i] = inf
    }
    f[0] = 0

    for _, x := range primes[:m] {
        for i := x; i <= n; i++ {
            if f[i-x] < inf && f[i-x]+1 < f[i] {
                f[i] = f[i-x] + 1
            }
        }
    }

    if f[n] < inf {
        return f[n]
    }
    return -1
}
```

#### TypeScript

```ts
const primes: number[] = [];
let x = 2;
const M = 1000;
while (primes.length < M) {
    let is_prime = true;
    for (const p of primes) {
        if (p * p > x) break;
        if (x % p === 0) {
            is_prime = false;
            break;
        }
    }
    if (is_prime) primes.push(x);
    x++;
}

function minNumberOfPrimes(n: number, m: number): number {
    const inf = 1e9;
    const f: number[] = Array(n + 1).fill(inf);
    f[0] = 0;

    for (const x of primes.slice(0, m)) {
        for (let i = x; i <= n; i++) {
            if (f[i - x] < inf) {
                f[i] = Math.min(f[i], f[i - x] + 1);
            }
        }
    }

    return f[n] < inf ? f[n] : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
