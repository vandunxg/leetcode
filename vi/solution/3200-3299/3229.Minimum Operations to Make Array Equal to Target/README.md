---
comments: true
difficulty: Hard
rating: 2066
source: Weekly Contest 407 Q4
tags:
    - Stack
    - Greedy
    - Array
    - Dynamic Programming
    - Monotonic Stack
---

<!-- problem:start -->

# [3229. Minimum Operations to Make Array Equal to Target](https://leetcode.com/problems/minimum-operations-to-make-array-equal-to-target)

[Tài liệu tiếng Trung](/solution/3200-3299/3229.Minimum%20Operations%20to%20Make%20Array%20Equal%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên dương <code>nums</code> và <code>target</code> có cùng độ dài.</p>

<p>Trong một thao tác, bạn có thể chọn bất kỳ mảng con nào của <code>nums</code> và tăng mỗi phần tử trong mảng con đó lên 1 hoặc giảm mỗi phần tử đi 1.</p>

<p>Hãy trả về số thao tác <strong>ít nhất</strong> cần thực hiện để biến <code>nums</code> thành mảng <code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,5,1,2], target = [4,6,2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta thực hiện các thao tác sau để biến <code>nums</code> thành <code>target</code>:<br />
- Tăng <code>nums[0..3]</code> lên 1, <code>nums = [4,6,2,3]</code>.<br />
- Tăng <code>nums[3..3]</code> lên 1, <code>nums = [4,6,2,4]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2], target = [2,1,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta thực hiện các thao tác sau để biến <code>nums</code> thành <code>target</code>:<br />
- Tăng <code>nums[0..0]</code> lên 1, <code>nums = [2,3,2]</code>.<br />
- Giảm <code>nums[1..1]</code> đi 1, <code>nums = [2,2,2]</code>.<br />
- Giảm <code>nums[1..1]</code> đi 1, <code>nums = [2,1,2]</code>.<br />
- Tăng <code>nums[2..2]</code> lên 1, <code>nums = [2,1,3]</code>.<br />
- Tăng <code>nums[2..2]</code> lên 1, <code>nums = [2,1,4]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length == target.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i], target[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác tăng hoặc giảm 1 trên một mảng con để biến $\textit{nums}$ thành $\textit{target}$. Vì $n\le 10^5$, ta không thể thử từng đoạn. Với $d_i=\textit{target}_i-\textit{nums}_i$, một đoạn liên tiếp cùng dấu có thể dùng chung phần tiền tố của các thao tác.
>
> Khi dấu không đổi, ta chỉ cần trả thêm phần tăng tuyệt đối; khi dấu đổi, phải trả lại $|d_i|$ từ đầu. Vì vậy, chỉ cần quét từ trái sang phải và so sánh với hiệu trước đó, với $O(1)$ không gian phụ.

<!-- thinking:end -->

Trước tiên, ta tính hiệu giữa hai mảng $\textit{nums}$ và $\textit{target}$. Với mảng hiệu, ta tìm các đoạn liên tiếp mà các hiệu có cùng dấu. Với mỗi đoạn, ta cộng giá trị tuyệt đối của phần tử đầu tiên vào kết quả. Với các phần tử tiếp theo, nếu giá trị tuyệt đối của hiệu lớn hơn giá trị tuyệt đối của hiệu trước đó, ta cộng phần chênh lệch giữa hai giá trị tuyệt đối vào kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

Bài toán tương tự:

- [1526. Số thao tác tăng tối thiểu trên các mảng con để tạo thành mảng đích](https://github.com/doocs/leetcode/tree/main/solution/1500-1599/1526.Minimum%20Number%20of%20Increments%20on%20Subarrays%20to%20Form%20a%20Target%20Array/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int], target: List[int]) -> int:
        n = len(nums)
        f = abs(target[0] - nums[0])
        for i in range(1, n):
            x = target[i] - nums[i]
            y = target[i - 1] - nums[i - 1]
            if x * y > 0:
                d = abs(x) - abs(y)
                if d > 0:
                    f += d
            else:
                f += abs(x)
        return f
```

#### Java

```java
class Solution {
    public long minimumOperations(int[] nums, int[] target) {
        long f = Math.abs(target[0] - nums[0]);
        for (int i = 1; i < nums.length; ++i) {
            long x = target[i] - nums[i];
            long y = target[i - 1] - nums[i - 1];
            if (x * y > 0) {
                long d = Math.abs(x) - Math.abs(y);
                if (d > 0) {
                    f += d;
                }
            } else {
                f += Math.abs(x);
            }
        }
        return f;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumOperations(vector<int>& nums, vector<int>& target) {
        using ll = long long;
        ll f = abs(target[0] - nums[0]);
        for (int i = 1; i < nums.size(); ++i) {
            long x = target[i] - nums[i];
            long y = target[i - 1] - nums[i - 1];
            if (x * y > 0) {
                ll d = abs(x) - abs(y);
                if (d > 0) {
                    f += d;
                }
            } else {
                f += abs(x);
            }
        }
        return f;
    }
};
```

#### Go

```go
func minimumOperations(nums []int, target []int) int64 {
    f := abs(target[0] - nums[0])
    for i := 1; i < len(target); i++ {
        x := target[i] - nums[i]
        y := target[i-1] - nums[i-1]
        if x*y > 0 {
            if d := abs(x) - abs(y); d > 0 {
                f += d
            }
        } else {
            f += abs(x)
        }
    }
    return int64(f)
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

#### TypeScript

```ts
function minimumOperations(nums: number[], target: number[]): number {
    const n = nums.length;
    let f = Math.abs(target[0] - nums[0]);
    for (let i = 1; i < n; ++i) {
        const x = target[i] - nums[i];
        const y = target[i - 1] - nums[i - 1];
        if (x * y > 0) {
            const d = Math.abs(x) - Math.abs(y);
            if (d > 0) {
                f += d;
            }
        } else {
            f += Math.abs(x);
        }
    }
    return f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
