---
comments: true
difficulty: Medium
tags:
    - Array
---

<!-- problem:start -->

# [915. Partition Array into Disjoint Intervals](https://leetcode.com/problems/partition-array-into-disjoint-intervals)

[中文文档](/solution/0900-0999/0915.Partition%20Array%20into%20Disjoint%20Intervals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, chia mảng thành hai mảng con (liên tiếp) <code>left</code> và <code>right</code> sao cho:</p>

<ul>
	<li>Mọi phần tử trong <code>left</code> đều nhỏ hơn hoặc bằng mọi phần tử trong <code>right</code>.</li>
	<li><code>left</code> và <code>right</code> đều không rỗng.</li>
	<li><code>left</code> có kích thước nhỏ nhất có thể.</li>
</ul>

<p>Trả về <em>độ dài của </em><code>left</code><em> sau khi chia như trên</em>.</p>

<p>Các test case được tạo sao cho luôn tồn tại cách chia hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [5,0,3,8,6]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> left = [5,0,3], right = [8,6]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,1,1,0,6,12]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> left = [1,1,1,0], right = [6,12]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li>Đầu vào luôn có ít nhất một đáp án hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Maximum + Suffix Minimum

<!-- thinking:start -->

> **Tư duy**
>
> Điểm chia $i$ hợp lệ khi giá trị lớn nhất bên trái không vượt quá giá trị nhỏ nhất bên phải. Vì $n\le 10^5$, không thể duyệt lại cả hai phía với mỗi $i$. Tính trước suffix minimum, rồi duyệt từ trái sang phải và theo dõi prefix maximum; chỉ số đầu tiên thỏa $mx\le mi[i]$ cho đoạn trái ngắn nhất (đảm bảo luôn có điểm chia).

<!-- thinking:end -->

Để thỏa điều kiện sau khi chia thành hai mảng con, cần bảo đảm "giá trị lớn nhất của prefix" nhỏ hơn hoặc bằng "giá trị nhỏ nhất của suffix".

Vì vậy, trước tiên ta tính trước giá trị nhỏ nhất của suffix và lưu vào mảng `mi`.

Sau đó, duyệt mảng từ đầu đến cuối và duy trì giá trị lớn nhất `mx` của prefix. Khi giá trị lớn nhất của prefix nhỏ hơn hoặc bằng giá trị nhỏ nhất của suffix tại một vị trí, đó là điểm chia; ta có thể trả về vị trí này ngay.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def partitionDisjoint(self, nums: List[int]) -> int:
        n = len(nums)
        mi = [inf] * (n + 1)
        for i in range(n - 1, -1, -1):
            mi[i] = min(nums[i], mi[i + 1])
        mx = 0
        for i, v in enumerate(nums, 1):
            mx = max(mx, v)
            if mx <= mi[i]:
                return i
```

#### Java

```java
class Solution {
    public int partitionDisjoint(int[] nums) {
        int n = nums.length;
        int[] mi = new int[n + 1];
        mi[n] = nums[n - 1];
        for (int i = n - 1; i >= 0; --i) {
            mi[i] = Math.min(nums[i], mi[i + 1]);
        }
        int mx = 0;
        for (int i = 1;; ++i) {
            int v = nums[i - 1];
            mx = Math.max(mx, v);
            if (mx <= mi[i]) {
                return i;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int partitionDisjoint(vector<int>& nums) {
        int n = nums.size();
        vector<int> mi(n + 1, INT_MAX);
        for (int i = n - 1; ~i; --i) {
            mi[i] = min(nums[i], mi[i + 1]);
        }
        int mx = 0;
        for (int i = 1;; ++i) {
            int v = nums[i - 1];
            mx = max(mx, v);
            if (mx <= mi[i]) {
                return i;
            }
        }
    }
};
```

#### Go

```go
func partitionDisjoint(nums []int) int {
	n := len(nums)
	mi := make([]int, n+1)
	mi[n] = nums[n-1]
	for i := n - 1; i >= 0; i-- {
		mi[i] = min(nums[i], mi[i+1])
	}
	mx := 0
	for i := 1; ; i++ {
		v := nums[i-1]
		mx = max(mx, v)
		if mx <= mi[i] {
			return i
		}
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
