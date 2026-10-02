---
comments: true
difficulty: Medium
rating: 1802
source: Weekly Contest 217 Q2
tags:
    - Stack
    - Greedy
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [1673. Find the Most Competitive Subsequence](https://leetcode.com/problems/find-the-most-competitive-subsequence)

[中文文档](/solution/1600-1699/1673.Find%20the%20Most%20Competitive%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên dương <code>k</code>, hãy trả về <em>subsequence <strong>competitive</strong> nhất của </em><code>nums</code> <em>có kích thước </em><code>k</code>.</p>

<p>Subsequence của một mảng là dãy thu được bằng cách xóa một số phần tử (có thể không xóa phần tử nào) khỏi mảng.</p>

<p>Subsequence <code>a</code> được gọi là <strong>competitive</strong> hơn subsequence <code>b</code> (cùng độ dài) nếu tại vị trí đầu tiên mà chúng khác nhau, subsequence <code>a</code> có giá trị <strong>nhỏ hơn</strong> giá trị tương ứng trong <code>b</code>. Ví dụ, <code>a</code> = <code>[1,3,4]</code> competitive hơn <code>b</code> = <code>[1,3,5]</code> vì vị trí khác nhau đầu tiên là phần tử cuối, và <code>4</code> nhỏ hơn <code>5</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [3,5,2,6], k = 2
<strong>Output:</strong> [2,6]
<strong>Giải thích:</strong> Trong tất cả các subsequence có thể có: {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]}, [2,6] là subsequence competitive nhất.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,4,3,3,5,4,9,6], k = 4
<strong>Output:</strong> [2,3,3,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Subsequence competitive nhất có độ dài $k$ chính là subsequence nhỏ nhất theo thứ tự từ điển có độ dài đó. Vì $n$ có thể bằng $10^5$, ta dùng monotone stack để xóa phần tử đầu stack lớn hơn khi vẫn còn đủ phần tử để tạo đủ $k$ phần tử.
>
> Đưa giá trị hiện tại vào stack khi stack chưa đủ $k$ phần tử; stack chính là đáp án.

<!-- thinking:end -->

Ta duyệt mảng `nums` từ trái sang phải và duy trì một stack `stk`. Nếu phần tử hiện tại `nums[i]` nhỏ hơn phần tử trên cùng của stack, đồng thời số phần tử trong stack cộng với $n-i$ lớn hơn $k$, ta xóa phần tử trên cùng cho đến khi điều kiện trên không còn đúng. Sau đó, nếu stack còn ít hơn $k$ phần tử thì đưa phần tử hiện tại vào stack.

Sau khi duyệt xong, các phần tử trong stack là đáp án.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(k)$, trong đó $n$ là độ dài mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostCompetitive(self, nums: List[int], k: int) -> List[int]:
        stk = []
        n = len(nums)
        for i, v in enumerate(nums):
            while stk and stk[-1] > v and len(stk) + n - i > k:
                stk.pop()
            if len(stk) < k:
                stk.append(v)
        return stk
```

#### Java

```java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        Deque<Integer> stk = new ArrayDeque<>();
        int n = nums.length;
        for (int i = 0; i < nums.length; ++i) {
            while (!stk.isEmpty() && stk.peek() > nums[i] && stk.size() + n - i > k) {
                stk.pop();
            }
            if (stk.size() < k) {
                stk.push(nums[i]);
            }
        }
        int[] ans = new int[stk.size()];
        for (int i = ans.length - 1; i >= 0; --i) {
            ans[i] = stk.pop();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> mostCompetitive(vector<int>& nums, int k) {
        vector<int> stk;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            while (stk.size() && stk.back() > nums[i] && stk.size() + n - i > k) {
                stk.pop_back();
            }
            if (stk.size() < k) {
                stk.push_back(nums[i]);
            }
        }
        return stk;
    }
};
```

#### Go

```go
func mostCompetitive(nums []int, k int) []int {
	stk := []int{}
	n := len(nums)
	for i, v := range nums {
		for len(stk) > 0 && stk[len(stk)-1] > v && len(stk)+n-i > k {
			stk = stk[:len(stk)-1]
		}
		if len(stk) < k {
			stk = append(stk, v)
		}
	}
	return stk
}
```

#### TypeScript

```ts
function mostCompetitive(nums: number[], k: number): number[] {
    const stk: number[] = [];
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        while (stk.length && stk.at(-1)! > nums[i] && stk.length + n - i > k) {
            stk.pop();
        }
        if (stk.length < k) {
            stk.push(nums[i]);
        }
    }
    return stk;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
