---
comments: true
difficulty: Medium
rating: 1720
source: Weekly Contest 332 Q2
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2563. Count the Number of Fair Pairs](https://leetcode.com/problems/count-the-number-of-fair-pairs)

[中文文档](/solution/2500-2599/2563.Count%20the%20Number%20of%20Fair%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code> có kích thước <code>n</code> và hai số nguyên <code>lower</code>, <code>upper</code>, hãy trả về <em>số lượng cặp hợp lệ</em>.</p>

<p>Một cặp <code>(i, j)</code> là <b>cặp hợp lệ</b> nếu:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; n</code>, và</li>
	<li><code>lower &lt;= nums[i] + nums[j] &lt;= upper</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,7,4,4,5], lower = 3, upper = 6
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có 6 cặp hợp lệ: (0,3), (0,4), (0,5), (1,3), (1,4) và (1,5).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,7,9,2,5], lower = 11, upper = 11
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có một cặp hợp lệ: (2,3).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums.length == n</code></li>
	<li><code><font face="monospace">-10<sup>9</sup></font>&nbsp;&lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code><font face="monospace">-10<sup>9</sup>&nbsp;&lt;= lower &lt;= upper &lt;= 10<sup>9</sup></font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cặp không thứ tự có tổng nằm trong $[\textit{lower},\textit{upper}]$. Vòng lặp kép không phù hợp với $n\le 10^5$.
>
> Chỉ các giá trị mới ảnh hưởng, nên có thể sắp xếp an toàn. Với $x=nums[i]$ cố định, phần tử ghép cặp phải nằm trong $[\textit{lower}-x,\textit{upper}-x]$; hiệu giữa hai cận dưới chính là số lượng cặp, bắt đầu từ $i+1$ để tránh đếm trùng.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng `nums` theo thứ tự tăng dần. Sau đó, với mỗi `nums[i]`, ta sử dụng tìm kiếm nhị phân để tìm cận dưới `j` của `nums[j]`, tức là chỉ số đầu tiên thỏa mãn `nums[j] >= lower - nums[i]`. Tiếp theo, ta lại sử dụng tìm kiếm nhị phân để tìm cận dưới `k` của `nums[k]`, tức là chỉ số đầu tiên thỏa mãn `nums[k] >= upper - nums[i] + 1`. Do đó, `[j, k)` là phạm vi chỉ số của `nums[j]` thỏa mãn `lower <= nums[i] + nums[j] <= upper`. Số lượng các chỉ số này, tương ứng với số lượng `nums[j]`, là `k - j`, và ta cộng giá trị này vào đáp án. Lưu ý rằng $j > i$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countFairPairs(self, nums: List[int], lower: int, upper: int) -> int:
        nums.sort()
        ans = 0
        for i, x in enumerate(nums):
            j = bisect_left(nums, lower - x, lo=i + 1)
            k = bisect_left(nums, upper - x + 1, lo=i + 1)
            ans += k - j
        return ans
```

#### Java

```java
class Solution {
    public long countFairPairs(int[] nums, int lower, int upper) {
        Arrays.sort(nums);
        long ans = 0;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            int j = search(nums, lower - nums[i], i + 1);
            int k = search(nums, upper - nums[i] + 1, i + 1);
            ans += k - j;
        }
        return ans;
    }

    private int search(int[] nums, int x, int left) {
        int right = nums.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countFairPairs(vector<int>& nums, int lower, int upper) {
        long long ans = 0;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); ++i) {
            auto j = lower_bound(nums.begin() + i + 1, nums.end(), lower - nums[i]);
            auto k = lower_bound(nums.begin() + i + 1, nums.end(), upper - nums[i] + 1);
            ans += k - j;
        }
        return ans;
    }
};
```

#### Go

```go
func countFairPairs(nums []int, lower int, upper int) (ans int64) {
	sort.Ints(nums)
	for i, x := range nums {
		j := sort.Search(len(nums), func(h int) bool { return h > i && nums[h] >= lower-x })
		k := sort.Search(len(nums), func(h int) bool { return h > i && nums[h] >= upper-x+1 })
		ans += int64(k - j)
	}
	return
}
```

#### TypeScript

```ts
function countFairPairs(nums: number[], lower: number, upper: number): number {
    const search = (x: number, l: number): number => {
        let r = nums.length;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };

    nums.sort((a, b) => a - b);
    let ans = 0;
    for (let i = 0; i < nums.length; ++i) {
        const j = search(lower - nums[i], i + 1);
        const k = search(upper - nums[i] + 1, i + 1);
        ans += k - j;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
