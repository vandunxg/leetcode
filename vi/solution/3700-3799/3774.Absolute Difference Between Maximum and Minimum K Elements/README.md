---
comments: true
difficulty: Easy
rating: 1206
source: Weekly Contest 480 Q1
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [3774. Absolute Difference Between Maximum and Minimum K Elements](https://leetcode.com/problems/absolute-difference-between-maximum-and-minimum-k-elements)

[中文文档](/solution/3700-3799/3774.Absolute%20Difference%20Between%20Maximum%20and%20Minimum%20K%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Hãy tìm độ chênh lệch tuyệt đối giữa:</p>

<ul>
	<li><strong>tổng</strong> của <code>k</code> phần tử <strong>lớn nhất</strong> trong mảng; và</li>
	<li><strong>tổng</strong> của <code>k</code> phần tử <strong>nhỏ nhất</strong> trong mảng.</li>
</ul>

<p>Trả về một số nguyên biểu thị độ chênh lệch này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,2,2,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>k = 2</code> phần tử lớn nhất là 4 và 5. Tổng của chúng là <code>4 + 5 = 9</code>.</li>
	<li><code>k = 2</code> phần tử nhỏ nhất là 2 và 2. Tổng của chúng là <code>2 + 2 = 4</code>.</li>
	<li>Độ chênh lệch tuyệt đối là <code>abs(9 - 4) = 5</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [100], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phần tử lớn nhất là 100.</li>
	<li>Phần tử nhỏ nhất là 100.</li>
	<li>Độ chênh lệch tuyệt đối là <code>abs(100 - 100) = 0</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi sắp xếp, độ chênh lệch giữa tổng của $k$ giá trị lớn nhất và $k$ giá trị nhỏ nhất chính là tổng $k$ phần tử cuối trừ đi tổng $k$ phần tử đầu. Với $n\le 100$, chỉ cần sắp xếp toàn bộ mảng.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp mảng $\textit{nums}$. Sau đó, chúng ta tính tổng của $k$ phần tử đầu tiên và tổng của $k$ phần tử cuối cùng trong mảng, rồi trả về hiệu giữa hai tổng này.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def absDifference(self, nums: List[int], k: int) -> int:
        nums.sort()
        return sum(nums[-k:]) - sum(nums[:k])
```

#### Java

```java
class Solution {
    public int absDifference(int[] nums, int k) {
        Arrays.sort(nums);
        int ans = 0;
        int n = nums.length;
        for (int i = 0; i < k; ++i) {
            ans += nums[n - i - 1] - nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int absDifference(vector<int>& nums, int k) {
        ranges::sort(nums);
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < k; ++i) {
            ans += nums[n - i - 1] - nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func absDifference(nums []int, k int) (ans int) {
	slices.Sort(nums)
	for i := 0; i < k; i++ {
		ans += nums[len(nums)-i-1] - nums[i]
	}
	return
}
```

#### TypeScript

```ts
function absDifference(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    let ans = 0;
    for (let i = 0; i < k; ++i) {
        ans += nums.at(-i - 1)! - nums[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
