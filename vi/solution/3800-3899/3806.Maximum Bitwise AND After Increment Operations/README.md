---
comments: true
difficulty: Hard
rating: 2259
source: Weekly Contest 484 Q4
tags:
    - Greedy
    - Bit Manipulation
    - Array
    - Sorting
---

<!-- problem:start -->

# [3806. Maximum Bitwise AND After Increment Operations](https://leetcode.com/problems/maximum-bitwise-and-after-increment-operations)

[中文文档](/solution/3800-3899/3806.Maximum%20Bitwise%20AND%20After%20Increment%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>k</code> và <code>m</code>.</p>

<p>Bạn có thể thực hiện <strong>tối đa</strong> <code>k</code> thao tác. Trong một thao tác, bạn có thể chọn bất kỳ chỉ số <code>i</code> nào và <strong>tăng</strong> <code>nums[i]</code> lên 1.</p>

<p>Trả về một số nguyên biểu thị <strong>phép AND bitwise</strong> <strong>lớn nhất</strong> có thể có của bất kỳ <strong><span data-keyword="subset">tập con</span></strong> nào có kích thước <code>m</code> sau khi thực hiện tối đa <code>k</code> thao tác một cách tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2], k = 8, m = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta cần một tập con có kích thước <code>m = 2</code>. Chọn các chỉ số <code>[0, 2]</code>.</li>
	<li>Tăng <code>nums[0] = 3</code> lên 6 bằng 3 thao tác, và tăng <code>nums[2] = 2</code> lên 6 bằng 4 thao tác.</li>
	<li>Tổng số thao tác đã sử dụng là 7, không lớn hơn <code>k = 8</code>.</li>
	<li>Hai giá trị được chọn trở thành <code>[6, 6]</code>, và phép AND bitwise của chúng là <code>6</code>, đây là giá trị lớn nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,8,4], k = 7, m = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta cần một tập con có kích thước <code>m = 3</code>. Chọn các chỉ số <code>[0, 1, 3]</code>.</li>
	<li>Tăng <code>nums[0] = 1</code> lên 4 bằng 3 thao tác, tăng <code>nums[1] = 2</code> lên 4 bằng 2 thao tác, và giữ nguyên <code>nums[3] = 4</code>.</li>
	<li>Tổng số thao tác đã sử dụng là 5, không lớn hơn <code>k = 7</code>.</li>
	<li>Ba giá trị được chọn trở thành <code>[4, 4, 4]</code>, và phép AND bitwise của chúng là 4, đây là giá trị lớn nhất có thể.​​​​​​​</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1], k = 3, m = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ta cần một tập con có kích thước <code>m = 2</code>. Chọn các chỉ số <code>[0, 1]</code>.</li>
	<li>Tăng cả hai giá trị từ 1 lên 2, mỗi giá trị cần 1 thao tác.</li>
	<li>Tổng số thao tác đã sử dụng là 2, không lớn hơn <code>k = 3</code>.</li>
	<li>Hai giá trị được chọn trở thành <code>[2, 2]</code>, và phép AND bitwise của chúng là 2, đây là giá trị lớn nhất có thể.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= m &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xây dựng tham lam theo bit + Phép thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Sau tối đa $k$ lần tăng, ta chọn $m$ số để tối đa hóa AND của chúng. Điều kiện $n \le 5 \times 10^4$ loại trừ việc liệt kê các tập con.
>
> AND lớn hơn ưu tiên các bit cao được bật. Ta thử các bit từ cao xuống thấp: với các bit cao hơn đã được chọn, ta kiểm tra xem bit hiện tại có thể bằng $1$ hay không.
>
> Để tăng một giá trị lên ít nhất $\textit{target}$ chỉ cần xử lý bit xung đột đầu tiên và các bit thấp hơn; chi phí là hiệu của các mặt nạ bit thấp.
>
> Với mỗi ứng viên, ta sắp xếp các chi phí và kiểm tra xem tổng $m$ chi phí nhỏ nhất có không vượt quá $k$ hay không. Nếu kiểm tra thành công, ta giữ lại bit đó, từ đó thu được đáp án tham lam theo thứ tự từ bit cao xuống thấp.

<!-- thinking:end -->

Ta duyệt từng bit từ bit cao nhất, thử đưa bit đó vào kết quả AND bitwise cuối cùng. Với kết quả AND bitwise đang được thử là $\textit{target}$, ta tính số thao tác tối thiểu cần thiết để tăng mỗi phần tử trong mảng lên ít nhất $\textit{target}$.

