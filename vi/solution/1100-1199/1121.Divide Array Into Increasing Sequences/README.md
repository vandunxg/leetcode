---
comments: true
difficulty: Hard
rating: 1664
source: Biweekly Contest 4 Q4
tags:
    - Array
    - Counting
---

<!-- problem:start -->

# [1121. Divide Array Into Increasing Sequences 🔒](https://leetcode.com/problems/divide-array-into-increasing-sequences)

[中文文档](/solution/1100-1199/1121.Divide%20Array%20Into%20Increasing%20Sequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được sắp xếp theo thứ tự không giảm và số nguyên <code>k</code>. Trả về <code>true</code><em> nếu có thể chia mảng thành một hoặc nhiều dãy con tăng không giao nhau, mỗi dãy có độ dài ít nhất </em><code>k</code><em>; nếu không thì trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,2,3,3,4,4], k = 3
<strong>Output:</strong> true
<strong>Giải thích:</strong> Có thể chia mảng thành hai dãy con [1,2,3,4] và [2,3,4], mỗi dãy có độ dài ít nhất 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [5,6,6,7,8], k = 3
<strong>Output:</strong> false
<strong>Giải thích:</strong> Không có cách nào chia mảng thỏa mãn các điều kiện đã cho.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>nums</code> được sắp xếp theo thứ tự không giảm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nhận xét nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Mảng được sắp xếp không giảm. Một dãy con tăng nghiêm ngặt không thể chứa giá trị lặp lại, nên các lần xuất hiện của giá trị phổ biến nhất phải nằm trong những dãy con khác nhau. Nếu giá trị đó xuất hiện $cnt$ lần thì cần ít nhất $cnt$ dãy con, mỗi dãy dài ít nhất $k$, tức là $cnt\times k\le n$.
>
> Các giá trị bằng nhau vốn tạo thành những đoạn liên tiếp, nên `groupby` tìm được đoạn dài nhất mà không cần hash table.

<!-- thinking:end -->

Giả sử có thể chia mảng thành $m$ dãy con tăng nghiêm ngặt, mỗi dãy dài ít nhất $k$. Nếu giá trị xuất hiện nhiều nhất trong mảng có tần suất $cnt$, thì $cnt$ lần xuất hiện này phải nằm trong các dãy con khác nhau, nên $m \geq cnt$. Mặt khác, vì mỗi dãy con có độ dài ít nhất $k$, số dãy con càng ít càng tốt; do đó $m = cnt$. Vì vậy, điều kiện cần thỏa mãn là $cnt \times k \leq n$. Ta chỉ cần tìm tần suất lớn nhất $cnt$ trong mảng rồi kiểm tra $cnt \times k \leq n$. Nếu đúng, trả về `true`; nếu không, trả về `false`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, với $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canDivideIntoSubsequences(self, nums: List[int], k: int) -> bool:
        mx = max(len(list(x)) for _, x in groupby(nums))
        return mx * k <= len(nums)
```

#### Java

```java
class Solution {
    public boolean canDivideIntoSubsequences(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, cnt.merge(x, 1, Integer::sum));
        }
        return mx * k <= nums.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canDivideIntoSubsequences(vector<int>& nums, int k) {
        int cnt = 0;
        int a = 0;
        for (int& b : nums) {
            cnt = a == b ? cnt + 1 : 1;
            if (cnt * k > nums.size()) {
                return false;
            }
            a = b;
        }
        return true;
    }
};
```

#### Go

```go
func canDivideIntoSubsequences(nums []int, k int) bool {
	cnt, a := 0, 0
	for _, b := range nums {
		cnt++
		if a != b {
			cnt = 1
		}
		if cnt*k > len(nums) {
			return false
		}
		a = b
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 tìm tần suất lớn nhất trên toàn mảng trước. Cách 2 theo dõi độ dài đoạn hiện tại và trả về false ngay khi $cnt\times k>n$. Tiêu chí vẫn như nhau, nhưng cách triển khai chỉ cần một lượt quét tuyến tính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Java

```java
class Solution {
    public boolean canDivideIntoSubsequences(int[] nums, int k) {
        int cnt = 0;
        int a = 0;
        for (int b : nums) {
            cnt = a == b ? cnt + 1 : 1;
            if (cnt * k > nums.length) {
                return false;
            }
            a = b;
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
