---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [2832. Maximal Range That Each Element Is Maximum in It 🔒](https://leetcode.com/problems/maximal-range-that-each-element-is-maximum-in-it)

[中文文档](/solution/2800-2899/2832.Maximal%20Range%20That%20Each%20Element%20Is%20Maximum%20in%20It/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <b>phân biệt </b><code>nums</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Ta định nghĩa một mảng <strong>được đánh chỉ số từ 0 </strong><code>ans</code> có cùng độ dài với <code>nums</code> như sau:</p>

<ul>
	<li><code>ans[i]</code> là độ dài <strong>lớn nhất</strong> của một mảng con <code>nums[l..r]</code>, sao cho phần tử lớn nhất trong mảng con đó bằng <code>nums[i]</code>.</li>
</ul>

<p>Trả về<em> mảng </em><code>ans</code>.</p>

<p><strong>Lưu ý</strong> rằng <strong>mảng con</strong> là một phần liên tiếp của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,4,3,6]
<strong>Đầu ra:</strong> [1,4,2,1,5]
<strong>Giải thích:</strong> Với nums[0], mảng con dài nhất mà 1 là giá trị lớn nhất là nums[0..0], do đó ans[0] = 1.
Với nums[1], mảng con dài nhất mà 5 là giá trị lớn nhất là nums[0..3], do đó ans[1] = 4.
Với nums[2], mảng con dài nhất mà 4 là giá trị lớn nhất là nums[2..3], do đó ans[2] = 2.
Với nums[3], mảng con dài nhất mà 3 là giá trị lớn nhất là nums[3..3], do đó ans[3] = 1.
Với nums[4], mảng con dài nhất mà 6 là giá trị lớn nhất là nums[0..4], do đó ans[4] = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> [1,2,3,4,5]
<strong>Giải thích:</strong> Với nums[i], mảng con dài nhất mà nó là giá trị lớn nhất là nums[0..i], do đó ans[i] = i + 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả phần tử trong <code>nums</code> đều phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng dài nhất mà $nums[i]$ là phần tử lớn nhất được giới hạn bởi các phần tử lớn hơn nghiêm ngặt gần nhất ở hai phía. Hai lượt duyệt bằng stack đơn điệu sẽ tìm ra các cận đó; độ dài là $right[i]-left[i]-1$.

<!-- thinking:end -->

Bài toán này là một dạng điển hình của stack đơn điệu. Ta chỉ cần sử dụng stack đơn điệu để tìm vị trí của phần tử đầu tiên lớn hơn $nums[i]$ ở bên trái và bên phải, lần lượt ký hiệu là $left[i]$ và $right[i]$. Khi đó, độ dài đoạn mà $nums[i]$ là giá trị lớn nhất là $right[i] - left[i] - 1$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumLengthOfRanges(self, nums: List[int]) -> List[int]:
        n = len(nums)
        left = [-1] * n
        right = [n] * n
        stk = []
        for i, x in enumerate(nums):
            while stk and nums[stk[-1]] <= x:
                stk.pop()
            if stk:
                left[i] = stk[-1]
            stk.append(i)
        stk = []
        for i in range(n - 1, -1, -1):
            while stk and nums[stk[-1]] <= nums[i]:
                stk.pop()
            if stk:
                right[i] = stk[-1]
            stk.append(i)
        return [r - l - 1 for l, r in zip(left, right)]
```

#### Java

```java
class Solution {
    public int[] maximumLengthOfRanges(int[] nums) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        Deque<Integer> stk = new ArrayDeque<>();
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
            while (!stk.isEmpty() && nums[stk.peek()] <= nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = right[i] - left[i] - 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maximumLengthOfRanges(vector<int>& nums) {
        int n = nums.size();
        vector<int> left(n, -1);
        vector<int> right(n, n);
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && nums[stk.top()] <= nums[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                left[i] = stk.top();
            }
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; ~i; --i) {
            while (!stk.empty() && nums[stk.top()] <= nums[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                right[i] = stk.top();
            }
            stk.push(i);
        }
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            ans[i] = right[i] - left[i] - 1;
        }
        return ans;
    }
};
```

#### Go

```go
func maximumLengthOfRanges(nums []int) []int {
	n := len(nums)
	left := make([]int, n)
	right := make([]int, n)
	for i := range left {
		left[i] = -1
		right[i] = n
	}
	stk := []int{}
	for i, x := range nums {
		for len(stk) > 0 && nums[stk[len(stk)-1]] <= x {
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
		for len(stk) > 0 && nums[stk[len(stk)-1]] <= x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	ans := make([]int, n)
	for i := range ans {
		ans[i] = right[i] - left[i] - 1
	}
	return ans
}
```

#### TypeScript

```ts
function maximumLengthOfRanges(nums: number[]): number[] {
    const n = nums.length;
    const left: number[] = Array(n).fill(-1);
    const right: number[] = Array(n).fill(n);
    const stk: number[] = [];
    for (let i = 0; i < n; ++i) {
        while (stk.length && nums[stk.at(-1)] <= nums[i]) {
            stk.pop();
        }
        if (stk.length) {
            left[i] = stk.at(-1);
        }
        stk.push(i);
    }
    stk.length = 0;
    for (let i = n - 1; i >= 0; --i) {
        while (stk.length && nums[stk.at(-1)] <= nums[i]) {
            stk.pop();
        }
        if (stk.length) {
            right[i] = stk.at(-1);
        }
        stk.push(i);
    }
    return left.map((l, i) => right[i] - l - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
