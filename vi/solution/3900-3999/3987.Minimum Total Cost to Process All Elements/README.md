---
comments: true
difficulty: Medium
rating: 1549
source: Weekly Contest 510 Q2
tags:
    - Array
    - Math
    - Simulation
---

<!-- problem:start -->

# [3987. Minimum Total Cost to Process All Elements](https://leetcode.com/problems/minimum-total-cost-to-process-all-elements)

[中文文档](/solution/3900-3999/3987.Minimum%20Total%20Cost%20to%20Process%20All%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Ban đầu, bạn có <code>k</code> đơn vị tài nguyên.</p>

<p>Bạn phải xử lý các phần tử của <code>nums</code> từ trái sang phải. Để xử lý phần tử thứ <code>i<sup>th</sup></code>, bạn cần <code>nums[i]</code> tài nguyên.</p>

<p>Nếu số tài nguyên hiện có ít hơn <code>nums[i]</code>, bạn có thể thực hiện một thao tác làm tăng số tài nguyên hiện có thêm <code>k</code>. Giá trị của <code>k</code> là cố định và không thay đổi trong suốt quá trình. Thao tác đầu tiên có chi phí là 1, thao tác thứ hai có chi phí là 2, và cứ tiếp tục như vậy.</p>

<p>Sau khi xử lý phần tử thứ <code>i<sup>th</sup></code>, số tài nguyên hiện có giảm đi <code>nums[i]</code>.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng chi phí nhỏ nhất</strong> cần để xử lý tất cả phần tử. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sau khi xử lý <code>nums[0]</code>, ta còn <code>4 - 1 = 3</code> đơn vị tài nguyên.</li>
	<li>Sau khi xử lý <code>nums[1]</code>, ta còn <code>3 - 2 = 1</code> đơn vị tài nguyên.</li>
	<li>Vì <code>nums[2] = 3</code> nhưng chỉ còn 1 đơn vị tài nguyên, ta thực hiện thao tác đầu tiên với chi phí 1. Sau khi xử lý <code>nums[2]</code>, ta còn <code>1 + 4 - 3 = 2</code> đơn vị tài nguyên.</li>
	<li>Vì <code>nums[3] = 4</code> nhưng chỉ còn 2 đơn vị tài nguyên, ta thực hiện thao tác thứ hai với chi phí 2, có <code>2 + 4 = 6</code> đơn vị tài nguyên, đủ để xử lý <code>nums[3]</code>.</li>
	<li>Do đó, tổng chi phí là <code>1 + 2 = 3</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,7,14], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sau khi xử lý <code>nums[0]</code>, ta còn <code>4 - 1 = 3</code> đơn vị tài nguyên.</li>
	<li>Sau khi xử lý <code>nums[1]</code>, ta còn <code>3 - 1 = 2</code> đơn vị tài nguyên.</li>
	<li>Vì <code>nums[2] = 7</code> nhưng chỉ còn 2 đơn vị tài nguyên, ta thực hiện hai thao tác với chi phí <code>1 + 2 = 3</code>. Sau khi xử lý <code>nums[2]</code>, ta còn <code>2 + 4 + 4 - 7 = 3</code> đơn vị tài nguyên.</li>
	<li>Vì <code>nums[3] = 14</code> nhưng chỉ còn 3 đơn vị tài nguyên, ta thực hiện ba thao tác với chi phí <code>3 + 4 + 5 = 12</code>, có <code>3 + 4 + 4 + 4 = 15</code> đơn vị tài nguyên, đủ để xử lý <code>nums[3]</code>.</li>
	<li>Do đó, tổng chi phí là <code>3 + 12 = 15</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4], k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Để xử lý tất cả phần tử, ta có thể dùng 10 đơn vị tài nguyên ban đầu mà không cần thực hiện thao tác nào. Vì vậy, tổng chi phí cần thiết là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Thao tác thứ $i$ có chi phí $i$, nên tổng chi phí là một số tam giác và ta chỉ cần tối thiểu hóa số thao tác. Mỗi thao tác cộng thêm $k$ tài nguyên; khi $x$ lớn hơn $\textit{cur}$, ta cần thêm $\lceil(x-\textit{cur})/k\rceil$ thao tác.
