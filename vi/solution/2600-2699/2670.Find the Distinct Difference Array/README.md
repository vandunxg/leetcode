---
comments: true
difficulty: Easy
rating: 1266
source: Weekly Contest 344 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2670. Find the Distinct Difference Array](https://leetcode.com/problems/find-the-distinct-difference-array)

[中文文档](/solution/2600-2699/2670.Find%20the%20Distinct%20Difference%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code>.</p>

<p><strong>Mảng hiệu số phần tử khác nhau</strong> của <code>nums</code> là một mảng <code>diff</code> có độ dài <code>n</code>, trong đó <code>diff[i]</code> bằng số phần tử khác nhau trong hậu tố <code>nums[i + 1, ..., n - 1]</code> <strong>trừ đi</strong> số phần tử khác nhau trong tiền tố <code>nums[0, ..., i]</code>.</p>

<p>Trả về <em><strong>mảng hiệu số phần tử khác nhau</strong> của </em><code>nums</code>.</p>

<p>Lưu ý rằng <code>nums[i, ..., j]</code> biểu diễn mảng con của <code>nums</code> bắt đầu tại chỉ số <code>i</code> và kết thúc tại chỉ số <code>j</code>, bao gồm cả hai đầu. Đặc biệt, nếu <code>i &gt; j</code> thì <code>nums[i, ..., j]</code> biểu diễn một mảng con rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> [-3,-1,1,3,5]
<strong>Giải thích:</strong> Với chỉ số i = 0, tiền tố có 1 phần tử và hậu tố có 4 phần tử khác nhau. Do đó, diff[0] = 1 - 4 = -3.
Với chỉ số i = 1, tiền tố có 2 phần tử khác nhau và hậu tố có 3 phần tử khác nhau. Do đó, diff[1] = 2 - 3 = -1.
Với chỉ số i = 2, tiền tố có 3 phần tử khác nhau và hậu tố có 2 phần tử khác nhau. Do đó, diff[2] = 3 - 2 = 1.
Với chỉ số i = 3, tiền tố có 4 phần tử khác nhau và hậu tố có 1 phần tử khác nhau. Do đó, diff[3] = 4 - 1 = 3.
Với chỉ số i = 4, tiền tố có 5 phần tử khác nhau và hậu tố không có phần tử nào. Do đó, diff[4] = 5 - 0 = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,3,4,2]
<strong>Đầu ra:</strong> [-2,-1,0,2,3]
<strong>Giải thích:</strong> Với chỉ số i = 0, tiền tố có 1 phần tử và hậu tố có 3 phần tử khác nhau. Do đó, diff[0] = 1 - 3 = -2.
Với chỉ số i = 1, tiền tố có 2 phần tử khác nhau và hậu tố có 3 phần tử khác nhau. Do đó, diff[1] = 2 - 3 = -1.
Với chỉ số i = 2, tiền tố có 2 phần tử khác nhau và hậu tố có 2 phần tử khác nhau. Do đó, diff[2] = 2 - 2 = 0.
Với chỉ số i = 3, tiền tố có 3 phần tử khác nhau và hậu tố có 1 phần tử khác nhau. Do đó, diff[3] = 3 - 1 = 2.
Với chỉ số i = 4, tiền tố có 3 phần tử khác nhau và hậu tố không có phần tử nào. Do đó, diff[4] = 3 - 0 = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length&nbsp;&lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Tiền xử lý hậu tố

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chỉ số cần hiệu giữa số phần tử khác nhau trong tiền tố và hậu tố. Việc xây dựng lại hai set cho từng $i$ là lãng phí, ngay cả khi $n \le 50$.
>
> Một set duyệt từ phải sang trái sẽ tạo ra số phần tử khác nhau trong hậu tố $suf[i]$; sau đó, một lượt duyệt từ trái sang phải sẽ cho kết quả $|s|-suf[i+1]$.

<!-- thinking:end -->

Ta có thể tiền xử lý một mảng hậu tố $suf$, trong đó $suf[i]$ biểu diễn số phần tử khác nhau trong hậu tố $nums[i, ..., n - 1]$. Trong quá trình tiền xử lý, ta sử dụng một hash table $s$ để lưu các phần tử đã xuất hiện trong hậu tố, nhờ đó có thể truy vấn số phần tử khác nhau trong hậu tố trong thời gian $O(1)$.

Sau khi tiền xử lý mảng hậu tố $suf$, ta xóa hash table $s$, rồi duyệt lại mảng $nums$, sử dụng hash table $s$ để lưu các phần tử đã xuất hiện trong tiền tố. Đáp án tại vị trí $i$ là số phần tử khác nhau trong $s$ trừ đi $suf[i + 1]$, tức là $s.size() - suf[i + 1]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctDifferenceArray(self, nums: List[int]) -> List[int]:
        n = len(nums)
        suf = [0] * (n + 1)
        s = set()
        for i in range(n - 1, -1, -1):
            s.add(nums[i])
            suf[i] = len(s)
        s.clear()
        ans = [0] * n
        for i, x in enumerate(nums):
            s.add(x)
            ans[i] = len(s) - suf[i + 1]
        return ans
```

#### Java

```java
class Solution {
    public int[] distinctDifferenceArray(int[] nums) {
        int n = nums.length;
        int[] suf = new int[n + 1];
        Set<Integer> s = new HashSet<>();
        for (int i = n - 1; i >= 0; --i) {
            s.add(nums[i]);
            suf[i] = s.size();
        }
        s.clear();
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            s.add(nums[i]);
            ans[i] = s.size() - suf[i + 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> distinctDifferenceArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> suf(n + 1);
        unordered_set<int> s;
        for (int i = n - 1; i >= 0; --i) {
            s.insert(nums[i]);
            suf[i] = s.size();
        }
        s.clear();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            s.insert(nums[i]);
            ans[i] = s.size() - suf[i + 1];
        }
        return ans;
    }
};
```

#### Go

```go
func distinctDifferenceArray(nums []int) []int {
	n := len(nums)
	suf := make([]int, n+1)
	s := map[int]bool{}
	for i := n - 1; i >= 0; i-- {
		s[nums[i]] = true
		suf[i] = len(s)
	}
	ans := make([]int, n)
	s = map[int]bool{}
	for i, x := range nums {
		s[x] = true
		ans[i] = len(s) - suf[i+1]
	}
	return ans
}
```

#### TypeScript

```ts
function distinctDifferenceArray(nums: number[]): number[] {
    const n = nums.length;
    const suf: number[] = Array(n + 1).fill(0);
    const s: Set<number> = new Set();
    for (let i = n - 1; i >= 0; --i) {
        s.add(nums[i]);
        suf[i] = s.size;
    }
    s.clear();
    const ans: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        s.add(nums[i]);
        ans[i] = s.size - suf[i + 1];
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn distinct_difference_array(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let mut suf = vec![0; n + 1];
        let mut s = HashSet::new();

        for i in (0..n).rev() {
            s.insert(nums[i]);
            suf[i] = s.len();
        }

        let mut ans = Vec::new();
        s.clear();
        for i in 0..n {
            s.insert(nums[i]);
            ans.push((s.len() - suf[i + 1]) as i32);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
