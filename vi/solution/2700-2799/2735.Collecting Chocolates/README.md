---
comments: true
difficulty: Medium
rating: 2043
source: Weekly Contest 349 Q3
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [2735. Collecting Chocolates](https://leetcode.com/problems/collecting-chocolates)

[中文文档](/solution/2700-2799/2735.Collecting%20Chocolates/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có kích thước <code>n</code>, biểu diễn chi phí thu thập các loại chocolate khác nhau. Chi phí thu thập chocolate tại chỉ số <code>i</code>&nbsp;là <code>nums[i]</code>. Mỗi chocolate thuộc một loại khác nhau; ban đầu, chocolate tại chỉ số&nbsp;<code>i</code>&nbsp;thuộc loại thứ <code>i<sup>th</sup></code>.</p>

<p>Trong một thao tác, bạn có thể thực hiện việc sau với <strong>chi phí</strong> là <code>x</code>:</p>

<ul>
	<li>Đồng thời chuyển chocolate thuộc loại <code>i<sup>th</sup></code> thành loại <code>((i + 1) mod n)<sup>th</sup></code> đối với tất cả chocolate.</li>
</ul>

<p>Trả về <em>chi phí nhỏ nhất để thu thập chocolate thuộc tất cả các loại, với điều kiện bạn có thể thực hiện số thao tác tùy ý.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [20,1,15], x = 5
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Ban đầu, các loại chocolate là [0,1,2]. Chúng ta sẽ mua chocolate loại 1<sup>st</sup> với chi phí là 1.
Sau đó, chúng ta thực hiện thao tác với chi phí là 5, các loại chocolate trở thành [1,2,0]. Chúng ta sẽ mua chocolate loại 2<sup>nd</sup><sup> </sup> với chi phí là 1.
Tiếp theo, chúng ta lại thực hiện thao tác với chi phí là 5, các loại chocolate trở thành [2,0,1]. Chúng ta sẽ mua chocolate loại 0<sup>th </sup> với chi phí là 1.
Vậy tổng chi phí là (1 + 5 + 1 + 5 + 1) = 13. Có thể chứng minh rằng đây là phương án tối ưu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], x = 4
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Chúng ta sẽ thu thập cả ba loại chocolate với giá ban đầu của chúng mà không thực hiện thao tác nào. Vì vậy, tổng chi phí là 1 + 2 + 3 = 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= x &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí của chocolate loại $i$ là giá trị nhỏ nhất xuất hiện tại vị trí đó sau một số lần dịch vòng sang trái; mỗi lần dịch tốn $x$. Việc tìm số lần dịch riêng cho từng loại bị ràng buộc lẫn nhau nên không khả thi.
>
> Số lần dịch $j$ là dùng chung cho mọi loại, và khi $j\ge n$ thì giá mua không thể giảm thêm. Gọi $f[i][j]$ là mức giá rẻ nhất của loại $i$ sau nhiều nhất $j$ lần dịch, khi đó ta cần tối thiểu hóa $\sum_i f[i][j]+x\cdot j$ theo $j$.

<!-- thinking:end -->

Ta xét việc liệt kê số lần thực hiện thao tác, và định nghĩa $f[i][j]$ là chi phí nhỏ nhất sau khi chocolate thứ $i$ đã trải qua $j$ thao tác.

Với chocolate thứ $i$:

- Nếu $j = 0$, tức là không thực hiện thao tác nào, thì $f[i][j] = nums[i]$.
- Nếu $0 < j \leq n-1$, chi phí nhỏ nhất của nó là giá trị nhỏ nhất trong khoảng chỉ số $[i,.. (i - j + n) \bmod n]$, tức là $f[i][j] = \min\{nums[i], nums[i - 1], \cdots, nums[(i - j + n) \bmod n]\}$, hoặc có thể viết là $f[i][j] = \min\{f[i][j - 1], nums[(i - j + n) \bmod n]\}$.
- Nếu $j \ge n$, vì khi $j = n - 1$ ta đã xét tất cả chi phí nhỏ nhất, nên nếu tiếp tục tăng $j$, chi phí nhỏ nhất sẽ không thay đổi, trong khi số lần thao tác tăng sẽ làm tổng chi phí tăng. Do đó, ta không cần xét trường hợp $j \ge n$.

Tóm lại, ta có công thức chuyển trạng thái:

$$
f[i][j] =
\begin{cases}
nums[i] ,& j = 0 \\
\min(f[i][j - 1], nums[(i - j + n) \bmod n]) ,& 0 \lt j \leq n - 1
\end{cases}
$$

Cuối cùng, ta chỉ cần liệt kê số lần thao tác $j$, tính tổng chi phí tương ứng với mỗi số lần thao tác, rồi lấy giá trị nhỏ nhất. Cụ thể, đáp án là $\min\limits_{0 \leq j \leq n - 1} \sum\limits_{i = 0}^{n - 1} f[i][j] + x \times j$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$. Trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, nums: List[int], x: int) -> int:
        n = len(nums)
        f = [[0] * n for _ in range(n)]
        for i, v in enumerate(nums):
            f[i][0] = v
            for j in range(1, n):
                f[i][j] = min(f[i][j - 1], nums[(i - j) % n])
        return min(sum(f[i][j] for i in range(n)) + x * j for j in range(n))
