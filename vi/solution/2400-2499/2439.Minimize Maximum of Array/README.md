---
comments: true
difficulty: Medium
rating: 1965
source: Biweekly Contest 89 Q3
tags:
    - Greedy
    - Array
    - Binary Search
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [2439. Minimize Maximum of Array](https://leetcode.com/problems/minimize-maximum-of-array)

[中文文档](/solution/2400-2499/2439.Minimize%20Maximum%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>được đánh chỉ số từ 0</strong> <code>nums</code> gồm <code>n</code> số nguyên không âm.</p>

<p>Trong một thao tác, bạn phải:</p>

<ul>
	<li>Chọn một số nguyên <code>i</code> sao cho <code>1 &lt;= i &lt; n</code> và <code>nums[i] &gt; 0</code>.</li>
	<li>Giảm <code>nums[i]</code> đi 1.</li>
	<li>Tăng <code>nums[i - 1]</code> lên 1.</li>
</ul>

<p>Trả về <em>giá trị <strong>nhỏ nhất</strong> có thể có của <strong>phần tử lớn nhất</strong> trong </em><code>nums</code><em> sau khi thực hiện <strong>bất kỳ</strong> số thao tác nào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,7,1,6]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Một chuỗi thao tác tối ưu như sau:
1. Chọn i = 1, khi đó nums trở thành [4,6,1,6].
2. Chọn i = 3, khi đó nums trở thành [4,6,2,5].
3. Chọn i = 1, khi đó nums trở thành [5,5,2,5].
Phần tử lớn nhất trong nums là 5. Có thể chứng minh rằng giá trị lớn nhất không thể nhỏ hơn 5.
Do đó, ta trả về 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,1]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong>
Giữ nguyên nums là tối ưu, và vì 10 là giá trị lớn nhất, ta trả về 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác chuyển giá trị từ $i$ sang $i-1$, tức là phần dư chảy sang trái. Với $n\le 10^5$, việc tối thiểu hóa giá trị lớn nhất cuối cùng là tìm kiếm nhị phân trên giới hạn $mx$.
>
> Duyệt từ phải sang trái: mọi giá trị vượt quá $mx$ đều được chuyển tiếp. Giá trị đoán là khả thi khi $nums[0]$ cộng với phần dư được chuyển tiếp vẫn $\le mx$.

<!-- thinking:end -->

Để tối thiểu hóa giá trị lớn nhất của mảng, việc sử dụng tìm kiếm nhị phân là một lựa chọn tự nhiên. Ta tìm kiếm nhị phân giá trị lớn nhất $mx$ của mảng, rồi tìm giá trị $mx$ nhỏ nhất thỏa mãn các yêu cầu của bài toán.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài của mảng và $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeArrayValue(self, nums: List[int]) -> int:
        def check(mx):
            d = 0
            for x in nums[:0:-1]:
                d = max(0, d + x - mx)
            return nums[0] + d <= mx

        left, right = 0, max(nums)
        while left < right:
            mid = (left + right) >> 1
            if check(mid):
                right = mid
            else:
                left = mid + 1
        return left
```

#### Java

```java
class Solution {
    private int[] nums;

    public int minimizeArrayValue(int[] nums) {
        this.nums = nums;
        int left = 0, right = max(nums);
        while (left < right) {
            int mid = (left + right) >> 1;
            if (check(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private boolean check(int mx) {
        long d = 0;
        for (int i = nums.length - 1; i > 0; --i) {
            d = Math.max(0, d + nums[i] - mx);
        }
        return nums[0] + d <= mx;
    }

    private int max(int[] nums) {
        int v = nums[0];
        for (int x : nums) {
            v = Math.max(v, x);
        }
        return v;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimizeArrayValue(vector<int>& nums) {
        int left = 0, right = *max_element(nums.begin(), nums.end());
        auto check = [&](int mx) {
            long d = 0;
            for (int i = nums.size() - 1; i; --i) {
                d = max(0l, d + nums[i] - mx);
            }
            return nums[0] + d <= mx;
        };
        while (left < right) {
            int mid = (left + right) >> 1;
            if (check(mid))
                right = mid;
            else
                left = mid + 1;
        }
        return left;
    }
};
```

#### Go

```go
func minimizeArrayValue(nums []int) int {
	check := func(mx int) bool {
		d := 0
		for i := len(nums) - 1; i > 0; i-- {
			d = max(0, nums[i]+d-mx)
		}
		return nums[0]+d <= mx
	}

	left, right := 0, slices.Max(nums)
	for left < right {
		mid := (left + right) >> 1
		if check(mid) {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return left
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
