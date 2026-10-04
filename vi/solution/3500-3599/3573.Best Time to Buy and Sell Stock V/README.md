---
comments: true
difficulty: Medium
rating: 1777
source: Biweekly Contest 158 Q2
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3573. Best Time to Buy and Sell Stock V](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-v)

[中文文档](/solution/3500-3599/3573.Best%20Time%20to%20Buy%20and%20Sell%20Stock%20V/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>prices</code>, trong đó <code>prices[i]</code> là giá cổ phiếu tính bằng đô la vào ngày thứ <code>i<sup>th</sup></code>, và một số nguyên <code>k</code>.</p>

<p>Bạn được phép thực hiện tối đa <code>k</code> giao dịch, trong đó mỗi giao dịch có thể thuộc một trong các loại sau:</p>

<ul>
	<li>
	<p><strong>Giao dịch thông thường</strong>: Mua vào ngày <code>i</code>, sau đó bán vào một ngày sau đó <code>j</code> sao cho <code>i &lt; j</code>. Lợi nhuận là <code>prices[j] - prices[i]</code>.</p>
	</li>
	<li>
	<p><strong>Giao dịch bán khống</strong>: Bán vào ngày <code>i</code>, sau đó mua lại vào một ngày sau đó <code>j</code> sao cho <code>i &lt; j</code>. Lợi nhuận là <code>prices[i] - prices[j]</code>.</p>
	</li>
</ul>

<p><strong>Lưu ý</strong> rằng bạn phải hoàn tất mỗi giao dịch trước khi bắt đầu giao dịch khác. Ngoài ra, bạn không thể mua hoặc bán trong cùng ngày với ngày bán hoặc mua lại của giao dịch trước đó.</p>

<p>Trả về tổng lợi nhuận <strong>lớn nhất</strong> có thể thu được khi thực hiện <strong>tối đa</strong> <code>k</code> giao dịch.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [1,7,9,8,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>
Bạn có thể thu được lợi nhuận $14 thông qua 2 giao dịch:

<ul>
	<li>Giao dịch thông thường: mua cổ phiếu vào ngày 0 với giá $1 then sell it on day 2 for $9.</li>
	<li>Giao dịch bán khống: bán cổ phiếu vào ngày 3 với giá $8 then buy back on day 4 for $2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [12,16,19,19,8,1,19,13,9], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">36</span></p>

<p><strong>Giải thích:</strong></p>
Bạn có thể thu được lợi nhuận $36 thông qua 3 giao dịch:

<ul>
	<li>Giao dịch thông thường: mua cổ phiếu vào ngày 0 với giá $12 then sell it on day 2 for $19.</li>
	<li>Giao dịch bán khống: bán cổ phiếu vào ngày 3 với giá $19 then buy back on day 4 for $8.</li>
	<li>Giao dịch thông thường: mua cổ phiếu vào ngày 5 với giá $1 then sell it on day 6 for $19.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= prices.length &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= prices.length / 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tương tự các bài toán cổ phiếu khác, ta có thể hoàn tất tối đa $k$ giao dịch, nhưng một giao dịch có thể là mua trước bán sau hoặc bán trước mua lại sau. Vì vậy, trạng thái cần phân biệt ba trường hợp: không nắm giữ, đang nắm giữ cổ phiếu và đang có vị thế bán khống.
>
> $f[i][j][0/1/2]$ là lợi nhuận tốt nhất sau $i$ ngày, với tối đa $j$ giao dịch, tương ứng với từng trạng thái nắm giữ. Việc mở một vị thế sẽ sử dụng một giao dịch; việc đóng vị thế sẽ đưa ta về trạng thái không nắm giữ. Đáp án là $f[n-1][k][0]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j][k]$ là lợi nhuận lớn nhất trong $i$ ngày đầu tiên, với tối đa $j$ giao dịch và trạng thái hiện tại là $k$. Trạng thái $k$ có ba khả năng:

- Nếu $k = 0$, nghĩa là hiện tại không nắm giữ cổ phiếu nào.
- Nếu $k = 1$, nghĩa là hiện tại đang nắm giữ một cổ phiếu.
- Nếu $k = 2$, nghĩa là hiện tại đang có một vị thế bán khống.

Ban đầu, với mọi $j \in [1, k]$, ta có $f[0][j][1] = -prices[0]$ và $f[0][j][2] = prices[0]$. Điều này tương ứng với việc mua cổ phiếu hoặc mở một vị thế bán khống vào ngày 0.

Tiếp theo, ta cập nhật $f[i][j][k]$ bằng các phép chuyển trạng thái. Với mỗi ngày $i$ và mỗi số giao dịch $j$, ta cập nhật dựa trên trạng thái hiện tại $k$:

- Nếu $k = 0$, nghĩa là không nắm giữ cổ phiếu, trạng thái này có thể đạt được từ ba trường hợp:
    - Ngày hôm trước không nắm giữ cổ phiếu.
    - Ngày hôm trước đang nắm giữ cổ phiếu và hôm nay bán ra.
    - Ngày hôm trước đang có vị thế bán khống và hôm nay mua lại.
- Nếu $k = 1$, nghĩa là đang nắm giữ cổ phiếu, trạng thái này có thể đạt được từ hai trường hợp:
    - Ngày hôm trước đã nắm giữ cổ phiếu.
    - Ngày hôm trước không nắm giữ cổ phiếu và hôm nay mua vào.
- Nếu $k = 2$, nghĩa là đang có vị thế bán khống, trạng thái này có thể đạt được từ hai trường hợp:
    - Ngày hôm trước đã có vị thế bán khống.
    - Ngày hôm trước không nắm giữ cổ phiếu và hôm nay mở một vị thế bán khống (bán ra).

Cụ thể, với $1 \leq i < n$ và $1 \leq j \leq k$, ta có các phương trình chuyển trạng thái sau:

$$
\begin{aligned}
f[i][j][0] &= \max(f[i - 1][j][0], f[i - 1][j][1] + prices[i], f[i - 1][j][2] - prices[i]) \\
f[i][j][1] &= \max(f[i - 1][j][1], f[i - 1][j - 1][0] - prices[i]) \\
f[i][j][2] &= \max(f[i - 1][j][2], f[i - 1][j - 1][0] + prices[i])
\end{aligned}
$$

Cuối cùng, ta trả về $f[n - 1][k][0]$, là lợi nhuận lớn nhất sau tối đa $k$ giao dịch và không nắm giữ cổ phiếu nào khi kết thúc $n$ ngày.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(n \times k)$, trong đó $n$ là độ dài của mảng $\textit{prices}$ và $k$ là số giao dịch tối đa.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumProfit(self, prices: List[int], k: int) -> int:
        n = len(prices)
        f = [[[0] * 3 for _ in range(k + 1)] for _ in range(n)]
        for j in range(1, k + 1):
            f[0][j][1] = -prices[0]
            f[0][j][2] = prices[0]
        for i in range(1, n):
            for j in range(1, k + 1):
                f[i][j][0] = max(
                    f[i - 1][j][0],
                    f[i - 1][j][1] + prices[i],
                    f[i - 1][j][2] - prices[i],
                )
                f[i][j][1] = max(f[i - 1][j][1], f[i - 1][j - 1][0] - prices[i])
                f[i][j][2] = max(f[i - 1][j][2], f[i - 1][j - 1][0] + prices[i])
        return f[n - 1][k][0]
```

#### Java

```java
class Solution {
    public long maximumProfit(int[] prices, int k) {
        int n = prices.length;
        long[][][] f = new long[n][k + 1][3];
        for (int j = 1; j <= k; ++j) {
            f[0][j][1] = -prices[0];
            f[0][j][2] = prices[0];
        }
        for (int i = 1; i < n; ++i) {
            for (int j = 1; j <= k; ++j) {
                f[i][j][0] = Math.max(f[i - 1][j][0],
                    Math.max(f[i - 1][j][1] + prices[i], f[i - 1][j][2] - prices[i]));
                f[i][j][1] = Math.max(f[i - 1][j][1], f[i - 1][j - 1][0] - prices[i]);
                f[i][j][2] = Math.max(f[i - 1][j][2], f[i - 1][j - 1][0] + prices[i]);
            }
        }
        return f[n - 1][k][0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumProfit(vector<int>& prices, int k) {
        int n = prices.size();
        long long f[n][k + 1][3];
        memset(f, 0, sizeof(f));
        for (int j = 1; j <= k; ++j) {
            f[0][j][1] = -prices[0];
            f[0][j][2] = prices[0];
        }

        for (int i = 1; i < n; ++i) {
            for (int j = 1; j <= k; ++j) {
                f[i][j][0] = max({f[i - 1][j][0], f[i - 1][j][1] + prices[i], f[i - 1][j][2] - prices[i]});
                f[i][j][1] = max(f[i - 1][j][1], f[i - 1][j - 1][0] - prices[i]);
                f[i][j][2] = max(f[i - 1][j][2], f[i - 1][j - 1][0] + prices[i]);
            }
        }

        return f[n - 1][k][0];
    }
};
```

#### Go

```go
func maximumProfit(prices []int, k int) int64 {
	n := len(prices)
	f := make([][][3]int, n)
	for i := range f {
		f[i] = make([][3]int, k+1)
	}

	for j := 1; j <= k; j++ {
		f[0][j][1] = -prices[0]
		f[0][j][2] = prices[0]
	}

	for i := 1; i < n; i++ {
		for j := 1; j <= k; j++ {
			f[i][j][0] = max(f[i-1][j][0], f[i-1][j][1]+prices[i], f[i-1][j][2]-prices[i])
			f[i][j][1] = max(f[i-1][j][1], f[i-1][j-1][0]-prices[i])
			f[i][j][2] = max(f[i-1][j][2], f[i-1][j-1][0]+prices[i])
		}
	}

	return int64(f[n-1][k][0])
}
```

#### TypeScript

```ts
function maximumProfit(prices: number[], k: number): number {
    const n = prices.length;
    const f: number[][][] = Array.from({ length: n }, () =>
        Array.from({ length: k + 1 }, () => Array(3).fill(0)),
    );

    for (let j = 1; j <= k; ++j) {
        f[0][j][1] = -prices[0];
        f[0][j][2] = prices[0];
    }

    for (let i = 1; i < n; ++i) {
        for (let j = 1; j <= k; ++j) {
            f[i][j][0] = Math.max(
                f[i - 1][j][0],
                f[i - 1][j][1] + prices[i],
                f[i - 1][j][2] - prices[i],
            );
            f[i][j][1] = Math.max(f[i - 1][j][1], f[i - 1][j - 1][0] - prices[i]);
            f[i][j][2] = Math.max(f[i - 1][j][2], f[i - 1][j - 1][0] + prices[i]);
        }
    }

    return f[n - 1][k][0];
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_profit(prices: Vec<i32>, k: i32) -> i64 {
        let n = prices.len();
        let k = k as usize;
        let mut f = vec![vec![vec![0i64; 3]; k + 1]; n];
        for j in 1..=k {
            f[0][j][1] = -(prices[0] as i64);
            f[0][j][2] = prices[0] as i64;
        }
        for i in 1..n {
            for j in 1..=k {
                f[i][j][0] = f[i - 1][j][0]
                    .max(f[i - 1][j][1] + prices[i] as i64)
                    .max(f[i - 1][j][2] - prices[i] as i64);
                f[i][j][1] = f[i - 1][j][1].max(f[i - 1][j - 1][0] - prices[i] as i64);
                f[i][j][2] = f[i - 1][j][2].max(f[i - 1][j - 1][0] + prices[i] as i64);
            }
        }
        f[n - 1][k][0]
    }
}
```

#### C#

```cs
public class Solution {
    public long MaximumProfit(int[] prices, int k) {
        int n = prices.Length;
        long[,,] f = new long[n, k + 1, 3];

        for (int j = 1; j <= k; ++j) {
            f[0, j, 1] = -prices[0];
            f[0, j, 2] = prices[0];
        }

        for (int i = 1; i < n; ++i) {
            for (int j = 1; j <= k; ++j) {
                f[i, j, 0] = Math.Max(
                    f[i - 1, j, 0],
                    Math.Max(
                        f[i - 1, j, 1] + prices[i],
                        f[i - 1, j, 2] - prices[i]
                    )
                );
                f[i, j, 1] = Math.Max(
                    f[i - 1, j, 1],
                    f[i - 1, j - 1, 0] - prices[i]
                );
                f[i, j, 2] = Math.Max(
                    f[i - 1, j, 2],
                    f[i - 1, j - 1, 0] + prices[i]
                );
            }
        }

        return f[n - 1, k, 0];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
