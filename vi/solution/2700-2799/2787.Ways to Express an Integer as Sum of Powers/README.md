---
comments: true
difficulty: Medium
rating: 1817
source: Biweekly Contest 109 Q4
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [2787. Ways to Express an Integer as Sum of Powers](https://leetcode.com/problems/ways-to-express-an-integer-as-sum-of-powers)

[中文文档](/solution/2700-2799/2787.Ways%20to%20Express%20an%20Integer%20as%20Sum%20of%20Powers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <strong>dương</strong> <code>n</code> và <code>x</code>.</p>

<p>Trả về <em>số cách </em><code>n</code><em> có thể được biểu diễn thành tổng các lũy thừa bậc </em><code>x<sup>th</sup></code><em> của các số nguyên dương <strong>phân biệt</strong>, nói cách khác, số tập hợp các số nguyên phân biệt </em><code>[n<sub>1</sub>, n<sub>2</sub>, ..., n<sub>k</sub>]</code><em> sao cho </em><code>n = n<sub>1</sub><sup>x</sup> + n<sub>2</sub><sup>x</sup> + ... + n<sub>k</sub><sup>x</sup></code><em>.</em></p>

<p>Vì kết quả có thể rất lớn, hãy trả về kết quả theo modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>Ví dụ, nếu <code>n = 160</code> và <code>x = 3</code>, một cách biểu diễn <code>n</code> là <code>n = 2<sup>3</sup> + 3<sup>3</sup> + 5<sup>3</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10, x = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể biểu diễn n như sau: n = 3<sup>2</sup> + 1<sup>2</sup> = 10.
Có thể chứng minh đây là cách duy nhất để biểu diễn 10 thành tổng các lũy thừa bậc 2<sup>nd</sup> của các số nguyên phân biệt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, x = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể biểu diễn n theo các cách sau:
- n = 4<sup>1</sup> = 4.
- n = 3<sup>1</sup> + 1<sup>1</sup> = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 300</code></li>
	<li><code>1 &lt;= x &lt;= 5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Viết $n$ thành tổng các lũy thừa bậc $x$ phân biệt và đếm số cách. Các cơ số phù hợp nhiều nhất là $n$, nhưng việc liệt kê tập con của các cơ số vẫn có số lượng theo cấp số mũ.
>
> Đây là bài toán ba lô $0$-$1$ với các vật phẩm $i^x$ và sức chứa $n$. $f[i][j]$ là số cách dùng $i$ cơ số đầu tiên để có tổng bằng $j$, trong đó chọn hoặc bỏ qua $i^x$. Đáp án là $f[n][n]$ modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số cách chọn một số từ $i$ số nguyên dương đầu tiên sao cho tổng các lũy thừa bậc $x$ của chúng bằng $j$. Ban đầu, $f[0][0] = 1$, các giá trị còn lại bằng $0$. Đáp án là $f[n][n]$.

Với mỗi số nguyên dương $i$, ta có thể chọn hoặc không chọn số đó:

- Không chọn: số cách là $f[i-1][j]$;
- Chọn: số cách là $f[i-1][j-i^x]$ (với điều kiện $j \geq i^x$).

Do đó, công thức chuyển trạng thái là:

$$
f[i][j] = f[i-1][j] + (j \geq i^x ? f[i-1][j-i^x] : 0)
$$

Lưu ý rằng đáp án có thể rất lớn, vì vậy ta cần lấy modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số nguyên được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWays(self, n: int, x: int) -> int:
        mod = 10**9 + 7
        f = [[0] * (n + 1) for _ in range(n + 1)]
        f[0][0] = 1
        for i in range(1, n + 1):
            k = pow(i, x)
            for j in range(n + 1):
                f[i][j] = f[i - 1][j]
                if k <= j:
                    f[i][j] = (f[i][j] + f[i - 1][j - k]) % mod
        return f[n][n]
```

#### Java

```java
class Solution {
    public int numberOfWays(int n, int x) {
        final int mod = (int) 1e9 + 7;
        int[][] f = new int[n + 1][n + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            long k = (long) Math.pow(i, x);
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (k <= j) {
                    f[i][j] = (f[i][j] + f[i - 1][j - (int) k]) % mod;
                }
            }
        }
        return f[n][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfWays(int n, int x) {
        const int mod = 1e9 + 7;
        int f[n + 1][n + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            long long k = (long long) pow(i, x);
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (k <= j) {
                    f[i][j] = (f[i][j] + f[i - 1][j - k]) % mod;
                }
            }
        }
        return f[n][n];
    }
};
```

#### Go

```go
func numberOfWays(n int, x int) int {
	const mod int = 1e9 + 7
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	f[0][0] = 1
	for i := 1; i <= n; i++ {
		k := int(math.Pow(float64(i), float64(x)))
		for j := 0; j <= n; j++ {
			f[i][j] = f[i-1][j]
			if k <= j {
				f[i][j] = (f[i][j] + f[i-1][j-k]) % mod
			}
		}
	}
	return f[n][n]
}
```

#### TypeScript

```ts
function numberOfWays(n: number, x: number): number {
    const mod = 10 ** 9 + 7;
    const f = Array.from({ length: n + 1 }, () => Array(n + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= n; ++i) {
        const k = Math.pow(i, x);
        for (let j = 0; j <= n; ++j) {
            f[i][j] = f[i - 1][j];
            if (k <= j) {
                f[i][j] = (f[i][j] + f[i - 1][j - k]) % mod;
            }
        }
    }
    return f[n][n];
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_ways(n: i32, x: i32) -> i32 {
        const MOD: i64 = 1_000_000_007;
        let n = n as usize;
        let x = x as u32;
        let mut f = vec![vec![0; n + 1]; n + 1];
        f[0][0] = 1;
        for i in 1..=n {
            let k = (i as i64).pow(x);
            for j in 0..=n {
                f[i][j] = f[i - 1][j];
                if j >= k as usize {
                    f[i][j] = (f[i][j] + f[i - 1][j - k as usize]) % MOD;
                }
            }
        }
        f[n][n] as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} x
 * @return {number}
 */
var numberOfWays = function (n, x) {
    const mod = 10 ** 9 + 7;
    const f = Array.from({ length: n + 1 }, () => Array(n + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= n; ++i) {
        const k = Math.pow(i, x);
        for (let j = 0; j <= n; ++j) {
            f[i][j] = f[i - 1][j];
            if (k <= j) {
                f[i][j] = (f[i][j] + f[i - 1][j - k]) % mod;
            }
        }
    }
    return f[n][n];
};
```

#### C#

```cs
public class Solution {
    public int NumberOfWays(int n, int x) {
        const int mod = 1000000007;
        int[,] f = new int[n + 1, n + 1];
        f[0, 0] = 1;
        for (int i = 1; i <= n; ++i) {
            long k = (long)Math.Pow(i, x);
            for (int j = 0; j <= n; ++j) {
                f[i, j] = f[i - 1, j];
                if (k <= j) {
                    f[i, j] = (f[i, j] + f[i - 1, j - (int)k]) % mod;
                }
            }
        }
        return f[n, n];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
