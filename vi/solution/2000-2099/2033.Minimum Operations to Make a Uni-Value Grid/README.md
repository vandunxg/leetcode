---
comments: true
difficulty: Medium
rating: 1671
source: Weekly Contest 262 Q2
tags:
    - Array
    - Math
    - Matrix
    - Sorting
---

<!-- problem:start -->

# [2033. Minimum Operations to Make a Uni-Value Grid](https://leetcode.com/problems/minimum-operations-to-make-a-uni-value-grid)

[中文文档](/solution/2000-2099/2033.Minimum%20Operations%20to%20Make%20a%20Uni-Value%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <code>grid</code> số nguyên hai chiều có kích thước <code>m x n</code> và một số nguyên <code>x</code>. Trong một thao tác, bạn có thể <strong>cộng</strong> <code>x</code> hoặc <strong>trừ</strong> <code>x</code> khỏi bất kỳ phần tử nào trong <code>grid</code>.</p>

<p>Một <strong>uni-value grid</strong> là một grid mà tất cả các phần tử đều bằng nhau.</p>

<p>Hãy trả về <em>số thao tác <strong>nhỏ nhất</strong> để biến grid thành <strong>uni-value</strong></em>. Nếu không thể thực hiện, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2033.Minimum%20Operations%20to%20Make%20a%20Uni-Value%20Grid/images/gridtxt.png" style="width: 164px; height: 165px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[2,4],[6,8]], x = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể đưa mọi phần tử về 4 như sau:
- Cộng x vào 2 một lần.
- Trừ x khỏi 6 một lần.
- Trừ x khỏi 8 hai lần.
Tổng cộng đã thực hiện 4 thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2033.Minimum%20Operations%20to%20Make%20a%20Uni-Value%20Grid/images/gridtxt-1.png" style="width: 164px; height: 165px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,5],[2,3]], x = 1
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Ta có thể đưa mọi phần tử về 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2033.Minimum%20Operations%20to%20Make%20a%20Uni-Value%20Grid/images/gridtxt-2.png" style="width: 164px; height: 165px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2],[3,4]], x = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể đưa mọi phần tử về cùng một giá trị.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= x, grid[i][j] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Vì chỉ được phép $\pm x$, tất cả các ô phải có cùng phần dư modulo $x$. Với tối đa $10^5$ ô, chi phí khi chọn giá trị đích $t$ là $\sum |a_i-t|/x$.
>
> Tổng này đạt giá trị nhỏ nhất tại median. Ta làm phẳng grid, sắp xếp, chọn phần tử ở giữa, rồi cộng các độ lệch tuyệt đối chia cho $x$.

<!-- thinking:end -->

Trước hết, để biến grid thành một grid có cùng một giá trị, phần dư của tất cả các phần tử trong grid khi chia cho $x$ phải giống nhau.

Do đó, trước tiên ta duyệt grid để kiểm tra xem phần dư của tất cả các phần tử khi chia cho $x$ có giống nhau hay không. Nếu không, trả về $-1$. Ngược lại, ta đưa tất cả phần tử vào một mảng, sắp xếp mảng, chọn median, rồi duyệt qua mảng, tính hiệu giữa mỗi phần tử và median, chia cho $x$, và cộng tất cả các hiệu để nhận được đáp án.

Độ phức tạp thời gian là $O((m \times n) \times \log (m \times n))$, và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, grid: List[List[int]], x: int) -> int:
        nums = []
        mod = grid[0][0] % x
        for row in grid:
            for v in row:
                if v % x != mod:
                    return -1
                nums.append(v)
        nums.sort()
        mid = nums[len(nums) >> 1]
        return sum(abs(v - mid) // x for v in nums)
```

#### Java

```java
class Solution {
    public int minOperations(int[][] grid, int x) {
        int m = grid.length, n = grid[0].length;
        int[] nums = new int[m * n];
        int mod = grid[0][0] % x;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] % x != mod) {
                    return -1;
                }
                nums[i * n + j] = grid[i][j];
            }
        }
        Arrays.sort(nums);
        int mid = nums[nums.length >> 1];
        int ans = 0;
        for (int v : nums) {
            ans += Math.abs(v - mid) / x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<vector<int>>& grid, int x) {
        int m = grid.size(), n = grid[0].size();
        int mod = grid[0][0] % x;
        int nums[m * n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] % x != mod) {
                    return -1;
                }
                nums[i * n + j] = grid[i][j];
            }
        }
        sort(nums, nums + m * n);
        int mid = nums[(m * n) >> 1];
        int ans = 0;
        for (int v : nums) {
            ans += abs(v - mid) / x;
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(grid [][]int, x int) int {
	mod := grid[0][0] % x
	nums := []int{}
	for _, row := range grid {
		for _, v := range row {
			if v%x != mod {
				return -1
			}
			nums = append(nums, v)
		}
	}
	sort.Ints(nums)
	mid := nums[len(nums)>>1]
	ans := 0
	for _, v := range nums {
		ans += abs(v-mid) / x
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minOperations(grid: number[][], x: number): number {
    const nums = grid.flat(2);
    const mod = nums[0] % x;

    if (nums.some(num => num % x !== mod)) {
        return -1;
    }

    nums.sort((a, b) => a - b);
    const mid = nums[Math.floor(nums.length / 2)];
    return nums.reduce((ans, num) => ans + Math.abs(num - mid) / x, 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(grid: Vec<Vec<i32>>, x: i32) -> i32 {
        let mut nums: Vec<i32> = grid.into_iter().flatten().collect();
        let mod_val = nums[0] % x;

        if nums.iter().any(|&num| num % x != mod_val) {
            return -1;
        }

        nums.sort_unstable();

        let mid = nums[nums.len() / 2];
        nums.iter().fold(0, |acc, &num| acc + (num - mid).abs() / x)
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @param {number} x
 * @return {number}
 */
var minOperations = function (grid, x) {
    const nums = grid.flat(2);
    const mod = nums[0] % x;

    if (nums.some(num => num % x !== mod)) {
        return -1;
    }

    nums.sort((a, b) => a - b);
    const mid = nums[Math.floor(nums.length / 2)];
    return nums.reduce((ans, num) => ans + Math.abs(num - mid) / x, 0);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
