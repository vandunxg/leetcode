---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - Sieve
    - Mixed Knapsack
---

<!-- problem:start -->

# [3183. The Number of Ways to Make the Sum 🔒](https://leetcode.com/problems/the-number-of-ways-to-make-the-sum)

[中文文档](/solution/3100-3199/3183.The%20Number%20of%20Ways%20to%20Make%20the%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <strong>vô hạn</strong> đồng xu có mệnh giá 1, 2 và 6, cùng với <strong>chỉ</strong> 2 đồng xu có mệnh giá 4.</p>

<p>Cho một số nguyên <code>n</code>, hãy trả về số cách tạo ra tổng bằng <code>n</code> bằng những đồng xu bạn có.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý</strong> rằng thứ tự các đồng xu không quan trọng và <code>[2, 2, 3]</code> được xem là giống <code>[2, 3, 2]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bốn tổ hợp là: <code>[1, 1, 1, 1]</code>, <code>[1, 1, 2]</code>, <code>[2, 2]</code>, <code>[4]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 12</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">22</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lưu ý rằng <code>[4, 4, 4]</code> <strong>không</strong> phải là một tổ hợp hợp lệ vì chúng ta không thể sử dụng đồng xu 4 ba lần.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bốn tổ hợp là: <code>[1, 1, 1, 1, 1]</code>, <code>[1, 1, 1, 2]</code>, <code>[1, 2, 2]</code>, <code>[1, 4]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (Knapsack đầy đủ)

<!-- thinking:start -->

> **Tư duy**
>
> Các đồng xu có mệnh giá $1,2,4,6$, trong đó có nhiều nhất hai đồng xu mệnh giá $4$. Nếu dùng knapsack với bốn mệnh giá, ta vẫn phải giới hạn số lượng đồng xu $4$.
>
> Dùng complete knapsack cho $1,2,6$ vào $f[j]$, sau đó cộng các trường hợp có $0/1/2$ đồng xu $4$ là $f[n]+f[n-4]+f[n-8]$.
>
> Vòng lặp bên trong chạy theo chiều tăng đối với unbounded knapsack. Cộng thêm các hạng tử khi $n$ đạt $4$ hoặc $8$, theo modulo $10^9+7$.

<!-- thinking:end -->

Trước hết, ta có thể bỏ qua đồng xu $4$, định nghĩa mảng đồng xu `coins = [1, 2, 6]`, sau đó áp dụng ý tưởng của bài toán complete knapsack. Ta định nghĩa $f[j]$ là số cách tạo ra số tiền $j$ bằng $i$ loại đồng xu đầu tiên, ban đầu $f[0] = 1$. Sau đó, ta duyệt qua mảng đồng xu `coins`, với mỗi đồng xu $x$, ta duyệt các số tiền từ $x$ đến $n$ và cập nhật $f[j] = f[j] + f[j - x]$.

Cuối cùng, $f[n]$ là số cách tạo ra số tiền $n$ bằng các đồng xu $1, 2, 6$. Sau đó, nếu $n \geq 4$, ta xét trường hợp chọn một đồng xu $4$, khi đó số cách là $f[n] + f[n - 4]$; nếu $n \geq 8$, ta xét trường hợp chọn hai đồng xu $4$, khi đó số cách là $f[n] + f[n - 4] + f[n - 8]$.

Lưu ý thực hiện phép modulo cho đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số tiền.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWays(self, n: int) -> int:
        mod = 10**9 + 7
        coins = [1, 2, 6]
        f = [0] * (n + 1)
        f[0] = 1
        for x in coins:
            for j in range(x, n + 1):
                f[j] = (f[j] + f[j - x]) % mod
        ans = f[n]
        if n >= 4:
            ans = (ans + f[n - 4]) % mod
        if n >= 8:
            ans = (ans + f[n - 8]) % mod
        return ans
```

#### Java

```java
class Solution {
    public int numberOfWays(int n) {
        final int mod = (int) 1e9 + 7;
        int[] coins = {1, 2, 6};
        int[] f = new int[n + 1];
        f[0] = 1;
        for (int x : coins) {
            for (int j = x; j <= n; ++j) {
                f[j] = (f[j] + f[j - x]) % mod;
            }
        }
        int ans = f[n];
        if (n >= 4) {
            ans = (ans + f[n - 4]) % mod;
        }
        if (n >= 8) {
            ans = (ans + f[n - 8]) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfWays(int n) {
        const int mod = 1e9 + 7;
        int coins[3] = {1, 2, 6};
        int f[n + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (int x : coins) {
            for (int j = x; j <= n; ++j) {
                f[j] = (f[j] + f[j - x]) % mod;
            }
        }
        int ans = f[n];
        if (n >= 4) {
            ans = (ans + f[n - 4]) % mod;
        }
        if (n >= 8) {
            ans = (ans + f[n - 8]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfWays(n int) int {
	const mod int = 1e9 + 7
	coins := []int{1, 2, 6}
	f := make([]int, n+1)
	f[0] = 1
	for _, x := range coins {
		for j := x; j <= n; j++ {
			f[j] = (f[j] + f[j-x]) % mod
		}
	}
	ans := f[n]
	if n >= 4 {
		ans = (ans + f[n-4]) % mod
	}
	if n >= 8 {
		ans = (ans + f[n-8]) % mod
	}
	return ans
}
```

#### TypeScript

```ts
function numberOfWays(n: number): number {
    const mod = 10 ** 9 + 7;
    const f: number[] = Array(n + 1).fill(0);
    f[0] = 1;
    for (const x of [1, 2, 6]) {
        for (let j = x; j <= n; ++j) {
            f[j] = (f[j] + f[j - x]) % mod;
        }
    }
    let ans = f[n];
    if (n >= 4) {
        ans = (ans + f[n - 4]) % mod;
    }
    if (n >= 8) {
        ans = (ans + f[n - 8]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tiền xử lý + Quy hoạch động (Complete Knapsack)

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 xây dựng lại $f$ cho mỗi truy vấn. Miền giá trị cố định ở mức $10^5$, nên nhiều truy vấn sẽ tính lại cùng một bảng.
>
> Tiền xử lý $f[1..10^5]$ và trả lời bằng một vài lần tra cứu chỉ số.
>
> Trả về $f[n]$, $f[n]+f[n-4]$ hoặc thêm $f[n-8]$ tùy theo giá trị của $n$.

<!-- thinking:end -->

Ta có thể tiền xử lý số cách tạo ra mọi số tiền từ $1$ đến $10^5$, sau đó trả về số cách tương ứng với giá trị của $n$:

- Nếu $n < 4$, trả về trực tiếp $f[n]$;
- Nếu $4 \leq n < 8$, trả về $f[n] + f[n - 4]$;
- Nếu $n \geq 8$, trả về $f[n] + f[n - 4] + f[n - 8]$.

Lưu ý thực hiện phép modulo cho đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số tiền.

<!-- tabs:start -->

#### Python3

```python
m = 10**5 + 1
mod = 10**9 + 7
coins = [1, 2, 6]
f = [0] * (m)
f[0] = 1
for x in coins:
    for j in range(x, m):
        f[j] = (f[j] + f[j - x]) % mod


class Solution:
    def numberOfWays(self, n: int) -> int:
        ans = f[n]
        if n >= 4:
            ans = (ans + f[n - 4]) % mod
        if n >= 8:
            ans = (ans + f[n - 8]) % mod
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = 1000000007;
    private static final int M = 100001;
    private static final int[] COINS = {1, 2, 6};
    private static final int[] f = new int[M];

    static {
        f[0] = 1;
        for (int x : COINS) {
            for (int j = x; j < M; ++j) {
                f[j] = (f[j] + f[j - x]) % MOD;
            }
        }
    }

    public int numberOfWays(int n) {
        int ans = f[n];
        if (n >= 4) {
            ans = (ans + f[n - 4]) % MOD;
        }
        if (n >= 8) {
            ans = (ans + f[n - 8]) % MOD;
        }
        return ans;
    }
}
```

#### C++

```cpp
const int m = 1e5 + 1;
const int mod = 1e9 + 7;
int f[m + 1];

auto init = [] {
    f[0] = 1;
    int coins[3] = {1, 2, 6};
    for (int x : coins) {
        for (int j = x; j < m; ++j) {
            f[j] = (f[j] + f[j - x]) % mod;
        }
    }
    return 0;
}();


class Solution {
public:
    int numberOfWays(int n) {
        int ans = f[n];
        if (n >= 4) {
            ans = (ans + f[n - 4]) % mod;
        }
        if (n >= 8) {
            ans = (ans + f[n - 8]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
const (
	m   = 100001
	mod = 1000000007
)

var f [m]int

func init() {
	f[0] = 1
	coins := []int{1, 2, 6}
	for _, x := range coins {
		for j := x; j < m; j++ {
			f[j] = (f[j] + f[j-x]) % mod
		}
	}
}

func numberOfWays(n int) int {
	ans := f[n]
	if n >= 4 {
		ans = (ans + f[n-4]) % mod
	}
	if n >= 8 {
		ans = (ans + f[n-8]) % mod
	}
	return ans
}
```

#### TypeScript

```ts
const m: number = 10 ** 5 + 1;
const mod: number = 10 ** 9 + 7;
const f: number[] = Array(m).fill(0);

(() => {
    f[0] = 1;
    const coins: number[] = [1, 2, 6];
    for (const x of coins) {
        for (let j = x; j < m; ++j) {
            f[j] = (f[j] + f[j - x]) % mod;
        }
    }
})();

function numberOfWays(n: number): number {
    let ans = f[n];
    if (n >= 4) {
        ans = (ans + f[n - 4]) % mod;
    }
    if (n >= 8) {
        ans = (ans + f[n - 8]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
