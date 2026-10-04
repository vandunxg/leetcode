---
comments: true
difficulty: Medium
rating: 2056
source: Biweekly Contest 96 Q3
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2542. Maximum Subsequence Score](https://leetcode.com/problems/maximum-subsequence-score)

[中文文档](/solution/2500-2599/2542.Maximum%20Subsequence%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>0-indexed</strong> <code>nums1</code> và <code>nums2</code> có cùng độ dài <code>n</code>, cùng một số nguyên dương <code>k</code>. Bạn phải chọn một <strong>dãy con</strong> các chỉ số từ <code>nums1</code> có độ dài <code>k</code>.</p>

<p>Với các chỉ số được chọn <code>i<sub>0</sub></code>, <code>i<sub>1</sub></code>, ..., <code>i<sub>k - 1</sub></code>, <strong>điểm số</strong> được định nghĩa như sau:</p>

<ul>
	<li>Tổng các phần tử được chọn từ <code>nums1</code> nhân với <strong>giá trị nhỏ nhất</strong> trong các phần tử được chọn từ <code>nums2</code>.</li>
	<li>Có thể viết đơn giản là: <code>(nums1[i<sub>0</sub>] + nums1[i<sub>1</sub>] +...+ nums1[i<sub>k - 1</sub>]) * min(nums2[i<sub>0</sub>] , nums2[i<sub>1</sub>], ... ,nums2[i<sub>k - 1</sub>])</code>.</li>
</ul>

<p>Trả về <em><strong>điểm số lớn nhất</strong> có thể đạt được.</em></p>

<p><strong>Dãy con</strong> các chỉ số của một mảng là một tập hợp có thể thu được từ tập <code>{0, 1, ..., n-1}</code> bằng cách xóa một số phần tử hoặc không xóa phần tử nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3
<strong>Output:</strong> 12
<strong>Giải thích:</strong>
Bốn điểm số có thể có của các dãy con là:
- Chọn các chỉ số 0, 1 và 2, điểm số = (1+3+3) * min(2,1,3) = 7.
- Chọn các chỉ số 0, 1 và 3, điểm số = (1+3+2) * min(2,1,4) = 6.
- Chọn các chỉ số 0, 2 và 3, điểm số = (1+3+2) * min(2,3,4) = 12.
- Chọn các chỉ số 1, 2 và 3, điểm số = (3+3+2) * min(1,3,4) = 8.
Do đó, ta trả về điểm số lớn nhất là 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1
<strong>Output:</strong> 30
<strong>Giải thích:</strong>
Chọn chỉ số 2 là tối ưu: nums1[2] * nums2[2] = 3 * 10 = 30, đây là điểm số lớn nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums1[i], nums2[j] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Hàng đợi ưu tiên (Min Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số là tổng của $k$ giá trị $\textit{nums1}$ được chọn nhân với giá trị nhỏ nhất trong các giá trị $\textit{nums2}$ tương ứng. Không thể liệt kê tất cả các tập con gồm $k$ phần tử.
>
> Nếu cố định giá trị nhỏ nhất là $a$, ta chỉ có thể chọn các chỉ số có $\textit{nums2}\ge a$. Duyệt các cặp theo thứ tự giảm dần của $\textit{nums2}$ giúp $a$ hiện tại là giá trị nhỏ nhất, còn tất cả các cặp trước đó vẫn đủ điều kiện. Min-heap giữ $k$ giá trị $\textit{nums1}$ lớn nhất; khi heap đủ phần tử, ta nhân tổng với $a$ rồi loại phần tử nhỏ nhất để nhường chỗ.

<!-- thinking:end -->

Sắp xếp nums2 và nums1 theo thứ tự giảm dần của nums2, sau đó duyệt từ đầu đến cuối, đồng thời duy trì một min heap. Heap lưu các phần tử từ nums1 và số phần tử trong heap không vượt quá $k$. Đồng thời, duy trì một biến $s$ biểu diễn tổng các phần tử trong heap, rồi liên tục cập nhật đáp án trong quá trình duyệt.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng nums1.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums1: List[int], nums2: List[int], k: int) -> int:
        nums = sorted(zip(nums2, nums1), reverse=True)
        q = []
        ans = s = 0
        for a, b in nums:
            s += b
            heappush(q, b)
            if len(q) == k:
                ans = max(ans, s * a)
                s -= heappop(q)
        return ans
```

#### Java

```java
class Solution {
    public long maxScore(int[] nums1, int[] nums2, int k) {
        int n = nums1.length;
        int[][] nums = new int[n][2];
        for (int i = 0; i < n; ++i) {
            nums[i] = new int[] {nums1[i], nums2[i]};
        }
        Arrays.sort(nums, (a, b) -> b[1] - a[1]);
        long ans = 0, s = 0;
        PriorityQueue<Integer> q = new PriorityQueue<>();
        for (int i = 0; i < n; ++i) {
            s += nums[i][0];
            q.offer(nums[i][0]);
            if (q.size() == k) {
                ans = Math.max(ans, s * nums[i][1]);
                s -= q.poll();
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
    long long maxScore(vector<int>& nums1, vector<int>& nums2, int k) {
        int n = nums1.size();
        vector<pair<int, int>> nums(n);
        for (int i = 0; i < n; ++i) {
            nums[i] = {-nums2[i], nums1[i]};
        }
        sort(nums.begin(), nums.end());
        priority_queue<int, vector<int>, greater<int>> q;
        long long ans = 0, s = 0;
        for (auto& [a, b] : nums) {
            s += b;
            q.push(b);
            if (q.size() == k) {
                ans = max(ans, s * -a);
                s -= q.top();
                q.pop();
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxScore(nums1 []int, nums2 []int, k int) int64 {
	type pair struct{ a, b int }
	nums := []pair{}
	for i, a := range nums1 {
		b := nums2[i]
		nums = append(nums, pair{a, b})
	}
	sort.Slice(nums, func(i, j int) bool { return nums[i].b > nums[j].b })
	q := hp{}
	var ans, s int
	for _, e := range nums {
		a, b := e.a, e.b
		s += a
		heap.Push(&q, a)
		if q.Len() == k {
			ans = max(ans, s*b)
			s -= heap.Pop(&q).(int)
		}
	}
	return int64(ans)
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] < h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
