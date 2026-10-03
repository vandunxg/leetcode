---
comments: true
difficulty: Medium
rating: 2011
source: Biweekly Contest 82 Q3
tags:
    - Greedy
    - Array
    - Binary Search
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2333. Minimum Sum of Squared Difference](https://leetcode.com/problems/minimum-sum-of-squared-difference)

[中文文档](/solution/2300-2399/2333.Minimum%20Sum%20of%20Squared%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên dương <strong>được đánh chỉ số từ 0</strong> <code>nums1</code> và <code>nums2</code>, cả hai đều có độ dài <code>n</code>.</p>

<p><strong>Tổng bình phương hiệu</strong> của hai mảng <code>nums1</code> và <code>nums2</code> được định nghĩa là <strong>tổng</strong> của <code>(nums1[i] - nums2[i])<sup>2</sup></code> với mỗi <code>0 &lt;= i &lt; n</code>.</p>

<p>Bạn cũng được cho hai số nguyên dương <code>k1</code> và <code>k2</code>. Bạn có thể thay đổi bất kỳ phần tử nào của <code>nums1</code> bằng <code>+1</code> hoặc <code>-1</code> nhiều nhất <code>k1</code> lần. Tương tự, bạn có thể thay đổi bất kỳ phần tử nào của <code>nums2</code> bằng <code>+1</code> hoặc <code>-1</code> nhiều nhất <code>k2</code> lần.</p>

<p>Trả về <em>giá trị <strong>tổng bình phương hiệu</strong> nhỏ nhất sau khi thay đổi mảng </em><code>nums1</code><em> nhiều nhất </em><code>k1</code><em> lần và thay đổi mảng </em><code>nums2</code><em> nhiều nhất </em><code>k2</code><em> lần</em>.</p>

<p><strong>Lưu ý</strong>: Bạn được phép thay đổi các phần tử của mảng thành <strong>số nguyên âm</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,3,4], nums2 = [2,10,20,19], k1 = 0, k2 = 0
<strong>Đầu ra:</strong> 579
<strong>Giải thích:</strong> Các phần tử trong nums1 không thể được thay đổi vì k1 = 0 và k2 = 0.
Tổng bình phương hiệu sẽ là: (1 - 2)<sup>2 </sup>+ (2 - 10)<sup>2 </sup>+ (3 - 20)<sup>2 </sup>+ (4 - 19)<sup>2</sup>&nbsp;= 579.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,4,10,12], nums2 = [5,8,6,9], k1 = 1, k2 = 1
<strong>Đầu ra:</strong> 43
<strong>Giải thích:</strong> Một cách để đạt được tổng bình phương hiệu nhỏ nhất là:
- Tăng nums1[0] một lần.
- Tăng nums2[2] một lần.
Tổng bình phương hiệu nhỏ nhất sẽ là:
(2 - 5)<sup>2 </sup>+ (4 - 8)<sup>2 </sup>+ (10 - 7)<sup>2 </sup>+ (12 - 9)<sup>2</sup>&nbsp;= 43.
Lưu ý rằng có những cách khác để đạt được tổng bình phương hiệu nhỏ nhất, nhưng không có cách nào đạt được tổng nhỏ hơn 43.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k1, k2 &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần điều chỉnh làm giảm một giá trị $|nums1_i-nums2_i|$ đi một đơn vị, tổng cộng thực hiện $k_1+k_2$ lần, từ đó giảm tổng bình phương. Với $n \le 10^5$ và có thể có tới $2 \times 10^9$ lần điều chỉnh, không thể thực hiện tuần tự từng lần.
>
> Hàm bình phương là hàm lồi, vì vậy nên san phẳng các độ lệch lớn trước. Dùng tìm kiếm nhị phân để tìm một ngưỡng $x$ mà $k$ lần điều chỉnh có thể đưa các độ lệch về, sau đó dùng số lần còn lại cho các phần tử vẫn bằng $x$. Nếu tổng độ lệch đã $\le k$, đáp án là $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSumSquareDiff(
        self, nums1: List[int], nums2: List[int], k1: int, k2: int
    ) -> int:
        d = [abs(a - b) for a, b in zip(nums1, nums2)]
        k = k1 + k2
        if sum(d) <= k:
            return 0
        left, right = 0, max(d)
        while left < right:
            mid = (left + right) >> 1
            if sum(max(v - mid, 0) for v in d) <= k:
                right = mid
            else:
                left = mid + 1
        for i, v in enumerate(d):
            d[i] = min(left, v)
            k -= max(0, v - left)
        for i, v in enumerate(d):
            if k == 0:
                break
            if v == left:
                k -= 1
                d[i] -= 1
        return sum(v * v for v in d)
