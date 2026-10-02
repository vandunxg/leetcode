---
comments: true
difficulty: Medium
rating: 1680
source: Weekly Contest 133 Q3
tags:
    - Array
    - Dynamic Programming
    - Sliding Window
---

<!-- problem:start -->

# [1031. Maximum Sum of Two Non-Overlapping Subarrays](https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays)

[中文文档](/solution/1000-1099/1031.Maximum%20Sum%20of%20Two%20Non-Overlapping%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và hai số nguyên <code>firstLen</code>, <code>secondLen</code>, hãy trả về <em>tổng lớn nhất của hai </em><strong>mảng con</strong><em> không giao nhau, có độ dài lần lượt là </em><code>firstLen</code><em> và </em><code>secondLen</code>.</p>

<p>Mảng con có độ dài <code>firstLen</code> có thể nằm trước hoặc sau mảng con có độ dài <code>secondLen</code>, nhưng hai mảng con không được giao nhau.</p>

<p><strong>Mảng con</strong> là một phần <strong>liên tiếp</strong> của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,6,5,2,2,5,1,9,4], firstLen = 1, secondLen = 2
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Một cách chọn là mảng con [9] có độ dài 1 và [6,5] có độ dài 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,8,1,3,2,1,8,9,0], firstLen = 3, secondLen = 2
<strong>Đầu ra:</strong> 29
<strong>Giải thích:</strong> Một cách chọn là mảng con [3,8,1] có độ dài 3 và [8,9] có độ dài 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,5,6,0,9,5,0,3,8], firstLen = 4, secondLen = 3
<strong>Đầu ra:</strong> 31
<strong>Giải thích:</strong> Một cách chọn là mảng con [5,6,0,9] có độ dài 4 và [0,3,8] có độ dài 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= firstLen, secondLen &lt;= 1000</code></li>
	<li><code>2 &lt;= firstLen + secondLen &lt;= 1000</code></li>
	<li><code>firstLen + secondLen &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt mọi cặp đoạn có độ phức tạp $O(n^2)$, vừa đủ với $n\le 1000$. Khi cố định một đoạn, ta chỉ cần tìm cửa sổ có độ dài cố định tốt nhất ở phía còn lại.
>
> Prefix sum cho phép tính tổng của bất kỳ cửa sổ nào trong $O(1)$. Ta xét riêng hai trường hợp: “$\textit{firstLen}$ ở bên trái” và “$\textit{secondLen}$ ở bên trái”, đồng thời lưu tổng cửa sổ bên trái tốt nhất vào $t$ khi cửa sổ bên phải trượt.
>
> Đáp án là giá trị lớn hơn trong hai lượt xét.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumTwoNoOverlap(self, nums: List[int], firstLen: int, secondLen: int) -> int:
        n = len(nums)
        s = list(accumulate(nums, initial=0))
        ans = t = 0
        i = firstLen
        while i + secondLen - 1 < n:
            t = max(t, s[i] - s[i - firstLen])
            ans = max(ans, t + s[i + secondLen] - s[i])
            i += 1
        t = 0
        i = secondLen
        while i + firstLen - 1 < n:
            t = max(t, s[i] - s[i - secondLen])
            ans = max(ans, t + s[i + firstLen] - s[i])
            i += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxSumTwoNoOverlap(int[] nums, int firstLen, int secondLen) {
        int n = nums.length;
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int ans = 0;
        for (int i = firstLen, t = 0; i + secondLen - 1 < n; ++i) {
            t = Math.max(t, s[i] - s[i - firstLen]);
            ans = Math.max(ans, t + s[i + secondLen] - s[i]);
        }
        for (int i = secondLen, t = 0; i + firstLen - 1 < n; ++i) {
            t = Math.max(t, s[i] - s[i - secondLen]);
            ans = Math.max(ans, t + s[i + firstLen] - s[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSumTwoNoOverlap(vector<int>& nums, int firstLen, int secondLen) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int ans = 0;
        for (int i = firstLen, t = 0; i + secondLen - 1 < n; ++i) {
            t = max(t, s[i] - s[i - firstLen]);
            ans = max(ans, t + s[i + secondLen] - s[i]);
        }
        for (int i = secondLen, t = 0; i + firstLen - 1 < n; ++i) {
            t = max(t, s[i] - s[i - secondLen]);
            ans = max(ans, t + s[i + firstLen] - s[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func maxSumTwoNoOverlap(nums []int, firstLen int, secondLen int) (ans int) {
	n := len(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	for i, t := firstLen, 0; i+secondLen-1 < n; i++ {
		t = max(t, s[i]-s[i-firstLen])
		ans = max(ans, t+s[i+secondLen]-s[i])
	}
	for i, t := secondLen, 0; i+firstLen-1 < n; i++ {
		t = max(t, s[i]-s[i-secondLen])
		ans = max(ans, t+s[i+firstLen]-s[i])
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
