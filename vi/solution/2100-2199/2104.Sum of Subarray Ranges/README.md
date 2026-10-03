---
comments: true
difficulty: Medium
rating: 1504
source: Weekly Contest 271 Q2
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [2104. Sum of Subarray Ranges](https://leetcode.com/problems/sum-of-subarray-ranges)

[中文文档](/solution/2100-2199/2104.Sum%20of%20Subarray%20Ranges/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. <strong>Độ chênh lệch</strong> của một mảng con trong <code>nums</code> là hiệu giữa phần tử lớn nhất và nhỏ nhất trong mảng con đó.</p>

<p>Hãy trả về <em>tổng <strong>độ chênh lệch của tất cả</strong> các mảng con của </em><code>nums</code><em>.</em></p>

<p>Mảng con là một dãy phần tử liên tiếp, <strong>không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 6 mảng con của nums là:
[1], độ chênh lệch = phần tử lớn nhất - phần tử nhỏ nhất = 1 - 1 = 0
[2], độ chênh lệch = 2 - 2 = 0
[3], độ chênh lệch = 3 - 3 = 0
[1,2], độ chênh lệch = 2 - 1 = 1
[2,3], độ chênh lệch = 3 - 2 = 1
[1,2,3], độ chênh lệch = 3 - 1 = 2
Vì vậy, tổng các độ chênh lệch là 0 + 0 + 0 + 1 + 1 + 2 = 4.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,3]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 6 mảng con của nums là:
[1], độ chênh lệch = phần tử lớn nhất - phần tử nhỏ nhất = 1 - 1 = 0
[3], độ chênh lệch = 3 - 3 = 0
[3], độ chênh lệch = 3 - 3 = 0
[1,3], độ chênh lệch = 3 - 1 = 2
[3,3], độ chênh lệch = 3 - 3 = 0
[1,3,3], độ chênh lệch = 3 - 1 = 2
Vì vậy, tổng các độ chênh lệch là 0 + 0 + 0 + 2 + 0 + 2 = 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,-2,-3,4,1]
<strong>Đầu ra:</strong> 59
<strong>Giải thích:</strong> Tổng độ chênh lệch của tất cả các mảng con của nums là 59.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm được lời giải với độ phức tạp thời gian <code>O(n)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tổng các độ chênh lệch là tổng của $\max-\min$ trên mọi mảng con. Với $n\le 1000$, duyệt các cặp đầu mút đồng thời duy trì giá trị lớn nhất và nhỏ nhất hiện tại có độ phức tạp $O(n^2)$, phù hợp với ràng buộc.
>
> Vòng lặp bên trong không cần khởi động lại: sau khi cố định chỉ số trái $i$, việc mở rộng $j$ chỉ cập nhật $\textit{mi}$ và $\textit{mx}$, rồi cộng hiệu của chúng.
>
> Vì vậy, hai vòng lặp có thể cộng dồn độ chênh lệch của mọi mảng con.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subArrayRanges(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        for i in range(n - 1):
            mi = mx = nums[i]
            for j in range(i + 1, n):
                mi = min(mi, nums[j])
                mx = max(mx, nums[j])
                ans += mx - mi
        return ans
```

#### Java

```java
class Solution {
    public long subArrayRanges(int[] nums) {
        long ans = 0;
        int n = nums.length;
        for (int i = 0; i < n - 1; ++i) {
            int mi = nums[i], mx = nums[i];
            for (int j = i + 1; j < n; ++j) {
                mi = Math.min(mi, nums[j]);
                mx = Math.max(mx, nums[j]);
                ans += (mx - mi);
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
    long long subArrayRanges(vector<int>& nums) {
        long long ans = 0;
        int n = nums.size();
        for (int i = 0; i < n - 1; ++i) {
            int mi = nums[i], mx = nums[i];
            for (int j = i + 1; j < n; ++j) {
                mi = min(mi, nums[j]);
                mx = max(mx, nums[j]);
                ans += (mx - mi);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func subArrayRanges(nums []int) int64 {
	var ans int64
	n := len(nums)
	for i := 0; i < n-1; i++ {
		mi, mx := nums[i], nums[i]
		for j := i + 1; j < n; j++ {
			mi = min(mi, nums[j])
			mx = max(mx, nums[j])
			ans += (int64)(mx - mi)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function subArrayRanges(nums: number[]): number {
    const n = nums.length;
    let res = 0;
    for (let i = 0; i < n - 1; i++) {
        let min = nums[i];
        let max = nums[i];
        for (let j = i + 1; j < n; j++) {
            min = Math.min(min, nums[j]);
            max = Math.max(max, nums[j]);
            res += max - min;
        }
    }
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn sub_array_ranges(nums: Vec<i32>) -> i64 {
        let n = nums.len();
        let mut res: i64 = 0;
        for i in 1..n {
            let mut min = nums[i - 1];
            let mut max = nums[i - 1];
            for j in i..n {
                min = min.min(nums[j]);
                max = max.max(nums[j]);
                res += (max - min) as i64;
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 trở nên nặng nề khi $n$ lớn. Đóng góp của mỗi giá trị khi là giá trị lớn nhất (hoặc nhỏ nhất) trong một mảng con bằng giá trị đó nhân với số mảng con mà nó đạt cực trị, nên tổng độ chênh lệch là "tổng đóng góp của các giá trị lớn nhất trừ tổng đóng góp của các giá trị nhỏ nhất".
>
> Stack đơn điệu tìm được vị trí lớn hơn hoặc bằng gần nhất bên trái và vị trí lớn hơn nghiêm ngặt gần nhất bên phải của mỗi chỉ số, từ đó đếm các mảng con trong thời gian tuyến tính. Đổi dấu mảng rồi lặp lại sẽ cho phần tính giá trị nhỏ nhất.
>
> Vì vậy, ta cài đặt $f$ và trả về $f(\textit{nums})+f([-v])$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subArrayRanges(self, nums: List[int]) -> int:
        def f(nums):
            stk = []
            n = len(nums)
            left = [-1] * n
            right = [n] * n
            for i, v in enumerate(nums):
                while stk and nums[stk[-1]] <= v:
                    stk.pop()
                if stk:
                    left[i] = stk[-1]
                stk.append(i)
            stk = []
            for i in range(n - 1, -1, -1):
                while stk and nums[stk[-1]] < nums[i]:
                    stk.pop()
                if stk:
                    right[i] = stk[-1]
                stk.append(i)
            return sum((i - left[i]) * (right[i] - i) * v for i, v in enumerate(nums))

        mx = f(nums)
        mi = f([-v for v in nums])
        return mx + mi
```

#### Java

```java
class Solution {
    public long subArrayRanges(int[] nums) {
        long mx = f(nums);
        for (int i = 0; i < nums.length; ++i) {
            nums[i] *= -1;
        }
        long mi = f(nums);
        return mx + mi;
    }

    private long f(int[] nums) {
        Deque<Integer> stk = new ArrayDeque<>();
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        for (int i = 0; i < n; ++i) {
            while (!stk.isEmpty() && nums[stk.peek()] <= nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[stk.peek()] < nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        long s = 0;
        for (int i = 0; i < n; ++i) {
            s += (long) (i - left[i]) * (right[i] - i) * nums[i];
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long subArrayRanges(vector<int>& nums) {
        long long mx = f(nums);
        for (int i = 0; i < nums.size(); ++i) nums[i] *= -1;
        long long mi = f(nums);
        return mx + mi;
    }

    long long f(vector<int>& nums) {
        stack<int> stk;
        int n = nums.size();
        vector<int> left(n, -1);
        vector<int> right(n, n);
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && nums[stk.top()] <= nums[i]) stk.pop();
            if (!stk.empty()) left[i] = stk.top();
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.empty() && nums[stk.top()] < nums[i]) stk.pop();
            if (!stk.empty()) right[i] = stk.top();
            stk.push(i);
        }
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += (long long) (i - left[i]) * (right[i] - i) * nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func subArrayRanges(nums []int) int64 {
	f := func(nums []int) int64 {
		stk := []int{}
		n := len(nums)
		left := make([]int, n)
		right := make([]int, n)
		for i := range left {
			left[i] = -1
			right[i] = n
		}
		for i, v := range nums {
			for len(stk) > 0 && nums[stk[len(stk)-1]] <= v {
				stk = stk[:len(stk)-1]
			}
			if len(stk) > 0 {
				left[i] = stk[len(stk)-1]
			}
			stk = append(stk, i)
		}
		stk = []int{}
		for i := n - 1; i >= 0; i-- {
			for len(stk) > 0 && nums[stk[len(stk)-1]] < nums[i] {
				stk = stk[:len(stk)-1]
			}
			if len(stk) > 0 {
				right[i] = stk[len(stk)-1]
			}
			stk = append(stk, i)
		}
		ans := 0
		for i, v := range nums {
			ans += (i - left[i]) * (right[i] - i) * v
		}
		return int64(ans)
	}
	mx := f(nums)
	for i := range nums {
		nums[i] *= -1
	}
	mi := f(nums)
	return mx + mi
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
