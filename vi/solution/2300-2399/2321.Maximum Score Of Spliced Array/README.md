---
comments: true
difficulty: Hard
rating: 1790
source: Weekly Contest 299 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2321. Maximum Score Of Spliced Array](https://leetcode.com/problems/maximum-score-of-spliced-array)

[中文文档](/solution/2300-2399/2321.Maximum%20Score%20Of%20Spliced%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums1</code> và <code>nums2</code>, cả hai đều có độ dài <code>n</code>.</p>

<p>Bạn có thể chọn hai số nguyên <code>left</code> và <code>right</code> sao cho <code>0 &lt;= left &lt;= right &lt; n</code>, rồi <strong>hoán đổi</strong> mảng con <code>nums1[left...right]</code> với mảng con <code>nums2[left...right]</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>nums1 = [1,2,3,4,5]</code> và <code>nums2 = [11,12,13,14,15]</code>, đồng thời chọn <code>left = 1</code> và <code>right = 2</code>, thì <code>nums1</code> trở thành <code>[1,<strong><u>12,13</u></strong>,4,5]</code> và <code>nums2</code> trở thành <code>[11,<strong><u>2,3</u></strong>,14,15]</code>.</li>
</ul>

<p>Bạn có thể chọn thực hiện thao tác trên <strong>đúng một lần</strong> hoặc không làm gì.</p>

<p><strong>Điểm số</strong> của hai mảng là <strong>giá trị lớn hơn</strong> giữa <code>sum(nums1)</code> và <code>sum(nums2)</code>, trong đó <code>sum(arr)</code> là tổng của tất cả các phần tử trong mảng <code>arr</code>.</p>

<p>Trả về <em><strong>điểm số lớn nhất có thể đạt được</strong></em>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp trong một mảng. <code>arr[left...right]</code> biểu thị mảng con chứa các phần tử của <code>nums</code> từ chỉ số <code>left</code> đến <code>right</code> (<strong>bao gồm cả hai đầu</strong>).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [60,60,60], nums2 = [10,90,10]
<strong>Đầu ra:</strong> 210
<strong>Giải thích:</strong> Chọn left = 1 và right = 1, ta có nums1 = [60,<u><strong>90</strong></u>,60] và nums2 = [10,<u><strong>60</strong></u>,10].
Điểm số là max(sum(nums1), sum(nums2)) = max(210, 80) = 210.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [20,40,20,70,30], nums2 = [50,20,50,40,20]
<strong>Đầu ra:</strong> 220
<strong>Giải thích:</strong> Chọn left = 3, right = 4, ta có nums1 = [20,40,20,<u><strong>40,20</strong></u>] và nums2 = [50,20,50,<u><strong>70,30</strong></u>].
Điểm số là max(sum(nums1), sum(nums2)) = max(140, 220) = 220.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [7,11,13], nums2 = [1,1,1]
<strong>Đầu ra:</strong> 31
<strong>Giải thích:</strong> Ta chọn không hoán đổi mảng con nào.
Điểm số là max(sum(nums1), sum(nums2)) = max(31, 3) = 31.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể hoán đổi một mảng con tương ứng và muốn tối đa hóa tổng của mảng lớn hơn. Vì $n \le 10^5$, không thể liệt kê tất cả các đoạn. Sau khi hoán đổi $[l,r]$, $nums2$ tăng thêm tổng trên đoạn của $nums1_i-nums2_i$.
>
> Đây là bài toán mảng con có tổng lớn nhất. Áp dụng Kadane trên hiệu để tìm mức tăng tốt nhất cho $nums2$; hiệu khi hoán đổi sẽ cho mức tăng của $nums1$. Cộng mỗi mức tăng vào tổng ban đầu tương ứng rồi chọn giá trị lớn hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumsSplicedArray(self, nums1: List[int], nums2: List[int]) -> int:
        def f(nums1, nums2):
            d = [a - b for a, b in zip(nums1, nums2)]
            t = mx = d[0]
            for v in d[1:]:
                if t > 0:
                    t += v
                else:
                    t = v
                mx = max(mx, t)
            return mx

        s1, s2 = sum(nums1), sum(nums2)
        return max(s2 + f(nums1, nums2), s1 + f(nums2, nums1))
```

#### Java

```java
class Solution {
    public int maximumsSplicedArray(int[] nums1, int[] nums2) {
        int s1 = 0, s2 = 0, n = nums1.length;
        for (int i = 0; i < n; ++i) {
            s1 += nums1[i];
            s2 += nums2[i];
        }
        return Math.max(s2 + f(nums1, nums2), s1 + f(nums2, nums1));
    }

    private int f(int[] nums1, int[] nums2) {
        int t = nums1[0] - nums2[0];
        int mx = t;
        for (int i = 1; i < nums1.length; ++i) {
            int v = nums1[i] - nums2[i];
            if (t > 0) {
                t += v;
            } else {
                t = v;
            }
            mx = Math.max(mx, t);
        }
        return mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumsSplicedArray(vector<int>& nums1, vector<int>& nums2) {
        int s1 = 0, s2 = 0, n = nums1.size();
        for (int i = 0; i < n; ++i) {
            s1 += nums1[i];
            s2 += nums2[i];
        }
        return max(s2 + f(nums1, nums2), s1 + f(nums2, nums1));
    }

    int f(vector<int>& nums1, vector<int>& nums2) {
        int t = nums1[0] - nums2[0];
        int mx = t;
        for (int i = 1; i < nums1.size(); ++i) {
            int v = nums1[i] - nums2[i];
            if (t > 0)
                t += v;
            else
                t = v;
            mx = max(mx, t);
        }
        return mx;
    }
};
```

#### Go

```go
func maximumsSplicedArray(nums1 []int, nums2 []int) int {
	s1, s2 := 0, 0
	n := len(nums1)
	for i, v := range nums1 {
		s1 += v
		s2 += nums2[i]
	}
	f := func(nums1, nums2 []int) int {
		t := nums1[0] - nums2[0]
		mx := t
		for i := 1; i < n; i++ {
			v := nums1[i] - nums2[i]
			if t > 0 {
				t += v
			} else {
				t = v
			}
			mx = max(mx, t)
		}
		return mx
	}
	return max(s2+f(nums1, nums2), s1+f(nums2, nums1))
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
