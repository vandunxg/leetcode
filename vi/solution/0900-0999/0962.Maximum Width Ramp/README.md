---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Two Pointers
    - Monotonic Stack
---

<!-- problem:start -->

# [962. Maximum Width Ramp](https://leetcode.com/problems/maximum-width-ramp)

[中文文档](/solution/0900-0999/0962.Maximum%20Width%20Ramp/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Ramp</strong> trong mảng số nguyên <code>nums</code> là một cặp <code>(i, j)</code> thỏa mãn <code>i &lt; j</code> và <code>nums[i] &lt;= nums[j]</code>. <strong>Độ rộng</strong> của ramp đó là <code>j - i</code>.</p>

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em>độ rộng lớn nhất của một <strong>ramp</strong> trong </em><code>nums</code>. Nếu <code>nums</code> không có <strong>ramp</strong>, hãy trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [6,0,8,2,1,5]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Ramp có độ rộng lớn nhất là (i, j) = (1, 5): nums[1] = 0 và nums[5] = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [9,8,1,0,1,9,4,0,4,1]
<strong>Output:</strong> 7
<strong>Giải thích:</strong> Ramp có độ rộng lớn nhất là (i, j) = (2, 9): nums[2] = 1 và nums[9] = 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 5 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm cặp $i<j$ có $nums[i]\le nums[j]$ và độ rộng lớn nhất. Duyệt mọi cặp mất thời gian bậc hai. Các vị trí đầu có thể hữu ích tạo thành một dãy giá trị giảm nghiêm ngặt tính từ đầu mảng; nếu có một giá trị lớn hơn ở phía sau thì nó không thể là vị trí đầu tốt hơn. Ta lưu các ứng viên này trong monotonic stack, sau đó duyệt $j$ từ phải sang trái và pop mọi phần tử trên đỉnh stack tạo thành ramp, đồng thời cập nhật độ rộng lớn nhất.

<!-- thinking:end -->

Từ đề bài, ta nhận thấy dãy con gồm các giá trị $\textit{nums}[i]$ có thể làm vị trí đầu phải giảm đơn điệu. Vì sao? Hãy chứng minh bằng phản chứng.

Giả sử tồn tại $i_1<i_2$ và $\textit{nums}[i_1]\leq\textit{nums}[i_2]$. Khi đó, $\textit{nums}[i_2]$ không thể là một giá trị ứng viên, vì $\textit{nums}[i_1]$ nằm ở bên trái hơn và là lựa chọn tốt hơn. Do đó, dãy con gồm các giá trị $\textit{nums}[i]$ phải giảm đơn điệu, và chỉ số $i$ phải bắt đầu từ 0.

Ta dùng stack giảm đơn điệu $\textit{stk}$ (từ đáy lên đỉnh) để lưu các chỉ số $i$ ứng viên tương ứng với $\textit{nums}[i]$. Sau đó, duyệt $j$ bắt đầu từ biên phải. Nếu gặp $\textit{nums}[\textit{stk.top()}]\leq\textit{nums}[j]$, nghĩa là ta tìm được một ramp. Ta liên tục pop các phần tử trên đỉnh stack và cập nhật $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxWidthRamp(self, nums: List[int]) -> int:
        stk = []
        for i, v in enumerate(nums):
            if not stk or nums[stk[-1]] > v:
                stk.append(i)
        ans = 0
        for i in range(len(nums) - 1, -1, -1):
            while stk and nums[stk[-1]] <= nums[i]:
                ans = max(ans, i - stk.pop())
            if not stk:
                break
        return ans
```

#### Java

```java
class Solution {
    public int maxWidthRamp(int[] nums) {
        int n = nums.length;
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (stk.isEmpty() || nums[stk.peek()] > nums[i]) {
                stk.push(i);
            }
        }
        int ans = 0;
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[stk.peek()] <= nums[i]) {
                ans = Math.max(ans, i - stk.pop());
            }
            if (stk.isEmpty()) {
                break;
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
    int maxWidthRamp(vector<int>& nums) {
        int n = nums.size();
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            if (stk.empty() || nums[stk.top()] > nums[i]) stk.push(i);
        }
        int ans = 0;
        for (int i = n - 1; i; --i) {
            while (!stk.empty() && nums[stk.top()] <= nums[i]) {
                ans = max(ans, i - stk.top());
                stk.pop();
            }
            if (stk.empty()) break;
        }
        return ans;
    }
};
```

#### Go

```go
func maxWidthRamp(nums []int) int {
	n := len(nums)
	stk := []int{}
	for i, v := range nums {
		if len(stk) == 0 || nums[stk[len(stk)-1]] > v {
			stk = append(stk, i)
		}
	}
	ans := 0
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && nums[stk[len(stk)-1]] <= nums[i] {
			ans = max(ans, i-stk[len(stk)-1])
			stk = stk[:len(stk)-1]
		}
		if len(stk) == 0 {
			break
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxWidthRamp(nums: number[]): number {
    let [ans, n] = [0, nums.length];
    const stk: number[] = [];

    for (let i = 0; i < n - 1; i++) {
        if (stk.length === 0 || nums[stk.at(-1)!] > nums[i]) {
            stk.push(i);
        }
    }

    for (let i = n - 1; i >= 0; i--) {
        while (stk.length && nums[stk.at(-1)!] <= nums[i]) {
            ans = Math.max(ans, i - stk.pop()!);
        }
        if (stk.length === 0) break;
    }

    return ans;
}
```

#### JavaScript

```js
function maxWidthRamp(nums) {
    let [ans, n] = [0, nums.length];
    const stk = [];

    for (let i = 0; i < n - 1; i++) {
        if (stk.length === 0 || nums[stk.at(-1)] > nums[i]) {
            stk.push(i);
        }
    }

    for (let i = n - 1; i >= 0; i--) {
        while (stk.length && nums[stk.at(-1)] <= nums[i]) {
            ans = Math.max(ans, i - stk.pop());
        }
        if (stk.length === 0) break;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
