---
comments: true
difficulty: Hard
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [1063. Number of Valid Subarrays 🔒](https://leetcode.com/problems/number-of-valid-subarrays)

[中文文档](/solution/1000-1099/1063.Number%20of%20Valid%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em>số lượng <strong>mảng con</strong> không rỗng mà phần tử ngoài cùng bên trái không lớn hơn các phần tử khác trong mảng con đó</em>.</p>

<p><strong>Mảng con</strong> là một đoạn <strong>liên tiếp</strong> trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,2,5,3]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Có 11 mảng con hợp lệ: [1],[4],[2],[5],[3],[1,4],[2,5],[1,4,2],[2,5,3],[1,4,2,5],[1,4,2,5,3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 mảng con hợp lệ: [3],[2],[1].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có 6 mảng con hợp lệ: [2],[2],[2],[2,2],[2,2],[2,2,2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng con hợp lệ khi và chỉ khi phần tử đầu tiên là giá trị nhỏ nhất. Với mỗi $i$, đầu phải của mảng con có thể kéo dài đến ngay trước giá trị nhỏ hơn nghiêm ngặt tiếp theo. Vì $n\le 5\times 10^4$, cần tìm phần tử nhỏ hơn tiếp theo trong thời gian tuyến tính.
>
> Duyệt từ phải sang trái bằng stack chỉ số giảm dần, loại khỏi đỉnh mọi chỉ số có giá trị không nhỏ hơn $nums[i]$. Đỉnh mới (hoặc $n$ nếu stack rỗng) là ranh giới; số mảng con bắt đầu tại $i$ bằng chỉ số đó trừ $i$.
>
> Lưu ranh giới vào $\textit{right}[i]$ rồi tính tổng sẽ cho đáp án theo đúng định nghĩa.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validSubarrays(self, nums: List[int]) -> int:
        n = len(nums)
        right = [n] * n
        stk = []
        for i in range(n - 1, -1, -1):
            while stk and nums[stk[-1]] >= nums[i]:
                stk.pop()
            if stk:
                right[i] = stk[-1]
            stk.append(i)
        return sum(j - i for i, j in enumerate(right))
```

#### Java

```java
class Solution {
    public int validSubarrays(int[] nums) {
        int n = nums.length;
        int[] right = new int[n];
        Arrays.fill(right, n);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[stk.peek()] >= nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += right[i] - i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int validSubarrays(vector<int>& nums) {
        int n = nums.size();
        vector<int> right(n, n);
        stack<int> stk;
        for (int i = n - 1; ~i; --i) {
            while (stk.size() && nums[stk.top()] >= nums[i]) {
                stk.pop();
            }
            if (stk.size()) {
                right[i] = stk.top();
            }
            stk.push(i);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += right[i] - i;
        }
        return ans;
    }
};
```

#### Go

```go
func validSubarrays(nums []int) (ans int) {
	n := len(nums)
	right := make([]int, n)
	for i := range right {
		right[i] = n
	}
	stk := []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && nums[stk[len(stk)-1]] >= nums[i] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	for i, j := range right {
		ans += j - i
	}
	return
}
```

#### TypeScript

```ts
function validSubarrays(nums: number[]): number {
    const n = nums.length;
    const right: number[] = Array(n).fill(n);
    const stk: number[] = [];
    for (let i = n - 1; ~i; --i) {
        while (stk.length && nums[stk.at(-1)] >= nums[i]) {
            stk.pop();
        }
        if (stk.length) {
            right[i] = stk.at(-1)!;
        }
        stk.push(i);
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans += right[i] - i;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu toàn bộ mảng $\textit{right}$. Khi duyệt ngược, ta có thể xác định ranh giới ngay lập tức.
>
> Dùng cùng stack để cộng trực tiếp $(stk[-1]\text{ hoặc }n)-i$ vào đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validSubarrays(self, nums: List[int]) -> int:
        n = len(nums)
        stk = []
        ans = 0
        for i in range(n - 1, -1, -1):
            while stk and nums[stk[-1]] >= nums[i]:
                stk.pop()
            ans += (stk[-1] if stk else n) - i
            stk.append(i)
        return ans
```

#### Java

```java
class Solution {
    public int validSubarrays(int[] nums) {
        int n = nums.length;
        Deque<Integer> stk = new ArrayDeque<>();
        int ans = 0;
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[stk.peek()] >= nums[i]) {
                stk.pop();
            }
            ans += (stk.isEmpty() ? n : stk.peek()) - i;

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
    int validSubarrays(vector<int>& nums) {
        int n = nums.size();
        stack<int> stk;
        int ans = 0;
        for (int i = n - 1; ~i; --i) {
            while (stk.size() && nums[stk.top()] >= nums[i]) {
                stk.pop();
            }
            ans += (stk.size() ? stk.top() : n) - i;
            stk.push(i);
        }
        return ans;
    }
};
```

#### Go

```go
func validSubarrays(nums []int) (ans int) {
	n := len(nums)
	stk := []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && nums[stk[len(stk)-1]] >= nums[i] {
			stk = stk[:len(stk)-1]
		}
		ans -= i
		if len(stk) > 0 {
			ans += stk[len(stk)-1]
		} else {
			ans += n
		}
		stk = append(stk, i)
	}
	return
}
```

#### TypeScript

```ts
function validSubarrays(nums: number[]): number {
    const n = nums.length;
    const stk: number[] = [];
    let ans = 0;
    for (let i = n - 1; ~i; --i) {
        while (stk.length && nums[stk.at(-1)!] >= nums[i]) {
            stk.pop();
        }
        ans += (stk.at(-1) ?? n) - i;
        stk.push(i);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
