---
comments: true
difficulty: Medium
rating: 1776
source: Weekly Contest 500 Q3
tags:
    - Greedy
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3919. Minimum Cost to Move Between Indices](https://leetcode.com/problems/minimum-cost-to-move-between-indices)

[中文文档](/solution/3900-3999/3919.Minimum%20Cost%20to%20Move%20Between%20Indices/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>, trong đó <code>nums</code> <strong><span data-keyword="strictly-increasing-array">tăng nghiêm ngặt</span></strong>.</p>

<p>Với mỗi chỉ số <code>x</code>, gọi <code>closest(x)</code> là chỉ số <strong>liền kề</strong> <code>y</code> sao cho <code>abs(nums[x] - nums[y])</code> đạt <strong>giá trị nhỏ nhất</strong>. Nếu cả hai chỉ số <strong>liền kề</strong> đều tồn tại và cho cùng một hiệu, hãy chọn chỉ số <strong>nhỏ hơn</strong>.</p>

<p>Từ bất kỳ chỉ số <code>x</code> nào, bạn có thể di chuyển theo hai cách:</p>

<ul>
	<li>Đến bất kỳ chỉ số <code>y</code> nào với chi phí <code>abs(nums[x] - nums[y])</code>, hoặc</li>
	<li>Đến <code>closest(x)</code> với chi phí 1.</li>
</ul>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code>, trong đó mỗi <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>.</p>

<p>Với mỗi truy vấn, hãy tính <strong>tổng chi phí nhỏ nhất</strong> để di chuyển từ chỉ số <code>l<sub>i</sub></code> đến chỉ số <code>r<sub>i</sub></code>.</p>

<p>Trả về một mảng số nguyên <code>ans</code>, trong đó <code>ans[i]</code> là đáp án cho truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p><strong>Hiệu tuyệt đối</strong> giữa hai giá trị <code>x</code> và <code>y</code> được định nghĩa là <code>abs(x - y)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-5,-2,3], queries = [[0,2],[2,0],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6,2,5]</span></p>

<p><strong>Giải thích:</strong>​​​​​​​​​​​​​​​​​​​​</p>

<ul>
	<li>Các chỉ số gần nhất lần lượt là <code>[1, 0, 1]</code>.</li>
	<li>Với <code>[0, 2]</code>, đường đi <code>0 &rarr; 1 &rarr; 2</code> sử dụng một bước di chuyển gần nhất từ chỉ số 0 đến 1 với chi phí 1 và một bước di chuyển từ chỉ số 1 đến 2 với chi phí <code>|-2 - 3| = 5</code>, tổng cộng là <code>1 + 5 = 6</code>.</li>
	<li>Với <code>[2, 0]</code>, đường đi <code>2 &rarr; 1 &rarr; 0</code> sử dụng hai bước di chuyển gần nhất từ chỉ số 2 đến 1 và từ chỉ số 1 đến 0, mỗi bước có chi phí 1, tổng cộng là 2.</li>
	<li>Với <code>[1, 2]</code>, di chuyển trực tiếp từ chỉ số 1 đến chỉ số 2 có chi phí <code>|-2 - 3| = 5</code>, đây là phương án tối ưu.</li>
</ul>

<p>Do đó, <code>ans = [6, 2, 5]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,2,3,9], queries = [[3,0],[1,2],[2,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,1,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các chỉ số gần nhất lần lượt là <code>[1, 2, 1, 2]</code>.</li>
	<li>Với <code>[3, 0]</code>, đường đi <code>3 &rarr; 2 &rarr; 1 &rarr; 0</code> sử dụng các bước di chuyển gần nhất từ chỉ số 3 đến 2 và từ 2 đến 1, mỗi bước có chi phí 1, cùng với một bước di chuyển từ 1 đến 0 có chi phí <code>|2 - 0| = 2</code>, tổng cộng là <code>1 + 1 + 2 = 4</code>.</li>
	<li>Với <code>[1, 2]</code>, bước di chuyển gần nhất từ chỉ số 1 đến 2 có chi phí 1.</li>
	<li>Với <code>[2, 0]</code>, đường đi <code>2 &rarr; 1 &rarr; 0</code> sử dụng một bước di chuyển gần nhất từ chỉ số 2 đến 1 với chi phí 1 và một bước di chuyển từ 1 đến 0 với chi phí <code>|2 - 0| = 2</code>, tổng cộng là <code>1 + 2 = 3</code>.</li>
</ul>

<p>Do đó, <code>ans = [4, 1, 3]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>nums</code> tăng nghiêm ngặt</li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>​​​​​​​</li>
	<li><code>0 &lt;= l<sub>i</sub>, r<sub>i</sub> &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng tăng nghiêm ngặt và có tối đa $10^5$ truy vấn, nên ta không thể mô phỏng từng bước di chuyển trên đoạn $[l,r]$. Chi phí của một bước liền kề được quyết định bởi một bộ ba khoảng cách cục bộ, và quy tắc di chuyển từ trái sang phải không giống với chiều ngược lại.
>
> Hãy tiền xử lý chi phí đi sang phải $c_1$ và đi sang trái $c_2$ trên mỗi cạnh liền kề, sau đó lưu các tổng tiền tố tương ứng $s_1$ và $s_2$. Với truy vấn có $l<r$, ta lấy $s_1[r]-s_1[l]$; ngược lại, ta lấy $s_2[l]-s_2[r]$.
>
> Sau khi tiền xử lý $O(n)$, mỗi truy vấn chỉ mất $O(1)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, nums: list[int], queries: list[list[int]]) -> list[int]:
        n = len(nums)
        s1 = [0] * n
        s2 = [0] * n
        for i in range(1, n):
            c1 = (
                nums[i] - nums[i - 1]
                if i > 1 and nums[i - 1] - nums[i - 2] <= nums[i] - nums[i - 1]
                else 1
            )
            c2 = (
                nums[i] - nums[i - 1]
                if i < n - 1 and nums[i] - nums[i - 1] > nums[i + 1] - nums[i]
                else 1
            )
            s1[i] = s1[i - 1] + c1
            s2[i] = s2[i - 1] + c2
        m = len(queries)
        ans = [0] * m
        for i, (l, r) in enumerate(queries):
            ans[i] = s1[r] - s1[l] if l < r else s2[l] - s2[r]
        return ans
```

#### Java

```java
class Solution {
    public int[] minCost(int[] nums, int[][] queries) {
        int n = nums.length;
        int[] s1 = new int[n];
        int[] s2 = new int[n];
        for (int i = 1; i < n; i++) {
            int c1 = (i > 1 && nums[i - 1] - nums[i - 2] <= nums[i] - nums[i - 1])
                ? nums[i] - nums[i - 1]
                : 1;
            int c2 = (i < n - 1 && nums[i] - nums[i - 1] > nums[i + 1] - nums[i])
                ? nums[i] - nums[i - 1]
                : 1;
            s1[i] = s1[i - 1] + c1;
            s2[i] = s2[i - 1] + c2;
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; i++) {
            int l = queries[i][0];
            int r = queries[i][1];
            ans[i] = (l < r) ? s1[r] - s1[l] : s2[l] - s2[r];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minCost(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        vector<int> s1(n, 0);
        vector<int> s2(n, 0);
        for (int i = 1; i < n; i++) {
            int c1 = (i > 1 && nums[i - 1] - nums[i - 2] <= nums[i] - nums[i - 1]) ? nums[i] - nums[i - 1] : 1;
            int c2 = (i < n - 1 && nums[i] - nums[i - 1] > nums[i + 1] - nums[i]) ? nums[i] - nums[i - 1] : 1;
            s1[i] = s1[i - 1] + c1;
            s2[i] = s2[i - 1] + c2;
        }
        int m = queries.size();
        vector<int> ans(m);
        for (int i = 0; i < m; i++) {
            int l = queries[i][0];
            int r = queries[i][1];
            ans[i] = (l < r) ? s1[r] - s1[l] : s2[l] - s2[r];
        }
        return ans;
    }
};
```

#### Go

```go
func minCost(nums []int, queries [][]int) []int {
	n := len(nums)
	s1 := make([]int, n)
	s2 := make([]int, n)
	for i := 1; i < n; i++ {
		c1 := 1
		if i > 1 && nums[i-1]-nums[i-2] <= nums[i]-nums[i-1] {
			c1 = nums[i] - nums[i-1]
		}
		c2 := 1
		if i < n-1 && nums[i]-nums[i-1] > nums[i+1]-nums[i] {
			c2 = nums[i] - nums[i-1]
		}
		s1[i] = s1[i-1] + c1
		s2[i] = s2[i-1] + c2
	}
	m := len(queries)
	ans := make([]int, m)
	for i := 0; i < m; i++ {
		l := queries[i][0]
		r := queries[i][1]
		if l < r {
			ans[i] = s1[r] - s1[l]
		} else {
			ans[i] = s2[l] - s2[r]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minCost(nums: number[], queries: number[][]): number[] {
    const n = nums.length;
    const s1: number[] = new Array(n).fill(0);
    const s2: number[] = new Array(n).fill(0);
    for (let i = 1; i < n; i++) {
        const c1 =
            i > 1 && nums[i - 1] - nums[i - 2] <= nums[i] - nums[i - 1] ? nums[i] - nums[i - 1] : 1;
        const c2 =
            i < n - 1 && nums[i] - nums[i - 1] > nums[i + 1] - nums[i] ? nums[i] - nums[i - 1] : 1;
        s1[i] = s1[i - 1] + c1;
        s2[i] = s2[i - 1] + c2;
    }
    const m = queries.length;
    const ans: number[] = new Array(m);
    for (let i = 0; i < m; i++) {
        const l = queries[i][0];
        const r = queries[i][1];
        ans[i] = l < r ? s1[r] - s1[l] : s2[l] - s2[r];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
