---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Backtracking
---

<!-- problem:start -->

# [491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences)

[中文文档](/solution/0400-0499/0491.Non-decreasing%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em>tất cả các dãy con không giảm khác nhau có ít nhất hai phần tử</em> của mảng. Bạn có thể trả lời theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,6,7,7]
<strong>Đầu ra:</strong> [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,4,3,2,1]
<strong>Đầu ra:</strong> [[4,4]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 15</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm mọi dãy con không giảm có độ dài ít nhất $2$ và không tạo ra các dãy trùng nhau. Mảng chưa được sắp xếp, nên ta không thể sắp xếp rồi chọn phần tử.
>
> Dùng DFS tại chỉ số $u$: chọn $nums[u]$ nếu nó $\ge last$; chỉ bỏ qua nó khi $nums[u]\ne last$. Điều kiện thứ hai loại trường hợp trùng lặp do “bỏ qua một giá trị rồi chọn lại chính giá trị đó ở vị trí sau”.
>
> $last$ bắt đầu bằng một giá trị sentinel rất nhỏ để phần tử đầu tiên luôn đủ điều kiện được chọn. Chỉ giữ lại các dãy có độ dài lớn hơn $1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        def dfs(u, last, t):
            if u == len(nums):
                if len(t) > 1:
                    ans.append(t[:])
                return
            if nums[u] >= last:
                t.append(nums[u])
                dfs(u + 1, nums[u], t)
                t.pop()
            if nums[u] != last:
                dfs(u + 1, last, t)

        ans = []
        dfs(0, -1000, [])
        return ans
```

#### Java

```java
class Solution {
    private int[] nums;
    private List<List<Integer>> ans;

    public List<List<Integer>> findSubsequences(int[] nums) {
        this.nums = nums;
        ans = new ArrayList<>();
        dfs(0, -1000, new ArrayList<>());
        return ans;
    }

    private void dfs(int u, int last, List<Integer> t) {
        if (u == nums.length) {
            if (t.size() > 1) {
                ans.add(new ArrayList<>(t));
            }
            return;
        }
        if (nums[u] >= last) {
            t.add(nums[u]);
            dfs(u + 1, nums[u], t);
            t.remove(t.size() - 1);
        }
        if (nums[u] != last) {
            dfs(u + 1, last, t);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> t;
        dfs(0, -1000, t, nums, ans);
        return ans;
    }

    void dfs(int u, int last, vector<int>& t, vector<int>& nums, vector<vector<int>>& ans) {
        if (u == nums.size()) {
            if (t.size() > 1) ans.push_back(t);
            return;
        }
        if (nums[u] >= last) {
            t.push_back(nums[u]);
            dfs(u + 1, nums[u], t, nums, ans);
            t.pop_back();
        }
        if (nums[u] != last) dfs(u + 1, last, t, nums, ans);
    }
};
```

#### Go

```go
func findSubsequences(nums []int) [][]int {
	var ans [][]int
	var dfs func(u, last int, t []int)
	dfs = func(u, last int, t []int) {
		if u == len(nums) {
			if len(t) > 1 {
				ans = append(ans, slices.Clone(t))
			}
			return
		}
		if nums[u] >= last {
			t = append(t, nums[u])
			dfs(u+1, nums[u], t)
			t = t[:len(t)-1]
		}
		if nums[u] != last {
			dfs(u+1, last, t)
		}
	}
	var t []int
	dfs(0, -1000, t)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
