---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Cartesian Tree
    - Monotonic Stack
---

<!-- problem:start -->

# [1950. Maximum of Minimum Values in All Subarrays 🔒](https://leetcode.com/problems/maximum-of-minimum-values-in-all-subarrays)

[中文文档](/solution/1900-1999/1950.Maximum%20of%20Minimum%20Values%20in%20All%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có kích thước <code>n</code>. Bạn cần thực hiện <code>n</code> truy vấn cho mỗi số nguyên <code>i</code> trong khoảng <code>0 &lt;= i &lt; n</code>.</p>

<p>Để xử lý truy vấn thứ <code>i<sup>th</sup></code>:</p>

<ol>
	<li>Tìm <strong>giá trị nhỏ nhất</strong> trong mỗi mảng con có kích thước <code>i + 1</code> của mảng <code>nums</code>.</li>
	<li>Tìm <strong>giá trị lớn nhất</strong> trong các giá trị nhỏ nhất đó. Giá trị lớn nhất này là <strong>đáp án</strong> của truy vấn.</li>
</ol>

<p>Trả về <em>một <strong>mảng số nguyên được đánh chỉ số từ 0</strong></em> <code>ans</code> <em>có kích thước </em><code>n</code> <em>sao cho </em><code>ans[i]</code> <em>là đáp án của truy vấn thứ </em><code>i<sup>th</sup></code> <em>truy vấn</em>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2,4]
<strong>Đầu ra:</strong> [4,2,1,0]
<strong>Giải thích:</strong>
i=0:
- Các mảng con có kích thước 1 là [0], [1], [2], [4]. Các giá trị nhỏ nhất là 0, 1, 2, 4.
- Giá trị lớn nhất trong các giá trị nhỏ nhất là 4.
i=1:
- Các mảng con có kích thước 2 là [0,1], [1,2], [2,4]. Các giá trị nhỏ nhất là 0, 1, 2.
- Giá trị lớn nhất trong các giá trị nhỏ nhất là 2.
i=2:
- Các mảng con có kích thước 3 là [0,1,2], [1,2,4]. Các giá trị nhỏ nhất là 0, 1.
- Giá trị lớn nhất trong các giá trị nhỏ nhất là 1.
i=3:
- Chỉ có một mảng con có kích thước 4 là [0,1,2,4]. Giá trị nhỏ nhất là 0.
- Chỉ có một giá trị nên giá trị lớn nhất là 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,20,50,10]
<strong>Đầu ra:</strong> [50,20,10,10]
<strong>Giải thích:</strong>
i=0:
- Các mảng con có kích thước 1 là [10], [20], [50], [10]. Các giá trị nhỏ nhất là 10, 20, 50, 10.
- Giá trị lớn nhất trong các giá trị nhỏ nhất là 50.
i=1:
- Các mảng con có kích thước 2 là [10,20], [20,50], [50,10]. Các giá trị nhỏ nhất là 10, 20, 10.
- Giá trị lớn nhất trong các giá trị nhỏ nhất là 20.
i=2:
- Các mảng con có kích thước 3 là [10,20,50], [20,50,10]. Các giá trị nhỏ nhất là 10, 10.
- Giá trị lớn nhất trong các giá trị nhỏ nhất là 10.
i=3:
- Chỉ có một mảng con có kích thước 4 là [10,20,50,10]. Giá trị nhỏ nhất là 10.
- Chỉ có một giá trị nên giá trị lớn nhất là 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi độ dài $k$, ta cần giá trị nhỏ nhất lớn nhất trong các cửa sổ. Dùng một deque cho mỗi $k$ sẽ có tổng độ phức tạp $O(n^2)$.
>
> Khoảng dài nhất mà $nums[i]$ là giá trị nhỏ nhất được giới hạn bởi các giá trị gần nhất nhỏ hơn nghiêm ngặt ở hai phía; độ dài đó là $m$, cho phép $nums[i]$ tham gia xét mọi đáp án có độ dài $\le m$.
>
> Stack đơn điệu giúp tìm các biên đó. Ta ghi vào $ans[m-1]$, sau đó duyệt từ phải sang trái để các đáp án dài hơn cũng điền cho những độ dài ngắn hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaximums(self, nums: List[int]) -> List[int]:
        n = len(nums)
        left = [-1] * n
        right = [n] * n
        stk = []
        for i, x in enumerate(nums):
            while stk and nums[stk[-1]] >= x:
                stk.pop()
            if stk:
                left[i] = stk[-1]
            stk.append(i)
        stk = []
        for i in range(n - 1, -1, -1):
            while stk and nums[stk[-1]] >= nums[i]:
                stk.pop()
            if stk:
                right[i] = stk[-1]
            stk.append(i)
        ans = [0] * n
        for i in range(n):
            m = right[i] - left[i] - 1
            ans[m - 1] = max(ans[m - 1], nums[i])
        for i in range(n - 2, -1, -1):
            ans[i] = max(ans[i], ans[i + 1])
        return ans
```

#### Java

```java
class Solution {
    public int[] findMaximums(int[] nums) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            while (!stk.isEmpty() && nums[stk.peek()] >= nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[stk.peek()] >= nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int m = right[i] - left[i] - 1;
            ans[m - 1] = Math.max(ans[m - 1], nums[i]);
        }
        for (int i = n - 2; i >= 0; --i) {
            ans[i] = Math.max(ans[i], ans[i + 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findMaximums(vector<int>& nums) {
        int n = nums.size();
        vector<int> left(n, -1);
        vector<int> right(n, n);
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && nums[stk.top()] >= nums[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                left[i] = stk.top();
            }
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.empty() && nums[stk.top()] >= nums[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                right[i] = stk.top();
            }
            stk.push(i);
        }
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            int m = right[i] - left[i] - 1;
            ans[m - 1] = max(ans[m - 1], nums[i]);
        }
        for (int i = n - 2; i >= 0; --i) {
            ans[i] = max(ans[i], ans[i + 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func findMaximums(nums []int) []int {
	n := len(nums)
	left := make([]int, n)
	right := make([]int, n)
	for i := range left {
		left[i], right[i] = -1, n
	}
	stk := []int{}
	for i, x := range nums {
		for len(stk) > 0 && nums[stk[len(stk)-1]] >= x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			left[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	stk = []int{}
	for i := n - 1; i >= 0; i-- {
		x := nums[i]
		for len(stk) > 0 && nums[stk[len(stk)-1]] >= x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	ans := make([]int, n)
	for i := range ans {
		m := right[i] - left[i] - 1
		ans[m-1] = max(ans[m-1], nums[i])
	}
	for i := n - 2; i >= 0; i-- {
		ans[i] = max(ans[i], ans[i+1])
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
