---
comments: true
difficulty: Medium
rating: 1761
source: Biweekly Contest 90 Q3
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2453. Destroy Sequential Targets](https://leetcode.com/problems/destroy-sequential-targets)

[中文文档](/solution/2400-2499/2453.Destroy%20Sequential%20Targets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> được đánh chỉ số từ <strong>0</strong>, gồm các số nguyên dương biểu diễn những mục tiêu trên một trục số. Bạn cũng được cho một số nguyên <code>space</code>.</p>

<p>Bạn có một cỗ máy có thể phá hủy các mục tiêu. <strong>Gieo hạt</strong> cho cỗ máy bằng một giá trị <code>nums[i]</code> cho phép nó phá hủy tất cả mục tiêu có giá trị biểu diễn được dưới dạng <code>nums[i] + c * space</code>, trong đó <code>c</code> là một số nguyên không âm. Bạn muốn phá hủy <strong>nhiều</strong> mục tiêu nhất trong <code>nums</code>.</p>

<p>Hãy trả về <em><strong>giá trị nhỏ nhất</strong> của </em><code>nums[i]</code><em> mà bạn có thể dùng để gieo hạt cho cỗ máy nhằm phá hủy nhiều mục tiêu nhất.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,7,8,1,1,5], space = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Nếu gieo hạt cho cỗ máy bằng nums[3], ta sẽ phá hủy tất cả mục tiêu bằng 1,3,5,7,9,...
Trong trường hợp này, ta phá hủy tổng cộng 5 mục tiêu (tất cả trừ nums[2]).
Không thể phá hủy nhiều hơn 5 mục tiêu, vì vậy ta trả về nums[3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,2,4,6], space = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Gieo hạt cho cỗ máy bằng nums[0] hoặc nums[3] sẽ phá hủy 3 mục tiêu.
Không thể phá hủy nhiều hơn 3 mục tiêu.
Vì nums[0] là số nguyên nhỏ nhất có thể phá hủy 3 mục tiêu, ta trả về 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,2,5], space = 100
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Dù chọn hạt ban đầu nào, ta cũng chỉ có thể phá hủy 1 mục tiêu. Hạt nhỏ nhất là nums[1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= space &lt;=&nbsp;10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Modulo + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Với một hạt giống $x$, ta phá hủy mọi giá trị $x+k\cdot space$, tức là một lớp đồng dư. Với $n\le 10^5$, ta đếm theo $v\bmod space$. Lớp xuất hiện nhiều nhất là lớp tốt nhất; trong lớp đó, chọn $v$ nhỏ nhất.

<!-- thinking:end -->

Ta duyệt mảng $nums$ và sử dụng một hash table $cnt$ để đếm tần suất của mỗi số modulo $space$. Tần suất càng cao thì càng phá hủy được nhiều mục tiêu. Ta tìm nhóm có tần suất cao nhất và lấy giá trị nhỏ nhất trong nhóm đó.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def destroyTargets(self, nums: List[int], space: int) -> int:
        cnt = Counter(v % space for v in nums)
        ans = mx = 0
        for v in nums:
            t = cnt[v % space]
            if t > mx or (t == mx and v < ans):
                ans = v
                mx = t
        return ans
```

#### Java

```java
class Solution {
    public int destroyTargets(int[] nums, int space) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int v : nums) {
            v %= space;
            cnt.put(v, cnt.getOrDefault(v, 0) + 1);
        }
        int ans = 0, mx = 0;
        for (int v : nums) {
            int t = cnt.get(v % space);
            if (t > mx || (t == mx && v < ans)) {
                ans = v;
                mx = t;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int destroyTargets(vector<int>& nums, int space) {
        unordered_map<int, int> cnt;
        for (int v : nums) ++cnt[v % space];
        int ans = 0, mx = 0;
        for (int v : nums) {
            int t = cnt[v % space];
            if (t > mx || (t == mx && v < ans)) {
                ans = v;
                mx = t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func destroyTargets(nums []int, space int) int {
	cnt := map[int]int{}
	for _, v := range nums {
		cnt[v%space]++
	}
	ans, mx := 0, 0
	for _, v := range nums {
		t := cnt[v%space]
		if t > mx || (t == mx && v < ans) {
			ans = v
			mx = t
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
