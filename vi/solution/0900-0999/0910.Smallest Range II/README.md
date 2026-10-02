---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Math
    - Sorting
---

<!-- problem:start -->

# [910. Smallest Range II](https://leetcode.com/problems/smallest-range-ii)

[中文文档](/solution/0900-0999/0910.Smallest%20Range%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>.</p>

<p>Với mỗi chỉ số <code>i</code> thỏa mãn <code>0 &lt;= i &lt; nums.length</code>, hãy đổi <code>nums[i]</code> thành <code>nums[i] + k</code> hoặc <code>nums[i] - k</code>.</p>

<p><strong>Điểm số</strong> của <code>nums</code> là hiệu giữa phần tử lớn nhất và nhỏ nhất trong mảng.</p>

<p>Trả về <em><strong>điểm số</strong> nhỏ nhất của </em><code>nums</code><em> sau khi thay đổi giá trị tại mỗi chỉ số</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1], k = 0
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Điểm số là max(nums) - min(nums) = 1 - 1 = 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,10], k = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Đổi nums thành [2, 8]. Điểm số là max(nums) - min(nums) = 8 - 2 = 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,6], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Đổi nums thành [4, 6, 3]. Điểm số là max(nums) - min(nums) = 6 - 3 = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giá trị được cộng hoặc trừ $k$; không thể thử hết $2^n$ cách gán. Sau khi sắp xếp, các số nhỏ hơn nên được cộng $k$ và các số lớn hơn nên bị trừ $k$, vì vậy cách chia tối ưu gồm một prefix được cộng $k$ và một suffix bị trừ $k$.
>
> Với mỗi điểm chia $i$, giá trị nhỏ nhất mới là $\min(nums[0]+k,\,nums[i]-k)$ và giá trị lớn nhất mới là $\max(nums[i-1]+k,\,nums[-1]-k)$. Tìm hiệu nhỏ nhất, đồng thời xét cả trường hợp giữ nguyên mảng.

<!-- thinking:end -->

Theo đề bài, ta cần tìm hiệu nhỏ nhất giữa giá trị lớn nhất và nhỏ nhất của mảng. Mỗi phần tử có thể được cộng hoặc trừ $k$, nên ta chia các phần tử thành hai nhóm: một nhóm được cộng $k$, nhóm còn lại bị trừ $k$. Để giảm chênh lệch lớn nhất có thể, ta nên trừ $k$ khỏi các giá trị lớn hơn và cộng $k$ vào các giá trị nhỏ hơn.

Vì vậy, trước tiên ta sắp xếp mảng, sau đó duyệt từng vị trí chia mảng thành hai phần: phần đầu được cộng $k$, phần sau bị trừ $k$. Với mỗi cách chia, tính hiệu giữa giá trị lớn nhất và nhỏ nhất rồi lấy hiệu nhỏ nhất.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, với $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestRangeII(self, nums: List[int], k: int) -> int:
        nums.sort()
        ans = nums[-1] - nums[0]
        for i in range(1, len(nums)):
            mi = min(nums[0] + k, nums[i] - k)
            mx = max(nums[i - 1] + k, nums[-1] - k)
            ans = min(ans, mx - mi)
        return ans
```

#### Java

```java
class Solution {
    public int smallestRangeII(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int ans = nums[n - 1] - nums[0];
        for (int i = 1; i < n; ++i) {
            int mi = Math.min(nums[0] + k, nums[i] - k);
            int mx = Math.max(nums[i - 1] + k, nums[n - 1] - k);
            ans = Math.min(ans, mx - mi);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestRangeII(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int ans = nums[n - 1] - nums[0];
        for (int i = 1; i < n; ++i) {
            int mi = min(nums[0] + k, nums[i] - k);
            int mx = max(nums[i - 1] + k, nums[n - 1] - k);
            ans = min(ans, mx - mi);
        }
        return ans;
    }
};
```

#### Go

```go
func smallestRangeII(nums []int, k int) int {
	sort.Ints(nums)
	n := len(nums)
	ans := nums[n-1] - nums[0]
	for i := 1; i < n; i++ {
		mi := min(nums[0]+k, nums[i]-k)
		mx := max(nums[i-1]+k, nums[n-1]-k)
		ans = min(ans, mx-mi)
	}
	return ans
}
```

#### TypeScript

```ts
function smallestRangeII(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    let ans = nums.at(-1)! - nums[0];
    for (let i = 1; i < nums.length; ++i) {
        const mi = Math.min(nums[0] + k, nums[i] - k);
        const mx = Math.max(nums.at(-1)! - k, nums[i - 1] + k);
        ans = Math.min(ans, mx - mi);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
