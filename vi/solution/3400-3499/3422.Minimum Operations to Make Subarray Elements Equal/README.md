---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Math
    - Sliding Window
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3422. Minimum Operations to Make Subarray Elements Equal 🔒](https://leetcode.com/problems/minimum-operations-to-make-subarray-elements-equal)

[中文文档](/solution/3400-3499/3422.Minimum%20Operations%20to%20Make%20Subarray%20Elements%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý:</p>

<ul>
	<li>Tăng hoặc giảm bất kỳ phần tử nào của <code>nums</code> đi 1.</li>
</ul>

<p>Hãy trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để đảm bảo rằng <strong>ít nhất</strong> một <span data-keyword="subarray">mảng con</span> có kích thước <code>k</code> trong <code>nums</code> có tất cả phần tử bằng nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,-3,2,1,-4,6], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thực hiện 4 thao tác để cộng 4 vào <code>nums[1]</code>. Mảng thu được là <span class="example-io"><code>[4, 1, 2, 1, -4, 6]</code>.</span></li>
	<li><span class="example-io">Thực hiện 1 thao tác để trừ 1 khỏi <code>nums[2]</code>. Mảng thu được là <code>[4, 1, 1, 1, -4, 6]</code>.</span></li>
	<li><span class="example-io">Mảng hiện chứa một mảng con <code>[1, 1, 1]</code> có kích thước <code>k = 3</code> với tất cả phần tử bằng nhau. Do đó, đáp án là 5.</span></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-2,-2,3,1,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>
	<p>Mảng con <code>[-2, -2]</code> có kích thước <code>k = 2</code> đã chứa toàn bộ phần tử bằng nhau, nên không cần thực hiện thao tác nào. Do đó, đáp án là 0.</p>
	</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>6</sup> &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>2 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tập hợp có thứ tự

<!-- thinking:start -->

> **Tư duy**
>
> Làm cho mọi phần tử trong một cửa sổ bằng nhau sẽ tốn ít thao tác nhất khi chọn median; chi phí là khoảng cách $L_1$ đến median. Ta cần tính chi phí đó cho mọi cửa sổ có độ dài $k$ với $n\le 10^5$.
>
> Sắp xếp từng cửa sổ tốn $O(nk\log k)$. Hai phía của median phải được duy trì khi cửa sổ trượt.
>
> Hai ordered set $l$ và $r$ lưu nửa dưới và nửa trên với $|r|-|l|\in\{0,1\}$, nên $\min r$ là median. Tổng của hai phía $s_1,s_2$ cho phép tính khoảng cách trong $O(1)$. Phần tử rời khỏi cửa sổ được xóa khỏi set đang chứa nó.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần tìm một mảng con có độ dài $k$ và làm cho tất cả phần tử trong mảng con bằng nhau với số thao tác ít nhất. Nói cách khác, ta cần tìm một mảng con có độ dài $k$ sao cho số thao tác ít nhất để đưa tất cả phần tử trong mảng con về median của $k$ phần tử đó là nhỏ nhất.

Ta có thể dùng hai ordered set $l$ và $r$ để duy trì hai phần trái và phải của $k$ phần tử. $l$ dùng để lưu phần nhỏ hơn của $k$ phần tử, còn $r$ dùng để lưu phần lớn hơn của $k$ phần tử. Số phần tử trong $l$ hoặc bằng số phần tử trong $r$, hoặc ít hơn số phần tử trong $r$ một phần tử, nên giá trị nhỏ nhất trong $r$ là median của $k$ phần tử.

Độ phức tạp thời gian là $O(n \times \log k)$, và độ phức tạp không gian là $O(k)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        l = SortedList()
        r = SortedList()
        s1 = s2 = 0
        ans = inf
        for i, x in enumerate(nums):
            l.add(x)
            s1 += x
            y = l.pop()
            s1 -= y
            r.add(y)
            s2 += y
            if len(r) - len(l) > 1:
                y = r.pop(0)
                s2 -= y
                l.add(y)
                s1 += y
            if i >= k - 1:
                ans = min(ans, s2 - r[0] * len(r) + r[0] * len(l) - s1)
                j = i - k + 1
                if nums[j] in r:
                    r.remove(nums[j])
                    s2 -= nums[j]
                else:
                    l.remove(nums[j])
                    s1 -= nums[j]
        return ans
```

#### Java

```java
class Solution {
    public long minOperations(int[] nums, int k) {
        TreeMap<Integer, Integer> l = new TreeMap<>();
        TreeMap<Integer, Integer> r = new TreeMap<>();
        long s1 = 0, s2 = 0;
        int sz1 = 0, sz2 = 0;
        long ans = Long.MAX_VALUE;
        for (int i = 0; i < nums.length; ++i) {
            l.merge(nums[i], 1, Integer::sum);
            s1 += nums[i];
            ++sz1;
            int y = l.lastKey();
            if (l.merge(y, -1, Integer::sum) == 0) {
                l.remove(y);
            }
            s1 -= y;
            --sz1;
            r.merge(y, 1, Integer::sum);
            s2 += y;
            ++sz2;
            if (sz2 - sz1 > 1) {
                y = r.firstKey();
                if (r.merge(y, -1, Integer::sum) == 0) {
                    r.remove(y);
                }
                s2 -= y;
                --sz2;
                l.merge(y, 1, Integer::sum);
                s1 += y;
                ++sz1;
            }
            if (i >= k - 1) {
                ans = Math.min(ans, s2 - r.firstKey() * sz2 + r.firstKey() * sz1 - s1);
                int j = i - k + 1;
                if (r.containsKey(nums[j])) {
                    if (r.merge(nums[j], -1, Integer::sum) == 0) {
                        r.remove(nums[j]);
                    }
                    s2 -= nums[j];
                    --sz2;
                } else {
                    if (l.merge(nums[j], -1, Integer::sum) == 0) {
                        l.remove(nums[j]);
                    }
                    s1 -= nums[j];
                    --sz1;
                }
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
    long long minOperations(vector<int>& nums, int k) {
        multiset<int> l, r;
        long long s1 = 0, s2 = 0, ans = 1e18;
        for (int i = 0; i < nums.size(); ++i) {
            l.insert(nums[i]);
            s1 += nums[i];
            int y = *l.rbegin();
            l.erase(l.find(y));
            s1 -= y;
            r.insert(y);
            s2 += y;
            if (r.size() - l.size() > 1) {
                y = *r.begin();
                r.erase(r.find(y));
                s2 -= y;
                l.insert(y);
                s1 += y;
            }
            if (i >= k - 1) {
                long long x = *r.begin();
                ans = min(ans, s2 - x * (int) r.size() + x * (int) l.size() - s1);
                int j = i - k + 1;
                if (r.contains(nums[j])) {
                    r.erase(r.find(nums[j]));
                    s2 -= nums[j];
                } else {
                    l.erase(l.find(nums[j]));
                    s1 -= nums[j];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int, k int) int64 {
	l := redblacktree.New[int, int]()
	r := redblacktree.New[int, int]()
	merge := func(st *redblacktree.Tree[int, int], x, v int) {
		c, _ := st.Get(x)
		if c+v == 0 {
			st.Remove(x)
		} else {
			st.Put(x, c+v)
		}
	}
	var s1, s2, sz1, sz2 int
	ans := math.MaxInt64
	for i, x := range nums {
		merge(l, x, 1)
		s1 += x
		y := l.Right().Key
		merge(l, y, -1)
		s1 -= y
		merge(r, y, 1)
		s2 += y
		sz2++
		if sz2-sz1 > 1 {
			y = r.Left().Key
			merge(r, y, -1)
			s2 -= y
			sz2--
			merge(l, y, 1)
			s1 += y
			sz1++
		}
		if j := i - k + 1; j >= 0 {
			ans = min(ans, s2-r.Left().Key*sz2+r.Left().Key*sz1-s1)
			if _, ok := r.Get(nums[j]); ok {
				merge(r, nums[j], -1)
				s2 -= nums[j]
				sz2--
			} else {
				merge(l, nums[j], -1)
				s1 -= nums[j]
				sz1--
			}
		}
	}
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