>
> Duyệt từ trái sang phải, duy trì $\textit{cur}$ và $\textit{cnt}$, bổ sung tài nguyên trước khi trừ $x$. Cuối cùng tính $\textit{cnt}(\textit{cnt}+1)/2$ theo modulo $10^9+7$.
>
> Vì $n\le 10^5$, một lần mô phỏng tuyến tính là đủ.

<!-- thinking:end -->

Thao tác thứ $i$ có chi phí $i$, nên nếu tổng cộng thực hiện $\textit{cnt}$ thao tác thì tổng chi phí là $1 + 2 + \cdots + \textit{cnt} = \dfrac{\textit{cnt}(\textit{cnt}+1)}{2}$. Tối thiểu hóa tổng chi phí tương đương với tối thiểu hóa số thao tác.

Mô phỏng quá trình từ trái sang phải. Duy trì số tài nguyên hiện có $\textit{cur}$ (ban đầu là $k$) và số thao tác đã thực hiện $\textit{cnt}$. Khi xử lý một phần tử $x$:

- Nếu $\textit{cur} \ge x$, chỉ cần trừ $x$;
- Ngược lại, thực hiện thêm $m = \left\lceil\dfrac{x - \textit{cur}}{k}\right\rceil$ thao tác để tăng tài nguyên thêm $m \times k$, rồi trừ $x$.

Sau khi duyệt xong, tính số tam giác của $\textit{cnt}$ và lấy kết quả modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(self, nums: list[int], k: int) -> int:
        cnt = 0
        cur = k
        mod = 10**9 + 7
        for x in nums:
            diff = x - cur
            if diff > 0:
                m = (diff + k - 1) // k
                cur += m * k
                cnt += m
            cur -= x
        cnt %= mod
        return (1 + cnt) * cnt // 2 % mod
```

#### Java

```java
class Solution {
    public int minimumCost(int[] nums, int k) {
        final int MOD = 1_000_000_007;
        long cnt = 0;
        long cur = k;

        for (int x : nums) {
            long diff = (long) x - cur;
            if (diff > 0) {
                long m = (diff + k - 1L) / k;
                cur += m * (long) k;
                cnt += m;
            }
            cur -= x;
        }

        cnt %= MOD;
        return (int) ((cnt + 1) * cnt / 2 % MOD);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCost(vector<int>& nums, int k) {
        const int MOD = 1'000'000'007;
        long long cnt = 0;
        long long cur = k;

        for (int x : nums) {
            long long diff = (long long) x - cur;
            if (diff > 0) {
                long long m = (diff + k - 1LL) / k;
                cur += m * k;
                cnt += m;
            }
            cur -= x;
        }

        cnt %= MOD;
        return (cnt + 1) * cnt / 2 % MOD;
    }
};
```

#### Go

```go
func minimumCost(nums []int, k int) int {
	const mod int64 = 1_000_000_007
	var cnt int64
	cur := int64(k)

	for _, x := range nums {
		diff := int64(x) - cur
		if diff > 0 {
			m := (diff + int64(k) - 1) / int64(k)
			cur += m * int64(k)
			cnt += m
		}
		cur -= int64(x)
	}

	cnt %= mod
	return int((cnt + 1) * cnt / 2 % mod)
}
```

#### TypeScript

```ts
function minimumCost(nums: number[], k: number): number {
    const MOD = 1000000007n;
    let cnt = 0n;
    let cur = BigInt(k);
    const K = BigInt(k);

    for (const x of nums) {
        const diff = BigInt(x) - cur;
        if (diff > 0n) {
            const m = (diff + K - 1n) / K;
            cur += m * K;
            cnt += m;
        }
        cur -= BigInt(x);
    }

    cnt %= MOD;
    return Number((((cnt + 1n) * cnt) / 2n) % MOD);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
