---
comments: true
difficulty: Hard
tags:
    - Array
    - Hash Table
    - Counting
    - Sliding Window
---

<!-- problem:start -->

# [992. Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers)

[中文文档](/solution/0900-0999/0992.Subarrays%20with%20K%20Different%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy trả về <em>số lượng <strong>mảng con tốt</strong> trong </em><code>nums</code>.</p>

<p><strong>Mảng tốt</strong> là mảng có đúng <code>k</code> số nguyên khác nhau.</p>

<ul>
	<li>Ví dụ, <code>[1,2,3,1,2]</code> có <code>3</code> số nguyên khác nhau: <code>1</code>, <code>2</code> và <code>3</code>.</li>
</ul>

<p><strong>Mảng con</strong> là một phần <strong>liên tiếp</strong> của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,2,3], k = 2
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các mảng con có đúng 2 số nguyên khác nhau là: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,3,4], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các mảng con có đúng 3 số nguyên khác nhau là: [1,2,1,3], [2,1,3], [1,3,4].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i], k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số mảng con có đúng $k$ giá trị phân biệt. Vì $n\le 2\times 10^4$, duyệt tất cả khả năng sẽ quá chậm. Số giá trị phân biệt tăng hoặc giữ nguyên khi mở rộng cửa sổ, nên ta có thể dùng sliding window để đếm trường hợp “tối đa $k$”. Với mỗi điểm kết thúc, số mảng con có đúng $k$ giá trị phân biệt bằng hiệu giữa hai ranh giới bắt đầu tương ứng với “tối đa $k-1$” và “tối đa $k$”; cộng các hiệu này cho mọi điểm kết thúc.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subarraysWithKDistinct(self, nums: List[int], k: int) -> int:
        def f(k):
            pos = [0] * len(nums)
            cnt = Counter()
            j = 0
            for i, x in enumerate(nums):
                cnt[x] += 1
                while len(cnt) > k:
                    cnt[nums[j]] -= 1
                    if cnt[nums[j]] == 0:
                        cnt.pop(nums[j])
                    j += 1
                pos[i] = j
            return pos

        return sum(a - b for a, b in zip(f(k - 1), f(k)))
```

#### Java

```java
class Solution {
    public int subarraysWithKDistinct(int[] nums, int k) {
        int[] left = f(nums, k);
        int[] right = f(nums, k - 1);
        int ans = 0;
        for (int i = 0; i < nums.length; ++i) {
            ans += right[i] - left[i];
        }
        return ans;
    }

    private int[] f(int[] nums, int k) {
        int n = nums.length;
        int[] cnt = new int[n + 1];
        int[] pos = new int[n];
        int s = 0;
        for (int i = 0, j = 0; i < n; ++i) {
            if (++cnt[nums[i]] == 1) {
                ++s;
            }
            for (; s > k; ++j) {
                if (--cnt[nums[j]] == 0) {
                    --s;
                }
            }
            pos[i] = j;
        }
        return pos;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subarraysWithKDistinct(vector<int>& nums, int k) {
        vector<int> left = f(nums, k);
        vector<int> right = f(nums, k - 1);
        int ans = 0;
        for (int i = 0; i < nums.size(); ++i) {
            ans += right[i] - left[i];
        }
        return ans;
    }

    vector<int> f(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> pos(n);
        int cnt[n + 1];
        memset(cnt, 0, sizeof(cnt));
        int s = 0;
        for (int i = 0, j = 0; i < n; ++i) {
            if (++cnt[nums[i]] == 1) {
                ++s;
            }
            for (; s > k; ++j) {
                if (--cnt[nums[j]] == 0) {
                    --s;
                }
            }
            pos[i] = j;
        }
        return pos;
    }
};
```

#### Go

```go
func subarraysWithKDistinct(nums []int, k int) (ans int) {
	f := func(k int) []int {
		n := len(nums)
		pos := make([]int, n)
		cnt := make([]int, n+1)
		s, j := 0, 0
		for i, x := range nums {
			cnt[x]++
			if cnt[x] == 1 {
				s++
			}
			for ; s > k; j++ {
				cnt[nums[j]]--
				if cnt[nums[j]] == 0 {
					s--
				}
			}
			pos[i] = j
		}
		return pos
	}
	left, right := f(k), f(k-1)
	for i := range left {
		ans += right[i] - left[i]
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
