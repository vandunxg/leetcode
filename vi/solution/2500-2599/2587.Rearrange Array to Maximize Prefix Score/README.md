---
comments: true
difficulty: Medium
rating: 1336
source: Weekly Contest 336 Q2
tags:
    - Greedy
    - Array
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2587. Rearrange Array to Maximize Prefix Score](https://leetcode.com/problems/rearrange-array-to-maximize-prefix-score)

[中文文档](/solution/2500-2599/2587.Rearrange%20Array%20to%20Maximize%20Prefix%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>. Bạn có thể sắp xếp lại các phần tử của <code>nums</code> theo <strong>bất kỳ thứ tự nào</strong> (bao gồm cả thứ tự ban đầu).</p>

<p>Gọi <code>prefix</code> là mảng chứa các tổng tiền tố của <code>nums</code> sau khi sắp xếp lại. Nói cách khác, <code>prefix[i]</code> là tổng các phần tử từ <code>0</code> đến <code>i</code> trong <code>nums</code> sau khi sắp xếp lại. <strong>Điểm số</strong> của <code>nums</code> là số lượng số nguyên dương trong mảng <code>prefix</code>.</p>

<p>Trả về <em>điểm số lớn nhất có thể đạt được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,-1,0,1,-3,3,-3]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Ta có thể sắp xếp lại mảng thành nums = [2,3,1,-1,-3,0,-3].
prefix = [2,5,6,5,2,2,-1], nên điểm số là 6.
Có thể chứng minh rằng 6 là điểm số lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-2,-3,0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mọi cách sắp xếp lại mảng đều cho điểm số bằng 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>6</sup> &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp lại mảng để tối đa hóa số lượng tổng tiền tố dương. Các giá trị dương nên được đặt trước, vì vậy ta sắp xếp theo thứ tự giảm dần.
>
> Tính tổng tiền tố; khi tổng này không còn dương, các tổng tiền tố về sau chỉ có thể giảm, và số lượng đã đếm được là đáp án. Nếu tổng không bao giờ giảm xuống, đáp án là $n$.

<!-- thinking:end -->

Để tối đa hóa số lượng số nguyên dương trong mảng tổng tiền tố, ta cần làm cho các phần tử trong mảng tổng tiền tố lớn nhất có thể, tức là cộng càng nhiều số nguyên dương càng tốt. Vì vậy, ta có thể sắp xếp mảng $nums$ theo thứ tự giảm dần, sau đó duyệt mảng và duy trì tổng tiền tố $s$. Nếu $s \leq 0$, điều đó có nghĩa là tại vị trí hiện tại và các vị trí sau đó không thể có thêm số nguyên dương nào, nên ta có thể trả về trực tiếp vị trí hiện tại.

Ngược lại, sau khi duyệt xong, ta trả về độ dài của mảng.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums: List[int]) -> int:
        nums.sort(reverse=True)
        s = 0
        for i, x in enumerate(nums):
            s += x
            if s <= 0:
                return i
        return len(nums)
```

#### Java

```java
class Solution {
    public int maxScore(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        long s = 0;
        for (int i = 0; i < n; ++i) {
            s += nums[n - i - 1];
            if (s <= 0) {
                return i;
            }
        }
        return n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScore(vector<int>& nums) {
        sort(nums.rbegin(), nums.rend());
        long long s = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            s += nums[i];
            if (s <= 0) {
                return i;
            }
        }
        return n;
    }
};
```

#### Go

```go
func maxScore(nums []int) int {
	sort.Ints(nums)
	n := len(nums)
	s := 0
	for i := range nums {
		s += nums[n-i-1]
		if s <= 0 {
			return i
		}
	}
	return n
}
```

#### TypeScript

```ts
function maxScore(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let s = 0;
    for (let i = 0; i < n; ++i) {
        s += nums[n - i - 1];
        if (s <= 0) {
            return i;
        }
    }
    return n;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_score(mut nums: Vec<i32>) -> i32 {
        nums.sort_by(|a, b| b.cmp(a));
        let mut s: i64 = 0;
        for (i, &x) in nums.iter().enumerate() {
            s += x as i64;
            if s <= 0 {
                return i as i32;
            }
        }
        nums.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