Cụ thể, ta tìm vị trí $j - 1$ tại đó $\textit{target}$ có bit đầu tiên bằng $1$ khi xét từ cao xuống thấp, còn phần tử hiện tại có bit tương ứng bằng $0$. Khi đó, ta chỉ cần tăng phần tử hiện tại đến giá trị của $\textit{target}$ trong $j$ bit thấp. Số thao tác cần thiết là $(\textit{target} \& 2^{j} - 1) - (\textit{nums}[i] \& 2^{j} - 1)$. Ta lưu số thao tác cần thiết của tất cả phần tử trong mảng vào $\textit{cost}$, sắp xếp mảng này, rồi lấy tổng $m$ phần tử đầu tiên. Nếu tổng này không vượt quá $k$, nghĩa là ta có thể đưa bit đó vào kết quả AND bitwise cuối cùng.

Độ phức tạp thời gian là $O(n \times \log n \times \log M)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$ và $M$ là giá trị lớn nhất trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumAND(self, nums: List[int], k: int, m: int) -> int:
        mx = (max(nums) + k).bit_length()
        ans = 0
        cost = [0] * len(nums)
        for bit in range(mx - 1, -1, -1):
            target = ans | (1 << bit)
            for i, x in enumerate(nums):
                j = (target & ~x).bit_length()
                mask = (1 << j) - 1
                cost[i] = (target & mask) - (x & mask)
            cost.sort()
            if sum(cost[:m]) <= k:
                ans = target
        return ans
```

#### Java

```java
class Solution {
    public int maximumAND(int[] nums, int k, int m) {
        int max = 0;
        for (int x : nums) {
            max = Math.max(max, x);
        }
        max += k;

        int mx = 32 - Integer.numberOfLeadingZeros(max);
        int n = nums.length;

        int ans = 0;
        int[] cost = new int[n];

        for (int bit = mx - 1; bit >= 0; bit--) {
            int target = ans | (1 << bit);
            for (int i = 0; i < n; i++) {
                int x = nums[i];
                int diff = target & ~x;
                int j = diff == 0 ? 0 : 32 - Integer.numberOfLeadingZeros(diff);
                int mask = (1 << j) - 1;
                cost[i] = (target & mask) - (x & mask);
            }
            Arrays.sort(cost);
            long sum = 0;
            for (int i = 0; i < m; i++) {
                sum += cost[i];
            }
            if (sum <= k) {
                ans = target;
            }
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumAND(const vector<int>& nums, int k, int m) {
        int max_val = ranges::max(nums) + k;
        int mx = max_val > 0 ? 32 - __builtin_clz(max_val) : 0;

        int ans = 0;
        vector<int> cost(nums.size());

        for (int bit = mx - 1; bit >= 0; bit--) {
            int target = ans | (1 << bit);
            for (size_t i = 0; i < nums.size(); i++) {
                int x = nums[i];
                int diff = target & ~x;
                int j = diff == 0 ? 0 : 32 - __builtin_clz(diff);
                long long mask = (1L << j) - 1;
                cost[i] = (target & mask) - (x & mask);
            }

            ranges::sort(cost);
            long long sum = accumulate(cost.begin(), cost.begin() + m, 0LL);
            if (sum <= k) {
                ans = target;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maximumAND(nums []int, k int, m int) int {
	mx := bits.Len(uint(slices.Max(nums) + k))

	ans := 0
	cost := make([]int, len(nums))

	for bit := mx - 1; bit >= 0; bit-- {
		target := ans | (1 << bit)
		for i, x := range nums {
			j := bits.Len(uint(target & ^x))
			mask := (1 << j) - 1
			cost[i] = (target & mask) - (x & mask)
		}
		sort.Ints(cost)
		sum := 0
		for i := 0; i < m; i++ {
			sum += cost[i]
		}
		if sum <= k {
			ans = target
		}
	}

	return ans
}
```

#### TypeScript

```ts
function maximumAND(nums: number[], k: number, m: number): number {
    const mx = 32 - Math.clz32(Math.max(...nums) + k);

    let ans = 0;
    const n = nums.length;
    const cost = new Array(n);

    for (let bit = mx - 1; bit >= 0; bit--) {
        let target = ans | (1 << bit);
        for (let i = 0; i < n; i++) {
            const x = nums[i];
            const diff = target & ~x;
            const j = diff === 0 ? 0 : 32 - Math.clz32(diff);
            const mask = (1 << j) - 1;
            cost[i] = (target & mask) - (x & mask);
        }
        cost.sort((a, b) => a - b);
        let sum = 0;
        for (let i = 0; i < m; i++) {
            sum += cost[i];
        }
        if (sum <= k) {
            ans = target;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
