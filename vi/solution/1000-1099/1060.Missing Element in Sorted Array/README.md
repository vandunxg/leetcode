---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1060. Missing Element in Sorted Array 🔒](https://leetcode.com/problems/missing-element-in-sorted-array)

[中文文档](/solution/1000-1099/1060.Missing%20Element%20in%20Sorted%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> được sắp xếp theo <strong>thứ tự tăng dần</strong>, các phần tử đều <strong>khác nhau</strong>, cùng số nguyên <code>k</code>. Hãy trả về số bị thiếu thứ <code>k<sup>th</sup></code> khi đếm bắt đầu từ giá trị ngoài cùng bên trái của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,7,9,10], k = 1
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Số đầu tiên bị thiếu là 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,7,9,10], k = 3
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Các số bị thiếu là [5,6,8,...], nên số bị thiếu thứ ba là 8.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,4], k = 3
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các số bị thiếu là [3,5,6,7,...], nên số bị thiếu thứ ba là 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
	<li><code>nums</code> được sắp xếp theo <strong>thứ tự tăng dần</strong>, và mọi phần tử đều <strong>khác nhau</strong>.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>8</sup></code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm lời giải có độ phức tạp thời gian logarit (tức <code>O(log(n))</code>) không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng đã được sắp xếp và không có phần tử trùng nhau, nên số lượng giá trị bị thiếu trước chỉ số $i$ là $nums[i]-nums[0]-i$. Duyệt tuyến tính có thể tìm số bị thiếu thứ $k$, nhưng câu hỏi mở rộng yêu cầu độ phức tạp logarit.
>
> $\textit{missing}(i)$ tăng dần. Nếu $k$ lớn hơn số lượng giá trị bị thiếu đến cuối mảng, đáp án nằm sau mảng; nếu không, ta tìm kiếm nhị phân chỉ số nhỏ nhất $i$ thỏa mãn $\textit{missing}(i)\ge k$ rồi cộng phần chênh lệch còn lại vào $nums[i-1]$.
>
> Phạm vi tìm kiếm là $[0,n-1]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingElement(self, nums: List[int], k: int) -> int:
        def missing(i: int) -> int:
            return nums[i] - nums[0] - i

        n = len(nums)
        if k > missing(n - 1):
            return nums[n - 1] + k - missing(n - 1)
        l, r = 0, n - 1
        while l < r:
            mid = (l + r) >> 1
            if missing(mid) >= k:
                r = mid
            else:
                l = mid + 1
        return nums[l - 1] + k - missing(l - 1)
```

#### Java

```java
class Solution {
    public int missingElement(int[] nums, int k) {
        int n = nums.length;
        if (k > missing(nums, n - 1)) {
            return nums[n - 1] + k - missing(nums, n - 1);
        }
        int l = 0, r = n - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (missing(nums, mid) >= k) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return nums[l - 1] + k - missing(nums, l - 1);
    }

    private int missing(int[] nums, int i) {
        return nums[i] - nums[0] - i;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int missingElement(vector<int>& nums, int k) {
        auto missing = [&](int i) {
            return nums[i] - nums[0] - i;
        };
        int n = nums.size();
        if (k > missing(n - 1)) {
            return nums[n - 1] + k - missing(n - 1);
        }
        int l = 0, r = n - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (missing(mid) >= k) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return nums[l - 1] + k - missing(l - 1);
    }
};
```

#### Go

```go
func missingElement(nums []int, k int) int {
	missing := func(i int) int {
		return nums[i] - nums[0] - i
	}
	n := len(nums)
	if k > missing(n-1) {
		return nums[n-1] + k - missing(n-1)
	}
	l, r := 0, n-1
	for l < r {
		mid := (l + r) >> 1
		if missing(mid) >= k {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return nums[l-1] + k - missing(l-1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
