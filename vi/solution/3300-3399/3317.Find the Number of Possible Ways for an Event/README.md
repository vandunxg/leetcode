---
comments: true
difficulty: Hard
rating: 2413
source: Biweekly Contest 141 Q4
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [3317. Find the Number of Possible Ways for an Event](https://leetcode.com/problems/find-the-number-of-possible-ways-for-an-event)

[中文文档](/solution/3300-3399/3317.Find%20the%20Number%20of%20Possible%20Ways%20for%20an%20Event/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <code>n</code>, <code>x</code> và <code>y</code>.</p>

<p>Một sự kiện có sự tham gia của <code>n</code> nghệ sĩ biểu diễn. Khi một nghệ sĩ đến, họ được <strong>phân công</strong> vào một trong <code>x</code> sân khấu. Tất cả nghệ sĩ được phân công vào <strong>cùng một</strong> sân khấu sẽ biểu diễn cùng nhau thành một ban nhạc, dù một số sân khấu <em>có thể</em> vẫn <strong>trống</strong>.</p>

<p>Sau khi tất cả tiết mục kết thúc, ban giám khảo sẽ <strong>trao</strong> cho mỗi ban nhạc một điểm trong khoảng <code>[1, y]</code>.</p>

<p>Hãy trả về <strong>tổng</strong> số cách có thể diễn ra sự kiện.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án theo <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý</strong> rằng hai sự kiện được xem là diễn ra <strong>khác nhau</strong> nếu thỏa mãn <strong>một trong hai</strong> điều kiện sau:</p>

<ul>
	<li><strong>Bất kỳ</strong> nghệ sĩ nào được <em>phân công</em> vào một sân khấu khác.</li>
	<li><strong>Bất kỳ</strong> ban nhạc nào được <em>trao</em> một điểm khác.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1, x = 2, y = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Có 2 cách phân công sân khấu cho nghệ sĩ.</li>
	<li>Ban giám khảo có thể trao cho ban nhạc duy nhất một trong các điểm 1, 2 hoặc 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, x = 2, y = 1</span></p>

<p><strong>Đầu ra:</strong> 32</p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mỗi nghệ sĩ sẽ được phân công vào sân khấu 1 hoặc sân khấu 2.</li>
	<li>Tất cả các ban nhạc đều được trao điểm 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, x = 3, y = 4</span></p>

<p><strong>Đầu ra:</strong> 684</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, x, y &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các nghệ sĩ chiếm nhiều nhất $x$ sân khấu không trống, sau đó mỗi sân khấu sẽ chọn một trong $y$ điểm. Với $n,x \le 1000$, ta có thể dùng quy hoạch động trên số người và số sân khấu đã sử dụng.
>
> Người thứ $i$ hoặc tham gia vào một trong $j$ sân khấu hiện có, hoặc mở một sân khấu mới trong số $x-j+1$ sân khấu chưa sử dụng. Hai trường hợp này phân hoạch toàn bộ các cách phân công.
>
> Sau khi phân công $n$ người, với mỗi $j$ ta nhân kết quả với $y^j$ để tính điểm của các sân khấu, rồi lấy tổng theo modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số cách sắp xếp $i$ nghệ sĩ đầu tiên vào $j$ sân khấu. Ban đầu, $f[0][0] = 1$, và các giá trị còn lại $f[i][j] = 0$.

Với $f[i][j]$, trong đó $1 \leq i \leq n$ và $1 \leq j \leq x$, ta xét nghệ sĩ thứ $i$:

- Nếu nghệ sĩ được phân công vào một sân khấu đã có nghệ sĩ, có $j$ lựa chọn, tức là $f[i - 1][j] \times j$;
- Nếu nghệ sĩ được phân công vào một sân khấu chưa có nghệ sĩ, có $x - (j - 1)$ lựa chọn, tức là $f[i - 1][j - 1] \times (x - (j - 1))$.

Vì vậy, công thức chuyển trạng thái là:

$$
f[i][j] = f[i - 1][j] \times j + f[i - 1][j - 1] \times (x - (j - 1))
$$

Với mỗi $j$, có $y^j$ lựa chọn điểm số, nên đáp án cuối cùng là:

$$
\sum_{j = 1}^{x} f[n][j] \times y^j
$$

Lưu ý rằng vì đáp án có thể rất lớn, ta cần lấy modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n \times x)$, và độ phức tạp không gian là $O(n \times x)$. Ở đây, $n$ và $x$ lần lượt là số nghệ sĩ và số sân khấu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWays(self, n: int, x: int, y: int) -> int:
        mod = 10**9 + 7
        f = [[0] * (x + 1) for _ in range(n + 1)]
        f[0][0] = 1
        for i in range(1, n + 1):
            for j in range(1, x + 1):
                f[i][j] = (f[i - 1][j] * j + f[i - 1][j - 1] * (x - (j - 1))) % mod
        ans, p = 0, 1
        for j in range(1, x + 1):
            p = p * y % mod
            ans = (ans + f[n][j] * p) % mod
        return ans
```

#### Java

```java
class Solution {
    public int numberOfWays(int n, int x, int y) {
        final int mod = (int) 1e9 + 7;
        long[][] f = new long[n + 1][x + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= x; ++j) {
                f[i][j] = (f[i - 1][j] * j % mod + f[i - 1][j - 1] * (x - (j - 1) % mod)) % mod;
            }
        }
        long ans = 0, p = 1;
        for (int j = 1; j <= x; ++j) {
            p = p * y % mod;
            ans = (ans + f[n][j] * p) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfWays(int n, int x, int y) {
        const int mod = 1e9 + 7;
        long long f[n + 1][x + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= x; ++j) {
                f[i][j] = (f[i - 1][j] * j % mod + f[i - 1][j - 1] * (x - (j - 1) % mod)) % mod;
            }
        }
        long long ans = 0, p = 1;
        for (int j = 1; j <= x; ++j) {
            p = p * y % mod;
            ans = (ans + f[n][j] * p) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfWays(n int, x int, y int) int {
	const mod int = 1e9 + 7
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, x+1)
	}
	f[0][0] = 1
	for i := 1; i <= n; i++ {
		for j := 1; j <= x; j++ {
			f[i][j] = (f[i-1][j]*j%mod + f[i-1][j-1]*(x-(j-1))%mod) % mod
		}
	}
	ans, p := 0, 1
	for j := 1; j <= x; j++ {
		p = p * y % mod
		ans = (ans + f[n][j]*p%mod) % mod
	}
	return ans
}
```

#### TypeScript

```ts
function numberOfWays(n: number, x: number, y: number): number {
    const mod = BigInt(10 ** 9 + 7);
    const f: bigint[][] = Array.from({ length: n + 1 }, () => Array(x + 1).fill(0n));
    f[0][0] = 1n;
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= x; ++j) {
            f[i][j] = (f[i - 1][j] * BigInt(j) + f[i - 1][j - 1] * BigInt(x - (j - 1))) % mod;
        }
    }
    let [ans, p] = [0n, 1n];
    for (let j = 1; j <= x; ++j) {
        p = (p * BigInt(y)) % mod;
        ans = (ans + f[n][j] * p) % mod;
    }
    return Number(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
