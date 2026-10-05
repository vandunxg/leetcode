---
comments: true
difficulty: Medium
rating: 1662
source: Weekly Contest 499 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3914. Minimum Operations to Make Array Non Decreasing](https://leetcode.com/problems/minimum-operations-to-make-array-non-decreasing)

[中文文档](/solution/3900-3999/3914.Minimum%20Operations%20to%20Make%20Array%20Non%20Decreasing/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Trong một thao tác, bạn có thể chọn bất kỳ <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> nào <code>nums[l..r]</code> và <strong>tăng</strong> mỗi phần tử trong <strong>mảng con</strong> đó thêm <code>x</code>, trong đó <code>x</code> là một số nguyên <strong>dương</strong> bất kỳ.</p>

<p>Trả về <strong>tổng</strong> <strong>nhỏ nhất</strong> có thể của các giá trị <code>x</code> trong tất cả các thao tác cần thực hiện để làm cho mảng trở thành <strong>không giảm</strong>.</p>

<p>Một mảng được gọi là <strong>không giảm</strong> nếu <code>nums[i] &lt;= nums[i + 1]</code> với mọi <code>0 &lt;= i &lt; n - 1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một tập hợp thao tác tối ưu:</p>

<ul>
	<li>Chọn mảng con <code>[2..3]</code> và thêm <code>x = 1</code>, thu được <code>[3, 3, 3, 2]</code></li>
	<li>Chọn mảng con <code>[3..3]</code> và thêm <code>x = 1</code>, thu được <code>[3, 3, 3, 3]</code></li>
</ul>

<p>Mảng trở thành không giảm, và tổng các giá trị <code>x</code> đã chọn là <code>1 + 1 = 2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một tập hợp thao tác tối ưu:</p>

<ul>
	<li>Chọn mảng con <code>[1..3]</code> và thêm <code>x = 4</code>, thu được <code>[5, 5, 6, 7]</code></li>
</ul>

<p>Mảng trở thành không giảm, và tổng các giá trị <code>x</code> đã chọn là <code>4</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác làm tăng một phần tử lên một đơn vị. Khi duyệt từ trái sang phải, một lần giảm buộc giá trị phía sau phải tăng lên bằng mức của phần tử trước đó, và việc tăng này không thể phá vỡ phần tiền tố đã thỏa mãn điều kiện.
>
> Do đó, mỗi cặp liền kề $(a,b)$ đóng góp $\max(a-b,0)$. Tổng các khoảng cách cục bộ này là số lần tăng nhỏ nhất.
>
> Với $n\le 10^5$, chỉ cần một lần duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể duyệt mảng từ trái sang phải và tính hiệu giữa mỗi cặp phần tử liền kề. Nếu phần tử hiện tại nhỏ hơn phần tử trước đó, ta cần tăng phần tử hiện tại để nó ít nhất bằng phần tử trước đó. Lượng cần tăng chính là hiệu giữa phần tử trước đó và phần tử hiện tại.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: list[int]) -> int:
        return sum(max(a - b, 0) for a, b in pairwise(nums))
```

#### Java

```java
class Solution {
    public long minOperations(int[] nums) {
        long ans = 0;
        for (int i = 1; i < nums.length; ++i) {
            ans += Math.max(nums[i - 1] - nums[i], 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minOperations(vector<int>& nums) {
        long long ans = 0;
        for (int i = 1; i < nums.size(); ++i) {
            ans += max(nums[i - 1] - nums[i], 0);
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) (ans int64) {
	for i := 1; i < len(nums); i++ {
		ans += max(int64(nums[i-1]-nums[i]), 0)
	}
	return
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    let ans = 0;
    for (let i = 1; i < nums.length; ++i) {
        ans += Math.max(nums[i - 1] - nums[i], 0);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
