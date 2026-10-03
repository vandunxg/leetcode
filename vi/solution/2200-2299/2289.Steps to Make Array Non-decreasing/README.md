---
comments: true
difficulty: Medium
rating: 2481
source: Weekly Contest 295 Q3
tags:
    - Stack
    - Array
    - Linked List
    - Dynamic Programming
    - Monotonic Stack
    - Simulation
---

<!-- problem:start -->

# [2289. Steps to Make Array Non-decreasing](https://leetcode.com/problems/steps-to-make-array-non-decreasing)

[Tài liệu tiếng Trung](/solution/2200-2299/2289.Steps%20to%20Make%20Array%20Non-decreasing/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code>. Trong một bước, <strong>xóa</strong> tất cả phần tử <code>nums[i]</code> sao cho <code>nums[i - 1] &gt; nums[i]</code> với mọi <code>0 &lt; i &lt; nums.length</code>.</p>

<p>Trả về <em>số bước được thực hiện cho đến khi </em><code>nums</code><em> trở thành một mảng <strong>không giảm</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,3,4,4,7,3,6,11,8,5,11]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các bước được thực hiện như sau:
- Bước 1: [5,<strong><u>3</u></strong>,4,4,7,<u><strong>3</strong></u>,6,11,<u><strong>8</strong></u>,<u><strong>5</strong></u>,11] trở thành [5,4,4,7,6,11,11]
- Bước 2: [5,<u><strong>4</strong></u>,4,7,<u><strong>6</strong></u>,11,11] trở thành [5,4,7,11,11]
- Bước 3: [5,<u><strong>4</strong></u>,7,11,11] trở thành [5,7,11,11]
[5,7,11,11] là một mảng không giảm. Vì vậy, ta trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,5,7,7,13]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> nums đã là một mảng không giảm. Vì vậy, ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt xóa mọi phần tử nhỏ hơn phần tử bên trái; ta cần tìm số lượt. Vì $n \le 10^5$, không thể mô phỏng từng lượt. Thời điểm xóa một chỉ số được quyết định bởi số lượt cần để một hậu tố giảm dần ở bên phải bị một giá trị lớn hơn ở bên trái hấp thụ.
>
> Một stack duyệt từ phải sang trái lưu các chỉ số chưa bị xóa. Mỗi lần pop cập nhật $dp[i] = \max(dp[i]+1, dp[\textit{top}])$. Đáp án là $\max(dp)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalSteps(self, nums: List[int]) -> int:
        stk = []
        ans, n = 0, len(nums)
        dp = [0] * n
        for i in range(n - 1, -1, -1):
            while stk and nums[i] > nums[stk[-1]]:
                dp[i] = max(dp[i] + 1, dp[stk.pop()])
            stk.append(i)
        return max(dp)
```

#### Java

```java
class Solution {
    public int totalSteps(int[] nums) {
        Deque<Integer> stk = new ArrayDeque<>();
        int ans = 0;
        int n = nums.length;
        int[] dp = new int[n];
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[i] > nums[stk.peek()]) {
                dp[i] = Math.max(dp[i] + 1, dp[stk.pop()]);
                ans = Math.max(ans, dp[i]);
            }
            stk.push(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalSteps(vector<int>& nums) {
        stack<int> stk;
        int ans = 0, n = nums.size();
        vector<int> dp(n);
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.empty() && nums[i] > nums[stk.top()]) {
                dp[i] = max(dp[i] + 1, dp[stk.top()]);
                ans = max(ans, dp[i]);
                stk.pop();
            }
            stk.push(i);
        }
        return ans;
    }
};
```

#### Go

```go
func totalSteps(nums []int) int {
	stk := []int{}
	ans, n := 0, len(nums)
	dp := make([]int, n)
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && nums[i] > nums[stk[len(stk)-1]] {
			dp[i] = max(dp[i]+1, dp[stk[len(stk)-1]])
			stk = stk[:len(stk)-1]
			ans = max(ans, dp[i])
		}
		stk = append(stk, i)
	}
	return ans
}
```

#### TypeScript

```ts
function totalSteps(nums: number[]): number {
    let ans = 0;
    let stack = [];
    for (let num of nums) {
        let max = 0;
        while (stack.length && stack[0][0] <= num) {
            max = Math.max(stack[0][1], max);
            stack.shift();
        }
        if (stack.length) max++;
        ans = Math.max(max, ans);
        stack.unshift([num, max]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
