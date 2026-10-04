---
comments: true
difficulty: Medium
rating: 1556
source: Weekly Contest 463 Q1
tags:
    - Array
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [3652. Best Time to Buy and Sell Stock using Strategy](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-using-strategy)

[中文文档](/solution/3600-3699/3652.Best%20Time%20to%20Buy%20and%20Sell%20Stock%20using%20Strategy/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>prices</code> và <code>strategy</code>, trong đó:</p>

<ul>
	<li><code>prices[i]</code> là giá của một cổ phiếu vào ngày thứ <code>i<sup>th</sup></code>.</li>
	<li><code>strategy[i]</code> biểu thị hành động giao dịch vào ngày thứ <code>i<sup>th</sup></code>, trong đó:
	<ul>
		<li><code>-1</code> biểu thị mua một đơn vị cổ phiếu.</li>
		<li><code>0</code> biểu thị giữ cổ phiếu.</li>
		<li><code>1</code> biểu thị bán một đơn vị cổ phiếu.</li>
	</ul>
	</li>
</ul>

<p>Đồng thời, cho một số nguyên <strong>chẵn</strong> <code>k</code>, và ta được phép thực hiện <strong>tối đa một</strong> lần sửa đổi trên <code>strategy</code>. Một lần sửa đổi bao gồm:</p>

<ul>
	<li>Chọn đúng <code>k</code> phần tử <strong>liên tiếp</strong> trong <code>strategy</code>.</li>
	<li>Đặt <strong><code>k / 2</code> phần tử đầu tiên</strong> thành <code>0</code> (giữ).</li>
	<li>Đặt <strong><code>k / 2</code> phần tử cuối cùng</strong> thành <code>1</code> (bán).</li>
</ul>

<p><strong>Lợi nhuận</strong> được định nghĩa là <strong>tổng</strong> của <code>strategy[i] * prices[i]</code> trên tất cả các ngày.</p>

<p>Hãy trả về <strong>lợi nhuận lớn nhất</strong> có thể đạt được.</p>

<p><strong>Lưu ý:</strong> Không có ràng buộc về ngân sách hay số cổ phiếu đang sở hữu, vì vậy mọi thao tác mua và bán đều có thể thực hiện bất kể các thao tác trước đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [4,2,8], strategy = [-1,0,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Sửa đổi</th>
			<th style="border: 1px solid black;">Strategy</th>
			<th style="border: 1px solid black;">Tính lợi nhuận</th>
			<th style="border: 1px solid black;">Lợi nhuận</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">Ban đầu</td>
			<td style="border: 1px solid black;">[-1, 0, 1]</td>
			<td style="border: 1px solid black;">(-1 &times; 4) + (0 &times; 2) + (1 &times; 8) = -4 + 0 + 8</td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Sửa đổi [0, 1]</td>
			<td style="border: 1px solid black;">[0, 1, 1]</td>
			<td style="border: 1px solid black;">(0 &times; 4) + (1 &times; 2) + (1 &times; 8) = 0 + 2 + 8</td>
			<td style="border: 1px solid black;">10</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Sửa đổi [1, 2]</td>
			<td style="border: 1px solid black;">[-1, 0, 1]</td>
			<td style="border: 1px solid black;">(-1 &times; 4) + (0 &times; 2) + (1 &times; 8) = -4 + 0 + 8</td>
			<td style="border: 1px solid black;">4</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, lợi nhuận lớn nhất có thể đạt được là 10, bằng cách sửa đổi mảng con <code>[0, 1]</code>​​​​​​​.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">prices = [5,4,3], strategy = [1,1,0], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<div class="example-block">
<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Sửa đổi</th>
			<th style="border: 1px solid black;">Strategy</th>
			<th style="border: 1px solid black;">Tính lợi nhuận</th>
			<th style="border: 1px solid black;">Lợi nhuận</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">Ban đầu</td>
			<td style="border: 1px solid black;">[1, 1, 0]</td>
			<td style="border: 1px solid black;">(1 &times; 5) + (1 &times; 4) + (0 &times; 3) = 5 + 4 + 0</td>
			<td style="border: 1px solid black;">9</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Sửa đổi [0, 1]</td>
			<td style="border: 1px solid black;">[0, 1, 0]</td>
			<td style="border: 1px solid black;">(0 &times; 5) + (1 &times; 4) + (0 &times; 3) = 0 + 4 + 0</td>
			<td style="border: 1px solid black;">4</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">Sửa đổi [1, 2]</td>
			<td style="border: 1px solid black;">[1, 0, 1]</td>
			<td style="border: 1px solid black;">(1 &times; 5) + (0 &times; 4) + (1 &times; 3) = 5 + 0 + 3</td>
			<td style="border: 1px solid black;">8</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, lợi nhuận lớn nhất có thể đạt được là 9, và đạt được mà không cần sửa đổi.</p>
</div>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= prices.length == strategy.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>-1 &lt;= strategy[i] &lt;= 1</code></li>
	<li><code>2 &lt;= k &lt;= prices.length</code></li>
	<li><code>k</code> là số chẵn</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> Lợi nhuận cơ sở là $\sum \textit{prices}[i]\cdot\textit{strategy}[i]$. Một lần sửa đổi sẽ viết lại một cửa sổ độ dài $k$ thành $k/2$ số 0 theo sau bởi $k/2$ số 1. Tính lại từ đầu cho từng cửa sổ sẽ quá chậm.
>
> Gọi $s$ là prefix của lợi nhuận từ strategy và $t$ là prefix của prices. Khi sửa đổi $[i-k,i)$, ta trừ lợi nhuận cũ của cửa sổ rồi cộng thêm $k/2$ giá ở cuối cửa sổ.
>
> Với mỗi điểm kết thúc phải $i\ge k$, cập nhật bằng $s[n]-(s[i]-s[i-k])+(t[i]-t[i-k/2])$. Lợi nhuận khi không sửa đổi là $s[n]$.

<!-- thinking:end -->

Ta dùng một mảng $\textit{s}$ để biểu diễn tổng tiền tố, trong đó $\textit{s}[i]$ là tổng lợi nhuận của $i$ ngày đầu tiên, tức là $\textit{s}[i] = \sum_{j=0}^{i-1} \textit{prices}[j] \times \textit{strategy}[j]$. Ta cũng dùng một mảng $\textit{t}$ để biểu diễn tổng tiền tố của giá cổ phiếu, trong đó $\textit{t}[i] = \sum_{j=0}^{i-1} \textit{prices}[j]$.

Ban đầu, lợi nhuận lớn nhất là $\textit{s}[n]$. Ta liệt kê điểm kết thúc $i$ của mảng con cần sửa đổi, với điểm bắt đầu là $i-k$. Sau khi sửa đổi, $k/2$ ngày đầu tiên của mảng con có strategy bằng $0$, còn $k/2$ ngày cuối có strategy bằng $1$, nên thay đổi lợi nhuận là:

$$\Delta = -(\textit{s}[i] - \textit{s}[i-k]) + (\textit{t}[i] - \textit{t}[i-k/2])$$

Do đó, ta có thể cập nhật lợi nhuận lớn nhất bằng cách liệt kê mọi $i$ có thể.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProfit(self, prices: List[int], strategy: List[int], k: int) -> int:
        n = len(prices)
        s = [0] * (n + 1)
        t = [0] * (n + 1)
        for i, (a, b) in enumerate(zip(prices, strategy), 1):
            s[i] = s[i - 1] + a * b
            t[i] = t[i - 1] + a
        ans = s[n]
        for i in range(k, n + 1):
            ans = max(ans, s[n] - (s[i] - s[i - k]) + t[i] - t[i - k // 2])
        return ans
```

#### Java

```java
class Solution {
    public long maxProfit(int[] prices, int[] strategy, int k) {
        int n = prices.length;
        long[] s = new long[n + 1];
        long[] t = new long[n + 1];
        for (int i = 1; i <= n; i++) {
            int a = prices[i - 1];
            int b = strategy[i - 1];
            s[i] = s[i - 1] + a * b;
            t[i] = t[i - 1] + a;
        }
        long ans = s[n];
        for (int i = k; i <= n; i++) {
            ans = Math.max(ans, s[n] - (s[i] - s[i - k]) + (t[i] - t[i - k / 2]));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxProfit(vector<int>& prices, vector<int>& strategy, int k) {
        int n = prices.size();
        vector<long long> s(n + 1), t(n + 1);
        for (int i = 1; i <= n; i++) {
            int a = prices[i - 1];
            int b = strategy[i - 1];
            s[i] = s[i - 1] + a * b;
            t[i] = t[i - 1] + a;
        }
        long long ans = s[n];
        for (int i = k; i <= n; i++) {
            ans = max(ans, s[n] - (s[i] - s[i - k]) + (t[i] - t[i - k / 2]));
        }
        return ans;
    }
};
```

#### Go

```go
func maxProfit(prices []int, strategy []int, k int) int64 {
	n := len(prices)
	s := make([]int64, n+1)
	t := make([]int64, n+1)

	for i := 1; i <= n; i++ {
		a := prices[i-1]
		b := strategy[i-1]
		s[i] = s[i-1] + int64(a*b)
		t[i] = t[i-1] + int64(a)
	}

	ans := s[n]
	for i := k; i <= n; i++ {
		ans = max(ans, s[n]-(s[i]-s[i-k])+(t[i]-t[i-k/2]))
	}
	return ans
}
```

#### TypeScript

```ts
function maxProfit(prices: number[], strategy: number[], k: number): number {
    const n = prices.length;
    const s: number[] = Array(n + 1).fill(0);
    const t: number[] = Array(n + 1).fill(0);

    for (let i = 1; i <= n; i++) {
        const a = prices[i - 1];
        const b = strategy[i - 1];
        s[i] = s[i - 1] + a * b;
        t[i] = t[i - 1] + a;
    }

    let ans = s[n];
    for (let i = k; i <= n; i++) {
        const val = s[n] - (s[i] - s[i - k]) + (t[i] - t[i - Math.floor(k / 2)]);
        ans = Math.max(ans, val);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_profit(prices: Vec<i32>, strategy: Vec<i32>, k: i32) -> i64 {
        let n: usize = prices.len();
        let k: usize = k as usize;

        let mut s: Vec<i64> = vec![0; n + 1];
        let mut t: Vec<i64> = vec![0; n + 1];

        for i in 1..=n {
            let a: i64 = prices[i - 1] as i64;
            let b: i64 = strategy[i - 1] as i64;
            s[i] = s[i - 1] + a * b;
            t[i] = t[i - 1] + a;
        }

        let mut ans: i64 = s[n];
        for i in k..=n {
            let cur = s[n] - (s[i] - s[i - k]) + (t[i] - t[i - k / 2]);
            if cur > ans {
                ans = cur;
            }
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public long MaxProfit(int[] prices, int[] strategy, int k) {
        int n = prices.Length;
        long[] s = new long[n + 1];
        long[] t = new long[n + 1];

        for (int i = 1; i <= n; i++) {
            long a = prices[i - 1];
            long b = strategy[i - 1];
            s[i] = s[i - 1] + a * b;
            t[i] = t[i - 1] + a;
        }

        long ans = s[n];
        for (int i = k; i <= n; i++) {
            long cur = s[n] - (s[i] - s[i - k]) + (t[i] - t[i - k / 2]);
            if (cur > ans) {
                ans = cur;
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
