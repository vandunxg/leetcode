---
comments: true
difficulty: Hard
rating: 2633
source: Biweekly Contest 93 Q4
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2499. Minimum Total Cost to Make Arrays Unequal](https://leetcode.com/problems/minimum-total-cost-to-make-arrays-unequal)

[中文文档](/solution/2400-2499/2499.Minimum%20Total%20Cost%20to%20Make%20Arrays%20Unequal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums1</code> và <code>nums2</code>, có cùng độ dài <code>n</code>.</p>

<p>Trong một thao tác, bạn có thể hoán đổi giá trị tại hai chỉ số bất kỳ của <code>nums1</code>. <strong>Chi phí</strong> của thao tác này là <strong>tổng các chỉ số</strong>.</p>

<p>Hãy tìm <strong>tổng chi phí nhỏ nhất</strong> khi thực hiện thao tác đã cho <strong>bất kỳ</strong> số lần nào sao cho sau khi hoàn thành mọi thao tác, <code>nums1[i] != nums2[i]</code> với mọi <code>0 &lt;= i &lt;= n - 1</code>.</p>

<p>Trả về <em><strong>tổng chi phí nhỏ nhất</strong> sao cho </em><code>nums1</code> và <code>nums2</code><em> thỏa mãn điều kiện trên</em>. Nếu không thể, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,3,4,5], nums2 = [1,2,3,4,5]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong>
Một trong những cách thực hiện các thao tác là:
- Hoán đổi các giá trị tại chỉ số 0 và 3, chi phí = 0 + 3 = 3. Khi đó, nums1 = [4,2,3,1,5]
- Hoán đổi các giá trị tại chỉ số 1 và 2, chi phí = 1 + 2 = 3. Khi đó, nums1 = [4,3,2,1,5].
- Hoán đổi các giá trị tại chỉ số 0 và 4, chi phí = 0 + 4 = 4. Khi đó, nums1 =[5,3,2,1,4].
Có thể thấy với mỗi chỉ số i, nums1[i] != nums2[i]. Chi phí cần thiết ở đây là 10.
Lưu ý rằng còn có những cách hoán đổi khác, nhưng có thể chứng minh rằng không thể đạt được chi phí nhỏ hơn 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,2,2,1,3], nums2 = [1,2,2,3,3]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong>
Một trong những cách thực hiện các thao tác là:
- Hoán đổi các giá trị tại chỉ số 2 và 3, chi phí = 2 + 3 = 5. Khi đó, nums1 = [2,2,1,2,3].
- Hoán đổi các giá trị tại chỉ số 1 và 4, chi phí = 1 + 4 = 5. Khi đó, nums1 = [2,3,1,2,2].
Tổng chi phí cần thiết là 10, đây là mức nhỏ nhất có thể.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,2], nums2 = [1,2,2]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Có thể chứng minh rằng không thể thỏa mãn các điều kiện đã cho, bất kể thực hiện bao nhiêu thao tác.
Do đó, ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các vị trí mà hai mảng có cùng giá trị phải được hoán đổi; trước tiên ta trả chi phí cho các chỉ số này. Nếu một giá trị xuất hiện ở hơn một nửa số vị trí đó, các vị trí còn lại không thể ghép cặp với nhau và cần thêm các phép hoán đổi với những chỉ số khác.
>
> Đếm các vị trí bằng nhau. Nếu tần suất lớn nhất là $v$ và $2v>same$, cần đưa $2v-same$ vị trí dư ra ngoài. Sau đó chọn các vị trí $a\ne b$ không chứa giá trị lớn nhất đó. Nếu vẫn còn vị trí dư thì không thể thực hiện.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTotalCost(self, nums1: List[int], nums2: List[int]) -> int:
        ans = same = 0
        cnt = Counter()
        for i, (a, b) in enumerate(zip(nums1, nums2)):
            if a == b:
                same += 1
                ans += i
                cnt[a] += 1

        m = lead = 0
        for k, v in cnt.items():
            if v * 2 > same:
                m = v * 2 - same
                lead = k
                break
        for i, (a, b) in enumerate(zip(nums1, nums2)):
            if m and a != b and a != lead and b != lead:
                ans += i
                m -= 1
        return -1 if m else ans
```

#### Java

```java
class Solution {
    public long minimumTotalCost(int[] nums1, int[] nums2) {
        long ans = 0;
        int same = 0;
        int n = nums1.length;
        int[] cnt = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            if (nums1[i] == nums2[i]) {
                ans += i;
                ++same;
                ++cnt[nums1[i]];
            }
        }
        int m = 0, lead = 0;
        for (int i = 0; i < cnt.length; ++i) {
            int t = cnt[i] * 2 - same;
            if (t > 0) {
                m = t;
                lead = i;
                break;
            }
        }
        for (int i = 0; i < n; ++i) {
            if (m > 0 && nums1[i] != nums2[i] && nums1[i] != lead && nums2[i] != lead) {
                ans += i;
                --m;
            }
        }
        return m > 0 ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumTotalCost(vector<int>& nums1, vector<int>& nums2) {
        long long ans = 0;
        int same = 0;
        int n = nums1.size();
        int cnt[n + 1];
        memset(cnt, 0, sizeof cnt);
        for (int i = 0; i < n; ++i) {
            if (nums1[i] == nums2[i]) {
                ans += i;
                ++same;
                ++cnt[nums1[i]];
            }
        }
        int m = 0, lead = 0;
        for (int i = 0; i < n + 1; ++i) {
            int t = cnt[i] * 2 - same;
            if (t > 0) {
                m = t;
                lead = i;
                break;
            }
        }
        for (int i = 0; i < n; ++i) {
            if (m > 0 && nums1[i] != nums2[i] && nums1[i] != lead && nums2[i] != lead) {
                ans += i;
                --m;
            }
        }
        return m > 0 ? -1 : ans;
    }
};
```

#### Go

```go
func minimumTotalCost(nums1 []int, nums2 []int) (ans int64) {
	same, n := 0, len(nums1)
	cnt := make([]int, n+1)
	for i, a := range nums1 {
		b := nums2[i]
		if a == b {
			same++
			ans += int64(i)
			cnt[a]++
		}
	}
	var m, lead int
	for i, v := range cnt {
		if t := v*2 - same; t > 0 {
			m = t
			lead = i
			break
		}
	}
	for i, a := range nums1 {
		b := nums2[i]
		if m > 0 && a != b && a != lead && b != lead {
			ans += int64(i)
			m--
		}
	}
	if m > 0 {
		return -1
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
