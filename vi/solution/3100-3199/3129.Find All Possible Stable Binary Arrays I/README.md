---
comments: true
difficulty: Medium
rating: 2200
source: Biweekly Contest 129 Q3
tags:
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3129. Find All Possible Stable Binary Arrays I](https://leetcode.com/problems/find-all-possible-stable-binary-arrays-i)

[Tài liệu tiếng Trung](/solution/3100-3199/3129.Find%20All%20Possible%20Stable%20Binary%20Arrays%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho 3 số nguyên dương <code>num_zeros</code>, <code>num_ones</code> và <code>limit</code>.</p>

<p>Một <span data-keyword="binary-array">mảng nhị phân</span> <code>arr</code> được gọi là <strong>ổn định</strong> nếu:</p>

<ul>
	<li>Số lần xuất hiện của 0 trong <code>arr</code> <strong>chính xác là </strong><code>num_zeros</code>.</li>
	<li>Số lần xuất hiện của 1 trong <code>arr</code> <strong>chính xác là</strong> <code>num_ones</code>.</li>
	<li>Mỗi <span data-keyword="subarray-nonempty">mảng con</span> của <code>arr</code> có kích thước lớn hơn <code>limit</code> phải chứa <strong>ít nhất</strong> một lần xuất hiện của <strong>cả hai</strong> số 0 và 1.</li>
</ul>

<p>Trả về một số nguyên biểu thị <em>tổng số</em> <strong>mảng nhị phân ổn định</strong>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">zero = 1, one = 1, limit = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai mảng nhị phân ổn định có thể tạo được là <code>[1,0]</code> và <code>[0,1]</code>, vì cả hai mảng đều có một số 0 và một số 1, đồng thời không có mảng con nào có độ dài lớn hơn 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">zero = 1, one = 2, limit = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng nhị phân ổn định duy nhất có thể tạo được là <code>[1,0,1]</code>.</p>

<p>Lưu ý rằng các mảng nhị phân <code>[1,1,0]</code> và <code>[0,1,1]</code> có các mảng con độ dài 2 gồm các phần tử giống nhau, nên chúng không ổn định.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">zero = 3, one = 3, limit = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các mảng nhị phân ổn định có thể tạo được là <code>[0,0,1,0,1,1]</code>, <code>[0,0,1,1,0,1]</code>, <code>[0,1,0,0,1,1]</code>, <code>[0,1,0,1,0,1]</code>, <code>[0,1,0,1,1,0]</code>, <code>[0,1,1,0,0,1]</code>, <code>[0,1,1,0,1,0]</code>, <code>[1,0,0,1,0,1]</code>, <code>[1,0,0,1,1,0]</code>, <code>[1,0,1,0,0,1]</code>, <code>[1,0,1,0,1,0]</code>, <code>[1,0,1,1,0,0]</code>, <code>[1,1,0,0,1,0]</code> và <code>[1,1,0,1,0,0]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= zero, one, limit &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng ổn định không bao giờ có quá $limit$ bit giống nhau liên tiếp. Nếu backtracking trên $zero$ số 0 và $one$ số 1, số trường hợp sẽ tăng rất nhanh.
>
> Tính hợp lệ của một prefix chỉ phụ thuộc vào số lượng số 0 và số 1 còn lại, cùng với bit sẽ được thêm tiếp theo. Trường hợp vượt giới hạn xảy ra khi thêm bit giống với $limit+1$ bit giống nhau sau bit đối diện.
>
> Gọi $dfs(i,j,k)$ là số cách khi còn lại $i$ số 0, $j$ số 1 và bit tiếp theo là $k$. Ta cộng các chuyển trạng thái cùng màu và khác màu, rồi trừ đi trường hợp vượt giới hạn. Memoization giúp số trạng thái còn $O(zero\cdot one)$.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs}(i, j, k)$ là số mảng nhị phân ổn định thỏa mãn các điều kiện của đề bài khi còn $i$ số 0 và $j$ số 1 cần đặt, đồng thời chữ số tiếp theo cần điền là $k$. Khi đó, đáp án là $\textit{dfs}(\textit{zero}, \textit{one}, 0) + \textit{dfs}(\textit{zero}, \textit{one}, 1)$.

Quy trình tính $\textit{dfs}(i, j, k)$ như sau:

- Nếu $i \lt 0$ hoặc $j \lt 0$, trả về $0$.
- Nếu $i = 0$, trả về $1$ khi $k = 1$ và $j \leq \textit{limit}$; ngược lại trả về $0$.
- Nếu $j = 0$, trả về $1$ khi $k = 0$ và $i \leq \textit{limit}$; ngược lại trả về $0$.
- Nếu $k = 0$, ta xét trường hợp chữ số trước đó là $0$, tức là $\textit{dfs}(i - 1, j, 0)$, và trường hợp chữ số trước đó là $1$, tức là $\textit{dfs}(i - 1, j, 1)$. Nếu chữ số trước đó là $0$, có thể có nhiều hơn $\textit{limit}$ số 0 trong một mảng con. Khi đó, trường hợp chữ số thứ $(\textit{limit} + 1)$ tính từ cuối là $1$ không hợp lệ, nên ta trừ trường hợp này: $\textit{dfs}(i - \textit{limit} - 1, j, 1)$.
- Nếu $k = 1$, ta xét trường hợp chữ số trước đó là $0$, tức là $\textit{dfs}(i, j - 1, 0)$, và trường hợp chữ số trước đó là $1$, tức là $\textit{dfs}(i, j - 1, 1)$. Nếu chữ số trước đó là $1$, có thể có nhiều hơn $\textit{limit}$ số 1 trong một mảng con. Khi đó, trường hợp chữ số thứ $(\textit{limit} + 1)$ tính từ cuối là $0$ không hợp lệ, nên ta trừ trường hợp này: $\textit{dfs}(i, j - \textit{limit} - 1, 0)$.

Để tránh tính toán lặp lại, ta sử dụng tìm kiếm có memoization.

Độ phức tạp thời gian là $O(\textit{zero} \times \textit{one})$, và độ phức tạp không gian là $O(\textit{zero} \times \textit{one})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfStableArrays(self, zero: int, one: int, limit: int) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if i == 0:
                return int(k == 1 and j <= limit)
            if j == 0:
                return int(k == 0 and i <= limit)
            if k == 0:
                return (
                    dfs(i - 1, j, 0)
                    + dfs(i - 1, j, 1)
                    - (0 if i - limit - 1 < 0 else dfs(i - limit - 1, j, 1))
                )
            return (
                dfs(i, j - 1, 0)
                + dfs(i, j - 1, 1)
                - (0 if j - limit - 1 < 0 else dfs(i, j - limit - 1, 0))
            )

        mod = 10**9 + 7
        ans = (dfs(zero, one, 0) + dfs(zero, one, 1)) % mod
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;
    private Long[][][] f;
    private int limit;

    public int numberOfStableArrays(int zero, int one, int limit) {
        f = new Long[zero + 1][one + 1][2];
        this.limit = limit;
        return (int) ((dfs(zero, one, 0) + dfs(zero, one, 1)) % mod);
    }

    private long dfs(int i, int j, int k) {
        if (i < 0 || j < 0) {
            return 0;
        }
        if (i == 0) {
            return k == 1 && j <= limit ? 1 : 0;
        }
        if (j == 0) {
            return k == 0 && i <= limit ? 1 : 0;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        if (k == 0) {
            f[i][j][k] = (dfs(i - 1, j, 0) + dfs(i - 1, j, 1) - dfs(i - limit - 1, j, 1) + mod) % mod;
        } else {
            f[i][j][k] = (dfs(i, j - 1, 0) + dfs(i, j - 1, 1) - dfs(i, j - limit - 1, 0) + mod) % mod;
        }
        return f[i][j][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfStableArrays(int zero, int one, int limit) {
        const int mod = 1e9 + 7;
        using ll = long long;
        vector<vector<array<ll, 2>>> f = vector<vector<array<ll, 2>>>(zero + 1, vector<array<ll, 2>>(one + 1, {-1, -1}));
        auto dfs = [&](this auto&& dfs, int i, int j, int k) -> ll {
            if (i < 0 || j < 0) {
                return 0;
            }
            if (i == 0) {
                return k == 1 && j <= limit;
            }
            if (j == 0) {
                return k == 0 && i <= limit;
            }
            ll& res = f[i][j][k];
            if (res != -1) {
                return res;
            }
            if (k == 0) {
                res = (dfs(i - 1, j, 0) + dfs(i - 1, j, 1) - dfs(i - limit - 1, j, 1) + mod) % mod;
            } else {
                res = (dfs(i, j - 1, 0) + dfs(i, j - 1, 1) - dfs(i, j - limit - 1, 0) + mod) % mod;
            }
            return res;
        };
        return (dfs(zero, one, 0) + dfs(zero, one, 1)) % mod;
    }
};
```

#### Go

```go
func numberOfStableArrays(zero int, one int, limit int) int {
	const mod int = 1e9 + 7
	f := make([][][2]int, zero+1)
	for i := range f {
		f[i] = make([][2]int, one+1)
		for j := range f[i] {
			f[i][j] = [2]int{-1, -1}
		}
	}
	var dfs func(i, j, k int) int
	dfs = func(i, j, k int) int {
		if i < 0 || j < 0 {
			return 0
		}
		if i == 0 {
			if k == 1 && j <= limit {
				return 1
			}
			return 0
		}
		if j == 0 {
			if k == 0 && i <= limit {
				return 1
			}
			return 0
		}
		res := &f[i][j][k]
		if *res != -1 {
			return *res
		}
		if k == 0 {
			*res = (dfs(i-1, j, 0) + dfs(i-1, j, 1) - dfs(i-limit-1, j, 1) + mod) % mod
		} else {
			*res = (dfs(i, j-1, 0) + dfs(i, j-1, 1) - dfs(i, j-limit-1, 0) + mod) % mod
		}
		return *res
	}
	return (dfs(zero, one, 0) + dfs(zero, one, 1)) % mod
}
```

#### TypeScript

```ts
function numberOfStableArrays(zero: number, one: number, limit: number): number {
    const mod = 1e9 + 7;
    const f: number[][][] = Array.from({ length: zero + 1 }, () =>
        Array.from({ length: one + 1 }, () => [-1, -1]),
    );

    const dfs = (i: number, j: number, k: number): number => {
        if (i < 0 || j < 0) {
            return 0;
        }
        if (i === 0) {
            return k === 1 && j <= limit ? 1 : 0;
        }
        if (j === 0) {
            return k === 0 && i <= limit ? 1 : 0;
        }
        let res = f[i][j][k];
        if (res !== -1) {
            return res;
        }
        if (k === 0) {
            res = (dfs(i - 1, j, 0) + dfs(i - 1, j, 1) - dfs(i - limit - 1, j, 1) + mod) % mod;
        } else {
            res = (dfs(i, j - 1, 0) + dfs(i, j - 1, 1) - dfs(i, j - limit - 1, 0) + mod) % mod;
        }
        return (f[i][j][k] = res);
    };

    return (dfs(zero, one, 0) + dfs(zero, one, 1)) % mod;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_stable_arrays(zero: i32, one: i32, limit: i32) -> i32 {
        const MOD: i64 = 1_000_000_007;

        fn dfs(
            i: i32,
            j: i32,
            k: usize,
            limit: i32,
            f: &mut Vec<Vec<[i64; 2]>>,
        ) -> i64 {
            if i < 0 || j < 0 {
                return 0;
            }

            if i == 0 {
                return if k == 1 && j <= limit { 1 } else { 0 };
            }

            if j == 0 {
                return if k == 0 && i <= limit { 1 } else { 0 };
            }

            let (iu, ju) = (i as usize, j as usize);

            if f[iu][ju][k] != -1 {
                return f[iu][ju][k];
            }

            let res = if k == 0 {
                (
                    dfs(i - 1, j, 0, limit, f)
                    + dfs(i - 1, j, 1, limit, f)
                    - dfs(i - limit - 1, j, 1, limit, f)
                    + MOD
                ) % MOD
            } else {
                (
                    dfs(i, j - 1, 0, limit, f)
                    + dfs(i, j - 1, 1, limit, f)
                    - dfs(i, j - limit - 1, 0, limit, f)
                    + MOD
                ) % MOD
            };

            f[iu][ju][k] = res;
            res
        }

        let mut f = vec![vec![[-1_i64; 2]; (one + 1) as usize]; (zero + 1) as usize];

        ((dfs(zero, one, 0, limit, &mut f) + dfs(zero, one, 1, limit, &mut f)) % MOD) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Công thức truy hồi không có hiệu ứng về sau; đệ quy vẫn phải chịu overhead của cache và lời gọi hàm khi đạt giới hạn lớn.
>
> Ta dùng cùng chuyển trạng thái để điền bảng $f[i][j][k]$: đã dùng $i$ số 0 và $j$ số 1, bit cuối cùng là $k$.
>
> Các trường hợp cơ sở là các chuỗi chỉ gồm một loại bit và có độ dài không vượt quá $limit$. Sau khi lặp và lấy modulo, đọc $f[zero][one][0]+f[zero][one][1]$.

<!-- thinking:end -->

Ta cũng có thể chuyển đổi cách tìm kiếm có memoization trong Lời giải 1 thành quy hoạch động.

Ta định nghĩa $f[i][j][k]$ là số mảng nhị phân ổn định sử dụng $i$ số 0 và $j$ số 1, với chữ số cuối cùng là $k$. Khi đó, đáp án là $f[zero][one][0] + f[zero][one][1]$.

Ban đầu, ta có $f[i][0][0] = 1$, với $1 \leq i \leq \min(\textit{limit}, \textit{zero})$; và $f[0][j][1] = 1$, với $1 \leq j \leq \min(\textit{limit}, \textit{one})$.

Các công thức chuyển trạng thái như sau:

- $f[i][j][0] = f[i - 1][j][0] + f[i - 1][j][1] - f[i - \textit{limit} - 1][j][1]$.
- $f[i][j][1] = f[i][j - 1][0] + f[i][j - 1][1] - f[i][j - \textit{limit} - 1][0]$.

Độ phức tạp thời gian là $O(\textit{zero} \times \textit{one})$, và độ phức tạp không gian là $O(\textit{zero} \times \textit{one})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfStableArrays(self, zero: int, one: int, limit: int) -> int:
        mod = 10**9 + 7
        f = [[[0, 0] for _ in range(one + 1)] for _ in range(zero + 1)]
        for i in range(1, min(limit, zero) + 1):
            f[i][0][0] = 1
        for j in range(1, min(limit, one) + 1):
            f[0][j][1] = 1
        for i in range(1, zero + 1):
            for j in range(1, one + 1):
                x = 0 if i - limit - 1 < 0 else f[i - limit - 1][j][1]
                y = 0 if j - limit - 1 < 0 else f[i][j - limit - 1][0]
                f[i][j][0] = (f[i - 1][j][0] + f[i - 1][j][1] - x) % mod
                f[i][j][1] = (f[i][j - 1][0] + f[i][j - 1][1] - y) % mod
        return sum(f[zero][one]) % mod
```

#### Java

```java
class Solution {
    public int numberOfStableArrays(int zero, int one, int limit) {
        final int mod = (int) 1e9 + 7;
        long[][][] f = new long[zero + 1][one + 1][2];
        for (int i = 1; i <= Math.min(zero, limit); ++i) {
            f[i][0][0] = 1;
        }
        for (int j = 1; j <= Math.min(one, limit); ++j) {
            f[0][j][1] = 1;
        }
        for (int i = 1; i <= zero; ++i) {
            for (int j = 1; j <= one; ++j) {
                long x = i - limit - 1 < 0 ? 0 : f[i - limit - 1][j][1];
                long y = j - limit - 1 < 0 ? 0 : f[i][j - limit - 1][0];
                f[i][j][0] = (f[i - 1][j][0] + f[i - 1][j][1] - x + mod) % mod;
                f[i][j][1] = (f[i][j - 1][0] + f[i][j - 1][1] - y + mod) % mod;
            }
        }
        return (int) ((f[zero][one][0] + f[zero][one][1]) % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfStableArrays(int zero, int one, int limit) {
        const int mod = 1e9 + 7;
        using ll = long long;
        ll f[zero + 1][one + 1][2];
        memset(f, 0, sizeof(f));
        for (int i = 1; i <= min(zero, limit); ++i) {
            f[i][0][0] = 1;
        }
        for (int j = 1; j <= min(one, limit); ++j) {
            f[0][j][1] = 1;
        }
        for (int i = 1; i <= zero; ++i) {
            for (int j = 1; j <= one; ++j) {
                ll x = i - limit - 1 < 0 ? 0 : f[i - limit - 1][j][1];
                ll y = j - limit - 1 < 0 ? 0 : f[i][j - limit - 1][0];
                f[i][j][0] = (f[i - 1][j][0] + f[i - 1][j][1] - x + mod) % mod;
                f[i][j][1] = (f[i][j - 1][0] + f[i][j - 1][1] - y + mod) % mod;
            }
        }
        return (f[zero][one][0] + f[zero][one][1]) % mod;
    }
};
```

#### Go

```go
func numberOfStableArrays(zero int, one int, limit int) int {
	const mod int = 1e9 + 7
	f := make([][][2]int, zero+1)
	for i := range f {
		f[i] = make([][2]int, one+1)
	}
	for i := 1; i <= min(zero, limit); i++ {
		f[i][0][0] = 1
	}
	for j := 1; j <= min(one, limit); j++ {
		f[0][j][1] = 1
	}
	for i := 1; i <= zero; i++ {
		for j := 1; j <= one; j++ {
			f[i][j][0] = (f[i-1][j][0] + f[i-1][j][1]) % mod
			if i-limit-1 >= 0 {
				f[i][j][0] = (f[i][j][0] - f[i-limit-1][j][1] + mod) % mod
			}
			f[i][j][1] = (f[i][j-1][0] + f[i][j-1][1]) % mod
			if j-limit-1 >= 0 {
				f[i][j][1] = (f[i][j][1] - f[i][j-limit-1][0] + mod) % mod
			}
		}
	}
	return (f[zero][one][0] + f[zero][one][1]) % mod
}
```

#### TypeScript

```ts
function numberOfStableArrays(zero: number, one: number, limit: number): number {
    const mod = 1e9 + 7;
    const f: number[][][] = Array.from({ length: zero + 1 }, () =>
        Array.from({ length: one + 1 }, () => [0, 0]),
    );

    for (let i = 1; i <= Math.min(limit, zero); i++) {
        f[i][0][0] = 1;
    }
    for (let j = 1; j <= Math.min(limit, one); j++) {
        f[0][j][1] = 1;
    }

    for (let i = 1; i <= zero; i++) {
        for (let j = 1; j <= one; j++) {
            const x = i - limit - 1 < 0 ? 0 : f[i - limit - 1][j][1];
            const y = j - limit - 1 < 0 ? 0 : f[i][j - limit - 1][0];
            f[i][j][0] = (f[i - 1][j][0] + f[i - 1][j][1] - x + mod) % mod;
            f[i][j][1] = (f[i][j - 1][0] + f[i][j - 1][1] - y + mod) % mod;
        }
    }

    return (f[zero][one][0] + f[zero][one][1]) % mod;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_stable_arrays(zero: i32, one: i32, limit: i32) -> i32 {
        let mod_: i64 = 1_000_000_007;

        let zero = zero as usize;
        let one = one as usize;
        let limit = limit as usize;

        let mut f = vec![vec![[0_i64; 2]; one + 1]; zero + 1];

        for i in 1..=zero.min(limit) {
            f[i][0][0] = 1;
        }

        for j in 1..=one.min(limit) {
            f[0][j][1] = 1;
        }

        for i in 1..=zero {
            for j in 1..=one {
                let x = if i > limit { f[i - limit - 1][j][1] } else { 0 };
                let y = if j > limit { f[i][j - limit - 1][0] } else { 0 };

                f[i][j][0] = (f[i - 1][j][0] + f[i - 1][j][1] - x + mod_) % mod_;
                f[i][j][1] = (f[i][j - 1][0] + f[i][j - 1][1] - y + mod_) % mod_;
            }
        }

        ((f[zero][one][0] + f[zero][one][1]) % mod_) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
