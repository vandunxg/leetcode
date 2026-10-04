---
comments: true
difficulty: Hard
rating: 2644
source: Weekly Contest 433 Q4
tags:
    - Stack
    - Array
    - Math
    - Monotonic Stack
---

<!-- problem:start -->

# [3430. Maximum and Minimum Sums of at Most Size K Subarrays](https://leetcode.com/problems/maximum-and-minimum-sums-of-at-most-size-k-subarrays)

[中文文档](/solution/3400-3499/3430.Maximum%20and%20Minimum%20Sums%20of%20at%20Most%20Size%20K%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <strong>dương</strong> <code>k</code>. Hãy trả về tổng của các phần tử <strong>lớn nhất</strong> và <strong>nhỏ nhất</strong> của tất cả các <span data-keyword="subarray-nonempty">mảng con</span> có <strong>nhiều nhất</strong> <code>k</code> phần tử.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con của <code>nums</code> có nhiều nhất 2 phần tử là:</p>

<table style="border: 1px solid black;">
    <tbody>
        <tr>
            <th style="border: 1px solid black;"><b>Mảng con</b></th>
            <th style="border: 1px solid black;">Nhỏ nhất</th>
            <th style="border: 1px solid black;">Lớn nhất</th>
            <th style="border: 1px solid black;">Tổng</th>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[1]</code></td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">2</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[2]</code></td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">4</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[3]</code></td>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;">6</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[1, 2]</code></td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">3</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[2, 3]</code></td>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;">5</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><strong>Tổng cuối cùng</strong></td>
            <td style="border: 1px solid black;">&nbsp;</td>
            <td style="border: 1px solid black;">&nbsp;</td>
            <td style="border: 1px solid black;">20</td>
        </tr>
    </tbody>
</table>

<p>Kết quả là 20.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-3,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con của <code>nums</code> có nhiều nhất 2 phần tử là:</p>

<table style="border: 1px solid black;">
    <tbody>
        <tr>
            <th style="border: 1px solid black;"><b>Mảng con</b></th>
            <th style="border: 1px solid black;">Nhỏ nhất</th>
            <th style="border: 1px solid black;">Lớn nhất</th>
            <th style="border: 1px solid black;">Tổng</th>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[1]</code></td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">2</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[-3]</code></td>
            <td style="border: 1px solid black;">-3</td>
            <td style="border: 1px solid black;">-3</td>
            <td style="border: 1px solid black;">-6</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[1]</code></td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">2</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[1, -3]</code></td>
            <td style="border: 1px solid black;">-3</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">-2</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><code>[-3, 1]</code></td>
            <td style="border: 1px solid black;">-3</td>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;">-2</td>
        </tr>
        <tr>
            <td style="border: 1px solid black;"><strong>Tổng cuối cùng</strong></td>
            <td style="border: 1px solid black;">&nbsp;</td>
            <td style="border: 1px solid black;">&nbsp;</td>
            <td style="border: 1px solid black;">-6</td>
        </tr>
    </tbody>
</table>

<p>Kết quả là -6.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 80000</code></li>
    <li><code>1 &lt;= k &lt;= nums.length</code></li>
    <li><code>-10<sup>6</sup> &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack (Đếm đóng góp)

<!-- thinking:start -->

> **Tư duy**
>
> Không giống phiên bản dãy con, ở đây ta tính tổng giá trị lớn nhất và nhỏ nhất của các mảng con liên tiếp có độ dài nhiều nhất $k$. Việc tính lại các cực trị cho từng mảng con sẽ quá chậm.
>
> Với điểm kết thúc $i$, giá trị lớn nhất (nhỏ nhất) trên các điểm bắt đầu hợp lệ sẽ chuyển số lần xuất hiện của nó cho phần tử mới khi monotonic stack thực hiện thao tác pop.
>
> Ta duy trì các monotonic stack với trường $\textit{shares}$, đồng thời trừ phần đóng góp đã hết hạn khi điểm bắt đầu của cửa sổ vượt qua $i-k$. Cộng $\textit{MaxSum}_i+\textit{MinSum}_i$ trên mọi $i$ sẽ cho đáp án.

<!-- thinking:end -->

Mục tiêu là tính tổng $S = \sum_i (\text{MaxSum}_i + \text{MinSum}_i)$, trong đó:

1.  $\text{MaxSum}_i$: tổng các giá trị lớn nhất của mọi mảng con hợp lệ kết thúc tại chỉ số $i$.
2.  $\text{MinSum}_i$: tổng các giá trị nhỏ nhất của mọi mảng con hợp lệ kết thúc tại chỉ số $i$.

Ta duy trì hai **Deque**, được sử dụng như các monotonic stack có kiểm soát biên: `max_stack` và `min_stack`.

Các deque này theo dõi giá trị lớn nhất và nhỏ nhất của mọi mảng con kết thúc tại chỉ số $i$ với độ dài không vượt quá $k$.

Mỗi phần tử trong deque là một bộ ba: `[index, value, count]`,
trong đó `count` biểu thị số lần `value` đóng vai trò là max/min trong cửa sổ hiện tại.

#### Bước 1: Kiểm soát biên

Với mỗi phần tử $nums[i]$, kiểm tra xem biên cửa sổ $\max(0, i - k + 1)$ đã dịch chuyển chưa.

- Nếu $i - k \ge 0$, điều đó có nghĩa là mảng con $nums[i-k \dots i]$ sẽ vượt quá độ dài $k$ do thêm $nums[i]$.
- Phải loại bỏ đóng góp của mảng con bắt đầu tại $i-k$. Đóng góp này nằm ở **đầu** của các deque.
- Giảm `count` ở đầu các deque. Nếu chỉ số của phần tử đầu nằm ngoài phạm vi cửa sổ, ta gọi `popleft()` để duy trì hiệu năng.

#### Bước 2: Tính đơn điệu

Lấy `max_stack` làm ví dụ:

- Trong khi `max_stack` không rỗng và `max_stack` ở đỉnh có giá trị $\leq nums[i]$ (1):
    - $nums[i]$ hiện tại sẽ thay thế phần tử ở đỉnh thành giá trị lớn nhất mới cho mọi mảng con mà phần tử ở đỉnh đó từng "phục vụ".
    - Ta tiếp nhận `prev_shares` (chính là count) từ phần tử vừa pop.
    - Mức tăng ròng của `subarrays_max_sum` được tính là $(nums[i] - prev\_num) \times prev\_shares$.
- Sau vòng lặp while, thêm đóng góp riêng của $nums[i]$, tương ứng với mảng con chỉ gồm một phần tử, vào `subarrays_max_sum`,
  rồi đưa $nums[i]$ vào `max_stack` cùng với `count` tích lũy và chỉ số của nó.

Logic xử lý `min_stack` tương tự, nhưng bất đẳng thức (1) phải đổi thành giá trị ở đỉnh `min_stack` $\geq nums[i]$.

#### Bước 3: Tích lũy

Ở cuối mỗi lần lặp $i$, cộng `subarrays_max_sum` hiện tại và `subarrays_min_sum` vào tổng toàn cục `subarrays_max_min_sum`.

### Phân tích độ phức tạp

- **Độ phức tạp thời gian:** $O(n)$, trong đó $n$ là độ dài của $nums$. Mỗi phần tử được push và pop nhiều nhất 4 lần trong tổng số hai deque.
- **Độ phức tạp không gian:** $O(n)$ để lưu các deque và các biến trạng thái.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMaxSubarraySum(self, nums: list[int], k: int) -> int:
        subarrays_max_min_sum = 0

        max_stack: deque[list[int]] = deque([])  # Format: [idx, num, shares].
        subarrays_max_sum = 0

        min_stack: deque[list[int]] = deque([])  # Format: [idx, num, shares].
        subarrays_min_sum = 0

        for end_idx, num in enumerate(nums):
            start_idx = max(0, end_idx - k + 1)

            # Window start idx slides by 1: must update stacks' info.
            if start_idx > 0:
                max_stack[0][2] -= 1  # Decrement stack's front num shares.
                subarrays_max_sum -= max_stack[0][1]

                if max_stack[0][0] < start_idx:  # Front num out of window.
                    max_stack.popleft()

                min_stack[0][2] -= 1  # Decrement stack's front num shares.
                subarrays_min_sum -= min_stack[0][1]

                if min_stack[0][0] < start_idx:  # Front num out of window.
                    min_stack.popleft()

            max_shares = 1  # Base case.
            subarrays_max_sum += num

            while max_stack and max_stack[-1][1] <= num:
                _, prev_num, prev_shares = max_stack.pop()

                max_shares += prev_shares  # Max shares transition.

                # Reflect transition in max sum.
                subarrays_max_sum += (num - prev_num) * prev_shares

            max_stack.append([end_idx, num, max_shares])

            min_shares = 1  # Base case.
            subarrays_min_sum += num

            while min_stack and min_stack[-1][1] >= num:
                _, prev_num, prev_shares = min_stack.pop()

                min_shares += prev_shares  # Min shares transition.

                # Reflect transition in min sum.
                subarrays_min_sum += (num - prev_num) * prev_shares

            min_stack.append([end_idx, num, min_shares])

            subarrays_max_min_sum += subarrays_max_sum + subarrays_min_sum

        return subarrays_max_min_sum
```

#### C++

```cpp
class Solution {
public:
    long long minMaxSubarraySum(vector<int>& nums, int k) {
        long long totalMaxMinSum = 0;
        long long windowMaxSum = 0, windowMinSum = 0;

        // Format: {idx, num, shares}. Use long long to prevent overflow.
        deque<tuple<int, int, long long>> maxStack, minStack;

        for (int endIdx = 0; endIdx < nums.size(); endIdx++) {
            int startIdx = max(0, endIdx - k + 1);

            // Window start idx slides by 1: must update stacks' info.
            if (startIdx > 0) {
                get<2>(maxStack.front())--; // Decrement stack's front num shares.
                windowMaxSum -= get<1>(maxStack.front());

                // Front num out of window.
                if (get<0>(maxStack.front()) < startIdx)
                    maxStack.pop_front();

                get<2>(minStack.front())--; // Decrement stack's front num shares.
                windowMinSum -= get<1>(minStack.front());

                // Front num out of window.
                if (get<0>(minStack.front()) < startIdx)
                    minStack.pop_front();
            }

            long long num = nums[endIdx];

            long long maxShares = 1; // Base case.
            windowMaxSum += num;

            while (!maxStack.empty() && get<1>(maxStack.back()) <= num) {
                int prevNum = get<1>(maxStack.back());
                long long prevShares = get<2>(maxStack.back());
                maxStack.pop_back();

                maxShares += prevShares; // Max shares transition.

                // Reflect transition in max sum.
                windowMaxSum += (num - prevNum) * prevShares;
            }

            maxStack.push_back({endIdx, num, maxShares});

            long long minShares = 1; // Base case.
            windowMinSum += num;

            while (!minStack.empty() && get<1>(minStack.back()) >= num) {
                int prevNum = get<1>(minStack.back());
                long long prevShares = get<2>(minStack.back());
                minStack.pop_back();

                minShares += prevShares; // Min shares transition.

                // Reflect transition in min sum.
                windowMinSum += (num - prevNum) * prevShares;
            }

            minStack.push_back({endIdx, num, minShares});

            totalMaxMinSum += windowMaxSum + windowMinSum;
        }

        return totalMaxMinSum;
    }
};
```

#### Java

```java
class Solution {
    public long minMaxSubarraySum(int[] nums, int k) {
        long total = 0, windowMax = 0, windowMin = 0;
        Deque<long[]> maxStack = new ArrayDeque<>();
        Deque<long[]> minStack = new ArrayDeque<>();
        for (int end = 0; end < nums.length; ++end) {
            int start = Math.max(0, end - k + 1);
            if (start > 0) {
                maxStack.peekFirst()[2]--;
                windowMax -= maxStack.peekFirst()[1];
                if (maxStack.peekFirst()[0] < start) {
                    maxStack.pollFirst();
                }
                minStack.peekFirst()[2]--;
                windowMin -= minStack.peekFirst()[1];
                if (minStack.peekFirst()[0] < start) {
                    minStack.pollFirst();
                }
            }
            long num = nums[end];
            long maxShares = 1;
            windowMax += num;
            while (!maxStack.isEmpty() && maxStack.peekLast()[1] <= num) {
                long prevNum = maxStack.peekLast()[1];
                long prevShares = maxStack.pollLast()[2];
                maxShares += prevShares;
                windowMax += (num - prevNum) * prevShares;
            }
            maxStack.addLast(new long[] {end, num, maxShares});
            long minShares = 1;
            windowMin += num;
            while (!minStack.isEmpty() && minStack.peekLast()[1] >= num) {
                long prevNum = minStack.peekLast()[1];
                long prevShares = minStack.pollLast()[2];
                minShares += prevShares;
                windowMin += (num - prevNum) * prevShares;
            }
            minStack.addLast(new long[] {end, num, minShares});
            total += windowMax + windowMin;
        }
        return total;
    }
}
```

#### Go

```go
func minMaxSubarraySum(nums []int, k int) int64 {
    var total, windowMax, windowMin int64
    type item struct {
        idx, num int
        shares   int64
    }
    maxStack, minStack := []item{}, []item{}
    for end := 0; end < len(nums); end++ {
        start := end - k + 1
        if start < 0 {
            start = 0
        }
        if start > 0 {
            maxStack[0].shares--
            windowMax -= int64(maxStack[0].num)
            if maxStack[0].idx < start {
                maxStack = maxStack[1:]
            }
            minStack[0].shares--
            windowMin -= int64(minStack[0].num)
            if minStack[0].idx < start {
                minStack = minStack[1:]
            }
        }
        num := int64(nums[end])
        maxShares := int64(1)
        windowMax += num
        for len(maxStack) > 0 && int64(maxStack[len(maxStack)-1].num) <= num {
            prev := maxStack[len(maxStack)-1]
            maxStack = maxStack[:len(maxStack)-1]
            maxShares += prev.shares
            windowMax += (num - int64(prev.num)) * prev.shares
        }
        maxStack = append(maxStack, item{end, nums[end], maxShares})
        minShares := int64(1)
        windowMin += num
        for len(minStack) > 0 && int64(minStack[len(minStack)-1].num) >= num {
            prev := minStack[len(minStack)-1]
            minStack = minStack[:len(minStack)-1]
            minShares += prev.shares
            windowMin += (num - int64(prev.num)) * prev.shares
        }
        minStack = append(minStack, item{end, nums[end], minShares})
        total += windowMax + windowMin
    }
    return total
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var minMaxSubarraySum = function (nums, k) {
    const computeSum = (nums, k, isMin) => {
        const n = nums.length;
        const prev = Array(n).fill(-1);
        const next = Array(n).fill(n);
        let stk = [];

        if (isMin) {
            for (let i = 0; i < n; i++) {
                while (stk.length > 0 && nums[stk[stk.length - 1]] >= nums[i]) {
                    stk.pop();
                }
                prev[i] = stk.length > 0 ? stk[stk.length - 1] : -1;
                stk.push(i);
            }
            stk = [];
            for (let i = n - 1; i >= 0; i--) {
                while (stk.length > 0 && nums[stk[stk.length - 1]] > nums[i]) {
                    stk.pop();
                }
                next[i] = stk.length > 0 ? stk[stk.length - 1] : n;
                stk.push(i);
            }
        } else {
            for (let i = 0; i < n; i++) {
                while (stk.length > 0 && nums[stk[stk.length - 1]] <= nums[i]) {
                    stk.pop();
                }
                prev[i] = stk.length > 0 ? stk[stk.length - 1] : -1;
                stk.push(i);
            }
            stk = [];
            for (let i = n - 1; i >= 0; i--) {
                while (stk.length > 0 && nums[stk[stk.length - 1]] < nums[i]) {
                    stk.pop();
                }
                next[i] = stk.length > 0 ? stk[stk.length - 1] : n;
                stk.push(i);
            }
        }

        let totalSum = 0;
        for (let i = 0; i < n; i++) {
            const left = prev[i];
            const right = next[i];
            const a = left + 1;
            const b = i;
            const c = i;
            const d = right - 1;

            let start1 = Math.max(a, i - k + 1);
            let endCandidate1 = d - k + 1;
            let upper1 = Math.min(b, endCandidate1);

            let sum1 = 0;
            if (upper1 >= start1) {
                const termCount = upper1 - start1 + 1;
                const first = start1;
                const last = upper1;
                const indexSum = (last * (last + 1)) / 2 - ((first - 1) * first) / 2;
                const constantSum = (k - i) * termCount;
                sum1 = indexSum + constantSum;
            }

            let start2 = upper1 + 1;
            let end2 = b;
            start2 = Math.max(start2, a);
            end2 = Math.min(end2, b);

            let sum2 = 0;
            if (start2 <= end2) {
                const count = end2 - start2 + 1;
                const term = d - i + 1;
                sum2 = term * count;
            }

            totalSum += nums[i] * (sum1 + sum2);
        }

        return totalSum;
    };

    const minSum = computeSum(nums, k, true);
    const maxSum = computeSum(nums, k, false);
    return minSum + maxSum;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