```

#### Java

```java
class Solution {
    public long minSumSquareDiff(int[] nums1, int[] nums2, int k1, int k2) {
        int n = nums1.length;
        int[] d = new int[n];
        long s = 0;
        int mx = 0;
        int k = k1 + k2;
        for (int i = 0; i < n; ++i) {
            d[i] = Math.abs(nums1[i] - nums2[i]);
            s += d[i];
            mx = Math.max(mx, d[i]);
        }
        if (s <= k) {
            return 0;
        }
        int left = 0, right = mx;
        while (left < right) {
            int mid = (left + right) >> 1;
            long t = 0;
            for (int v : d) {
                t += Math.max(v - mid, 0);
            }
            if (t <= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        for (int i = 0; i < n; ++i) {
            k -= Math.max(0, d[i] - left);
            d[i] = Math.min(d[i], left);
        }
        for (int i = 0; i < n && k > 0; ++i) {
            if (d[i] == left) {
                --k;
                --d[i];
            }
        }
        long ans = 0;
        for (int v : d) {
            ans += (long) v * v;
        }
        return ans;
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    long long minSumSquareDiff(vector<int>& nums1, vector<int>& nums2, int k1, int k2) {
        int n = nums1.size();
        vector<int> d(n);
        ll s = 0;
        int mx = 0;
        int k = k1 + k2;
        for (int i = 0; i < n; ++i) {
            d[i] = abs(nums1[i] - nums2[i]);
            s += d[i];
            mx = max(mx, d[i]);
        }
        if (s <= k) return 0;
        int left = 0, right = mx;
        while (left < right) {
            int mid = (left + right) >> 1;
            ll t = 0;
            for (int v : d) t += max(v - mid, 0);
            if (t <= k)
                right = mid;
            else
                left = mid + 1;
        }
        for (int i = 0; i < n; ++i) {
            k -= max(0, d[i] - left);
            d[i] = min(d[i], left);
        }
        for (int i = 0; i < n && k; ++i) {
            if (d[i] == left) {
                --k;
                --d[i];
            }
        }
        ll ans = 0;
        for (int v : d) ans += 1ll * v * v;
        return ans;
    }
};
```

#### Go

```go
func minSumSquareDiff(nums1 []int, nums2 []int, k1 int, k2 int) int64 {
	k := k1 + k2
	s, mx := 0, 0
	n := len(nums1)
	d := make([]int, n)
	for i, v := range nums1 {
		d[i] = abs(v - nums2[i])
		s += d[i]
		mx = max(mx, d[i])
	}
	if s <= k {
		return 0
	}
	left, right := 0, mx
	for left < right {
		mid := (left + right) >> 1
		t := 0
		for _, v := range d {
			t += max(v-mid, 0)
		}
		if t <= k {
			right = mid
		} else {
			left = mid + 1
		}
	}
	for i, v := range d {
		k -= max(v-left, 0)
		d[i] = min(v, left)
	}
	for i, v := range d {
		if k <= 0 {
			break
		}
		if v == left {
			d[i]--
			k--
		}
	}
	ans := 0
	for _, v := range d {
		ans += v * v
	}
	return int64(ans)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