```

#### Java

```java
class Solution {
    public long minCost(int[] nums, int x) {
        int n = nums.length;
        int[][] f = new int[n][n];
        for (int i = 0; i < n; ++i) {
            f[i][0] = nums[i];
            for (int j = 1; j < n; ++j) {
                f[i][j] = Math.min(f[i][j - 1], nums[(i - j + n) % n]);
            }
        }
        long ans = 1L << 60;
        for (int j = 0; j < n; ++j) {
            long cost = 1L * x * j;
            for (int i = 0; i < n; ++i) {
                cost += f[i][j];
            }
            ans = Math.min(ans, cost);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minCost(vector<int>& nums, int x) {
        int n = nums.size();
        int f[n][n];
        for (int i = 0; i < n; ++i) {
            f[i][0] = nums[i];
            for (int j = 1; j < n; ++j) {
                f[i][j] = min(f[i][j - 1], nums[(i - j + n) % n]);
            }
        }
        long long ans = 1LL << 60;
        for (int j = 0; j < n; ++j) {
            long long cost = 1LL * x * j;
            for (int i = 0; i < n; ++i) {
                cost += f[i][j];
            }
            ans = min(ans, cost);
        }
        return ans;
    }
};
```

#### Go

```go
func minCost(nums []int, x int) int64 {
	n := len(nums)
	f := make([][]int, n)
	for i, v := range nums {
		f[i] = make([]int, n)
		f[i][0] = v
		for j := 1; j < n; j++ {
			f[i][j] = min(f[i][j-1], nums[(i-j+n)%n])
		}
	}
	ans := 1 << 60
	for j := 0; j < n; j++ {
		cost := x * j
		for i := 0; i < n; i++ {
			cost += f[i][j]
		}
		ans = min(ans, cost)
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function minCost(nums: number[], x: number): number {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    for (let i = 0; i < n; ++i) {
        f[i][0] = nums[i];
        for (let j = 1; j < n; ++j) {
            f[i][j] = Math.min(f[i][j - 1], nums[(i - j + n) % n]);
        }
    }
    let ans = Infinity;
    for (let j = 0; j < n; ++j) {
        let cost = x * j;
        for (let i = 0; i < n; ++i) {
            cost += f[i][j];
        }
        ans = Math.min(ans, cost);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_cost(nums: Vec<i32>, x: i32) -> i64 {
        let n = nums.len();
        let mut f = vec![vec![0; n]; n];
        for i in 0..n {
            f[i][0] = nums[i];
            for j in 1..n {
                f[i][j] = f[i][j - 1].min(nums[(i - j + n) % n]);
            }
        }
        let mut ans = i64::MAX;
        for j in 0..n {
            let mut cost = (x as i64) * (j as i64);
            for i in 0..n {
                cost += f[i][j] as i64;
            }
            ans = ans.min(cost);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
