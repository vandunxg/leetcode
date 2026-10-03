---
comments: true
difficulty: Easy
rating: 1447
source: Biweekly Contest 67 Q1
tags:
    - Array
    - Hash Table
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2099. Find Subsequence of Length K With the Largest Sum](https://leetcode.com/problems/find-subsequence-of-length-k-with-the-largest-sum)

[中文文档](/solution/2000-2099/2099.Find%20Subsequence%20of%20Length%20K%20With%20the%20Largest%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Hãy tìm một <strong>dãy con </strong>của <code>nums</code> có độ dài <code>k</code> và có <strong>tổng lớn nhất</strong>.</p>

<p>Trả về<em> </em><em><strong>bất kỳ</strong> dãy con nào như vậy dưới dạng một mảng số nguyên có độ dài </em><code>k</code>.</p>

<p><strong>Dãy con</strong> là một mảng có thể được tạo ra từ một mảng khác bằng cách xóa một số phần tử hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3,3], k = 2
<strong>Đầu ra:</strong> [3,3]
<strong>Giải thích:</strong>
Dãy con có tổng lớn nhất là 3 + 3 = 6.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,-2,3,4], k = 3
<strong>Đầu ra:</strong> [-1,3,4]
<strong>Giải thích:</strong>
Dãy con có tổng lớn nhất là -1 + 3 + 4 = 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,3,3], k = 2
<strong>Đầu ra:</strong> [3,4]
<strong>Giải thích:</strong>
Dãy con có tổng lớn nhất là 3 + 4 = 7.
Một dãy con khả dĩ khác là [4, 3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-10<sup>5</sup>&nbsp;&lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Để có tổng lớn nhất mà vẫn giữ nguyên thứ tự ban đầu, ta chọn $k$ giá trị lớn nhất rồi sắp xếp chúng theo chỉ số. Vì $n \le 1000$, ta sắp xếp các chỉ số theo giá trị.
>
> Giữ lại $k$ chỉ số cuối cùng và xuất `nums` theo thứ tự chỉ số tăng dần.

<!-- thinking:end -->

Trước tiên, ta tạo một mảng chỉ số $\textit{idx}$, trong đó mỗi phần tử là một chỉ số của mảng $\textit{nums}$. Sau đó, ta sắp xếp mảng chỉ số $\textit{idx}$ dựa trên các giá trị trong $\textit{nums}$, với quy tắc sắp xếp là $\textit{nums}[i] < \textit{nums}[j]$, trong đó $i$ và $j$ là hai chỉ số trong mảng chỉ số $\textit{idx}$.

Sau khi sắp xếp, ta lấy $k$ phần tử cuối cùng của mảng chỉ số $\textit{idx}$. $k$ phần tử này tương ứng với $k$ phần tử lớn nhất trong mảng $\textit{nums}$. Sau đó, ta sắp xếp $k$ chỉ số này để thu được thứ tự của $k$ phần tử lớn nhất trong mảng $\textit{nums}$.

Độ phức tạp thời gian là $O(n \log n)$, còn độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubsequence(self, nums: List[int], k: int) -> List[int]:
        idx = sorted(range(len(nums)), key=lambda i: nums[i])[-k:]
        return [nums[i] for i in sorted(idx)]
```

#### Java

```java
class Solution {
    public int[] maxSubsequence(int[] nums, int k) {
        int n = nums.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i, j) -> nums[i] - nums[j]);
        Arrays.sort(idx, n - k, n);
        int[] ans = new int[k];
        for (int i = n - k; i < n; ++i) {
            ans[i - (n - k)] = nums[idx[i]];
        }
        return ans;
    }
}
```

#### C++

```cpp
#include <ranges>

class Solution {
public:
    vector<int> maxSubsequence(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> idx(n);
        ranges::iota(idx, 0);
        ranges::sort(idx, [&](int i, int j) { return nums[i] < nums[j]; });
        ranges::sort(idx | views::drop(n - k));
        vector<int> ans(k);
        for (int i = n - k; i < n; ++i) {
            ans[i - (n - k)] = nums[idx[i]];
        }
        return ans;
    }
};
```

#### Go

```go
func maxSubsequence(nums []int, k int) []int {
	idx := slices.Clone(make([]int, len(nums)))
	for i := range idx {
		idx[i] = i
	}
	slices.SortFunc(idx, func(i, j int) int { return nums[i] - nums[j] })
	slices.Sort(idx[len(idx)-k:])
	ans := make([]int, k)
	for i := range ans {
		ans[i] = nums[idx[len(idx)-k+i]]
	}
	return ans
}
```

#### TypeScript

```ts
function maxSubsequence(nums: number[], k: number): number[] {
    const n = nums.length;
    const idx: number[] = Array.from({ length: n }, (_, i) => i);
    idx.sort((i, j) => nums[i] - nums[j]);
    return idx
        .slice(n - k)
        .sort((i, j) => i - j)
        .map(i => nums[i]);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_subsequence(nums: Vec<i32>, k: i32) -> Vec<i32> {
        let n = nums.len();
        let k = k as usize;
        let mut idx: Vec<usize> = (0..n).collect();

        idx.sort_by_key(|&i| nums[i]);
        idx[n - k..].sort();

        let mut ans = Vec::with_capacity(k);
        for i in n - k..n {
            ans.push(nums[idx[i]]);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
