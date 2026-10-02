---
comments: true
difficulty: Medium
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [790. Domino and Tromino Tiling](https://leetcode.com/problems/domino-and-tromino-tiling)

[中文文档](/solution/0700-0799/0790.Domino%20and%20Tromino%20Tiling/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có hai loại miếng ghép: domino hình <code>2 x 1</code> và tromino. Bạn có thể xoay các miếng ghép này.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0790.Domino%20and%20Tromino%20Tiling/images/lc-domino.jpg" style="width: 362px; height: 195px;" />
<p>Cho số nguyên n, hãy trả về <em>số cách lát một bảng</em> <code>2 x n</code>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Khi lát, mọi ô vuông phải được phủ bởi một miếng ghép. Hai cách lát được xem là khác nhau khi và chỉ khi tồn tại hai ô kề nhau theo một trong bốn hướng sao cho chỉ một trong hai cách lát có cả hai ô được phủ bởi cùng một miếng ghép.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0790.Domino%20and%20Tromino%20Tiling/images/lc-domino1.jpg" style="width: 500px; height: 226px;" />
<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Năm cách lát khác nhau được minh họa ở trên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Lát bảng $2\times n$ bằng domino và tromino hình chữ L. $n\le 1000$; liệt kê cách đặt sẽ khiến ta phải tính lại nhiều cột.
>
> Một cột có thể được phủ kín, chỉ phủ ô trên, chỉ phủ ô dưới, hoặc để trống. Bốn trạng thái ở cột $i-1$ xác định trạng thái ở cột $i$ sau khi đặt một domino dọc, hai domino ngang hoặc một miếng chữ L.
>
> Dùng rolling array gồm bốn số nguyên; $f[0]$ biểu thị đoạn đầu bảng đã được phủ kín. Bảng rỗng có trạng thái ban đầu là $1$.

<!-- thinking:end -->

Trước hết, ta cần hiểu yêu cầu bài toán: tìm số cách lát bảng $2 \times n$, trong đó mỗi ô trên bảng chỉ được phủ bởi một miếng ghép.

Có hai loại miếng ghép: hình `2 x 1` và hình chữ `L`; cả hai loại đều có thể xoay. Ta ký hiệu các miếng sau khi xoay là hình `1 x 2` và `L'`.

Ta định nghĩa $f[i][j]$ là số cách lát phần bảng đầu tiên có kích thước $2 \times i$, trong đó $j$ biểu thị trạng thái của cột cuối. Cột cuối có 4 trạng thái:

- Cột cuối được phủ kín, ký hiệu là $0$.
- Cột cuối chỉ có ô trên được phủ, ký hiệu là $1$.
- Cột cuối chỉ có ô dưới được phủ, ký hiệu là $2$.
- Cột cuối chưa được phủ, ký hiệu là $3$.

Đáp án là $f[n][0]$. Ban đầu, $f[0][0] = 1$ và các trạng thái còn lại $f[0][j] = 0$.

Xét cách lát đến cột thứ $i$ và viết các phương trình chuyển trạng thái:

Khi $j = 0$, cột cuối được phủ kín. Trạng thái này có thể chuyển từ các trạng thái $0, 1, 2, 3$ của cột trước bằng cách đặt các miếng tương ứng: $f[i-1][0]$ với một miếng `1 x 2`, $f[i-1][1]$ với một miếng `L'`, $f[i-1][2]$ với một miếng `L'`, hoặc $f[i-1][3]$ với hai miếng `2 x 1`. Do đó, $f[i][0] = \sum_{j=0}^3 f[i-1][j]$.

Khi $j = 1$, cột cuối chỉ có ô trên được phủ. Trạng thái này có thể chuyển từ trạng thái $2$ hoặc $3$ của cột trước bằng cách đặt một miếng `2 x 1` hoặc một miếng `L`. Do đó, $f[i][1] = f[i-1][2] + f[i-1][3]$.

Khi $j = 2$, cột cuối chỉ có ô dưới được phủ. Trạng thái này có thể chuyển từ trạng thái $1$ hoặc $3$ của cột trước bằng cách đặt một miếng `2 x 1` hoặc một miếng `L'`. Do đó, $f[i][2] = f[i-1][1] + f[i-1][3]$.

Khi $j = 3$, cột cuối chưa được phủ. Trạng thái này có thể chuyển từ trạng thái $0$ của cột trước. Do đó, $f[i][3] = f[i-1][0]$.

Ta thấy các phương trình chuyển trạng thái chỉ phụ thuộc vào trạng thái của cột trước, nên có thể dùng rolling array để tối ưu độ phức tạp không gian.

Các giá trị trạng thái có thể rất lớn, vì vậy cần lấy modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, với $n$ là số cột của bảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numTilings(self, n: int) -> int:
        f = [1, 0, 0, 0]
        mod = 10**9 + 7
        for i in range(1, n + 1):
            g = [0] * 4
            g[0] = (f[0] + f[1] + f[2] + f[3]) % mod
            g[1] = (f[2] + f[3]) % mod
            g[2] = (f[1] + f[3]) % mod
            g[3] = f[0]
            f = g
        return f[0]
```

#### Java

```java
class Solution {
    public int numTilings(int n) {
        long[] f = {1, 0, 0, 0};
        int mod = (int) 1e9 + 7;
        for (int i = 1; i <= n; ++i) {
            long[] g = new long[4];
            g[0] = (f[0] + f[1] + f[2] + f[3]) % mod;
            g[1] = (f[2] + f[3]) % mod;
            g[2] = (f[1] + f[3]) % mod;
            g[3] = f[0];
            f = g;
        }
        return (int) f[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numTilings(int n) {
        const int mod = 1e9 + 7;
        long long f[4] = {1, 0, 0, 0};
        for (int i = 1; i <= n; ++i) {
            long long g[4];
            g[0] = (f[0] + f[1] + f[2] + f[3]) % mod;
            g[1] = (f[2] + f[3]) % mod;
            g[2] = (f[1] + f[3]) % mod;
            g[3] = f[0];
            memcpy(f, g, sizeof(g));
        }
        return f[0];
    }
};
```

#### Go

```go
func numTilings(n int) int {
	f := [4]int{}
	f[0] = 1
	const mod int = 1e9 + 7
	for i := 1; i <= n; i++ {
		g := [4]int{}
		g[0] = (f[0] + f[1] + f[2] + f[3]) % mod
		g[1] = (f[2] + f[3]) % mod
		g[2] = (f[1] + f[3]) % mod
		g[3] = f[0]
		f = g
	}
	return f[0]
}
```

#### TypeScript

```ts
function numTilings(n: number): number {
    const mod = 1_000_000_007;
    let f: number[] = [1, 0, 0, 0];

    for (let i = 1; i <= n; ++i) {
        const g: number[] = Array(4);
        g[0] = (f[0] + f[1] + f[2] + f[3]) % mod;
        g[1] = (f[2] + f[3]) % mod;
        g[2] = (f[1] + f[3]) % mod;
        g[3] = f[0] % mod;
        f = g;
    }

    return f[0];
}
```

#### Rust

```rust
impl Solution {
    pub fn num_tilings(n: i32) -> i32 {
        const MOD: i64 = 1_000_000_007;
        let mut f: [i64; 4] = [1, 0, 0, 0];
        for _ in 1..=n {
            let mut g = [0i64; 4];
            g[0] = (f[0] + f[1] + f[2] + f[3]) % MOD;
            g[1] = (f[2] + f[3]) % MOD;
            g[2] = (f[1] + f[3]) % MOD;
            g[3] = f[0] % MOD;
            f = g;
        }
        f[0] as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
