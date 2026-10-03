---
comments: true
difficulty: Easy
rating: 1306
source: Weekly Contest 256 Q1
tags:
    - Array
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [1984. Minimum Difference Between Highest and Lowest of K Scores](https://leetcode.com/problems/minimum-difference-between-highest-and-lowest-of-k-scores)

[中文文档](/solution/1900-1999/1984.Minimum%20Difference%20Between%20Highest%20and%20Lowest%20of%20K%20Scores/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được <strong>đánh chỉ số từ 0</strong>, trong đó <code>nums[i]</code> là điểm số của học sinh thứ <code>i<sup>th</sup></code>. Bạn cũng được cho một số nguyên <code>k</code>.</p>

<p>Hãy chọn điểm số của bất kỳ <code>k</code> học sinh nào trong mảng sao cho <strong>chênh lệch</strong> giữa điểm số <strong>cao nhất</strong> và <strong>thấp nhất</strong> trong <code>k</code> điểm số đó là <strong>nhỏ nhất</strong>.</p>

<p>Trả về <em><strong>chênh lệch nhỏ nhất có thể</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [90], k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Có một cách chọn điểm số của một học sinh:
- [<strong><u>90</u></strong>]. Chênh lệch giữa điểm số cao nhất và thấp nhất là 90 - 90 = 0.
Chênh lệch nhỏ nhất có thể là 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9,4,1,7], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có sáu cách chọn điểm số của hai học sinh:
- [<strong><u>9</u></strong>,<strong><u>4</u></strong>,1,7]. Chênh lệch giữa điểm số cao nhất và thấp nhất là 9 - 4 = 5.
- [<strong><u>9</u></strong>,4,<strong><u>1</u></strong>,7]. Chênh lệch giữa điểm số cao nhất và thấp nhất là 9 - 1 = 8.
- [<strong><u>9</u></strong>,4,1,<strong><u>7</u></strong>]. Chênh lệch giữa điểm số cao nhất và thấp nhất là 9 - 7 = 2.
- [9,<strong><u>4</u></strong>,<strong><u>1</u></strong>,7]. Chênh lệch giữa điểm số cao nhất và thấp nhất là 4 - 1 = 3.
- [9,<strong><u>4</u></strong>,1,<strong><u>7</u></strong>]. Chênh lệch giữa điểm số cao nhất và thấp nhất là 7 - 4 = 3.
- [9,4,<strong><u>1</u></strong>,<strong><u>7</u></strong>]. Chênh lệch giữa điểm số cao nhất và thấp nhất là 7 - 1 = 6.
Chênh lệch nhỏ nhất có thể là 2.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần chọn $k$ điểm số sao cho chênh lệch giữa giá trị lớn nhất và nhỏ nhất là nhỏ nhất. Sau khi sắp xếp, một tập gồm $k$ phần tử tối ưu luôn liên tiếp; nếu bỏ qua các giá trị ở giữa, hai đầu chỉ có thể cách xa nhau hơn.
>
> Đáp án là giá trị nhỏ nhất của $\textit{nums}[i+k-1]-\textit{nums}[i]$.

<!-- thinking:end -->

Ta có thể sắp xếp điểm số của học sinh theo thứ tự tăng dần, sau đó dùng một cửa sổ trượt có kích thước $k$ để tính chênh lệch giữa giá trị lớn nhất và nhỏ nhất trong cửa sổ, rồi lấy giá trị nhỏ nhất trong các chênh lệch của mọi cửa sổ.

Tại sao ta chọn $k$ học sinh liên tiếp? Bởi vì nếu chúng không liên tiếp, chênh lệch giữa giá trị lớn nhất và nhỏ nhất có thể giữ nguyên hoặc tăng lên, nhưng chắc chắn không giảm. Vì vậy, sau khi sắp xếp, ta chỉ cần xét các nhóm gồm $k$ học sinh liên tiếp.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số học sinh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDifference(self, nums: List[int], k: int) -> int:
        nums.sort()
        return min(nums[i + k - 1] - nums[i] for i in range(len(nums) - k + 1))
```

#### Java

```java
class Solution {
    public int minimumDifference(int[] nums, int k) {
        Arrays.sort(nums);
        int ans = 100000;
        for (int i = 0; i < nums.length - k + 1; ++i) {
            ans = Math.min(ans, nums[i + k - 1] - nums[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumDifference(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int ans = 1e5;
        for (int i = 0; i < nums.size() - k + 1; ++i) {
            ans = min(ans, nums[i + k - 1] - nums[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumDifference(nums []int, k int) int {
	sort.Ints(nums)
	ans := 100000
	for i := 0; i < len(nums)-k+1; i++ {
		ans = min(ans, nums[i+k-1]-nums[i])
	}
	return ans
}
```

#### TypeScript

```ts
function minimumDifference(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let ans = nums[n - 1] - nums[0];
    for (let i = 0; i + k - 1 < n; i++) {
        ans = Math.min(nums[i + k - 1] - nums[i], ans);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_difference(mut nums: Vec<i32>, k: i32) -> i32 {
        nums.sort();
        let k = k as usize;
        let mut res = i32::MAX;
        for i in 0..=nums.len() - k {
            res = res.min(nums[i + k - 1] - nums[i]);
        }
        res
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @param Integer $k
     * @return Integer
     */
    function minimumDifference($nums, $k) {
        sort($nums);
        $ans = 10 ** 5;
        for ($i = 0; $i < count($nums) - $k + 1; $i++) {
            $ans = min($ans, $nums[$i + $k - 1] - $nums[$i]);
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
