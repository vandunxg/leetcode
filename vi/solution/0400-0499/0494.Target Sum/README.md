---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Backtracking
    - Knapsack
    - 0-1 Knapsack
---

<!-- problem:start -->

# [494. Target Sum](https://leetcode.com/problems/target-sum)

[中文文档](/solution/0400-0499/0494.Target%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>target</code>.</p>

<p>Hãy tạo một <strong>biểu thức</strong> từ nums bằng cách thêm ký hiệu <code>&#39;+&#39;</code> hoặc <code>&#39;-&#39;</code> trước mỗi số nguyên trong nums, rồi nối tất cả các số lại.</p>

<ul>
	<li>Ví dụ, nếu <code>nums = [2, 1]</code>, ta có thể thêm <code>&#39;+&#39;</code> trước <code>2</code> và <code>&#39;-&#39;</code> trước <code>1</code>, rồi nối lại để tạo biểu thức <code>&quot;+2-1&quot;</code>.</li>
</ul>

<p>Trả về số <strong>biểu thức</strong> khác nhau có thể tạo ra và có giá trị bằng <code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1,1], target = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Có 5 cách gán ký hiệu để tổng của nums bằng target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1], target = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 20</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>0 &lt;= sum(nums[i]) &lt;= 1000</code></li>
	<li><code>-1000 &lt;= target &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Gán dấu $+$ hoặc $-$ sao cho biểu thức bằng $\textit{target}$. Với $n\le 20$, duyệt $2^n$ trường hợp khá tốn kém; có thể chuyển bài toán thành subset sum.
>
> Nếu tổng các số được gán dấu âm là $x$, thì $(s-x)-x=\textit{target}$, suy ra $x=(s-\textit{target})/2$. Không có lời giải nếu $s<\textit{target}$ hoặc hiệu là số lẻ. $f[i][j]$ là số cách để $i$ số đầu tiên có tổng bằng $j$.
>
> Tập con rỗng cho $f[0][0]=1$. Chọn hoặc bỏ mỗi phần tử chính là cách đếm của bài toán $0$-$1$ knapsack.

<!-- thinking:end -->

Gọi tổng các phần tử trong mảng $\textit{nums}$ là $s$, và tổng các phần tử được gán dấu âm là $x$. Khi đó, tổng các phần tử mang dấu dương là $s - x$. Ta có:

$$
(s - x) - x = \textit{target} \Rightarrow x = \frac{s - \textit{target}}{2}
$$

Vì $x \geq 0$ và $x$ phải là số nguyên, suy ra $s \geq \textit{target}$ và $s - \textit{target}$ phải là số chẵn. Nếu không thỏa hai điều kiện này, ta trả về $0$ ngay.

Tiếp theo, ta chuyển bài toán thành việc chọn một số phần tử từ mảng $\textit{nums}$ sao cho tổng bằng $\frac{s - \textit{target}}{2}$. Cần đếm số cách chọn như vậy.

Ta có thể dùng quy hoạch động. Định nghĩa $f[i][j]$ là số cách chọn một số phần tử trong $i$ phần tử đầu của mảng $\textit{nums}$ sao cho tổng bằng $j$.

Với $\textit{nums}[i - 1]$, ta có hai lựa chọn: lấy hoặc không lấy. Nếu không lấy $\textit{nums}[i - 1]$, thì $f[i][j] = f[i - 1][j]$; nếu lấy, thì $f[i][j] = f[i - 1][j - \textit{nums}[i - 1]]$. Do đó, công thức chuyển trạng thái là:

$$
f[i][j] = f[i - 1][j] + f[i - 1][j - \textit{nums}[i - 1]]
$$

Phép chuyển này chỉ áp dụng khi $j \geq \textit{nums}[i - 1]$.

Đáp án cuối cùng là $f[m][n]$, trong đó $m$ là độ dài mảng $\textit{nums}$ và $n = \frac{s - \textit{target}}{2}$. 

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        s = sum(nums)
        if s < target or (s - target) % 2:
            return 0
        m, n = len(nums), (s - target) // 2
        f = [[0] * (n + 1) for _ in range(m + 1)]
        f[0][0] = 1
        for i, x in enumerate(nums, 1):
            for j in range(n + 1):
                f[i][j] = f[i - 1][j]
                if j >= x:
                    f[i][j] += f[i - 1][j - x]
        return f[m][n]
```

#### Java

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int s = Arrays.stream(nums).sum();
        if (s < target || (s - target) % 2 != 0) {
            return 0;
        }
        int m = nums.length;
        int n = (s - target) / 2;
        int[][] f = new int[m + 1][n + 1];
        f[0][0] = 1;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= nums[i - 1]) {
                    f[i][j] += f[i - 1][j - nums[i - 1]];
                }
            }
        }
        return f[m][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int s = accumulate(nums.begin(), nums.end(), 0);
        if (s < target || (s - target) % 2) {
            return 0;
        }
        int m = nums.size();
        int n = (s - target) / 2;
        int f[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= nums[i - 1]) {
                    f[i][j] += f[i - 1][j - nums[i - 1]];
                }
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func findTargetSumWays(nums []int, target int) int {
	s := 0
	for _, x := range nums {
		s += x
	}
	if s < target || (s-target)%2 != 0 {
		return 0
	}
	m, n := len(nums), (s-target)/2
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	f[0][0] = 1
	for i := 1; i <= m; i++ {
		for j := 0; j <= n; j++ {
			f[i][j] = f[i-1][j]
			if j >= nums[i-1] {
				f[i][j] += f[i-1][j-nums[i-1]]
			}
		}
	}
	return f[m][n]
}
```

#### TypeScript

```ts
function findTargetSumWays(nums: number[], target: number): number {
    const s = nums.reduce((a, b) => a + b, 0);
    if (s < target || (s - target) % 2) {
        return 0;
    }
    const [m, n] = [nums.length, ((s - target) / 2) | 0];
    const f: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= m; i++) {
        for (let j = 0; j <= n; j++) {
            f[i][j] = f[i - 1][j];
            if (j >= nums[i - 1]) {
                f[i][j] += f[i - 1][j - nums[i - 1]];
            }
        }
    }
    return f[m][n];
}
```

#### Rust

```rust
impl Solution {
    pub fn find_target_sum_ways(nums: Vec<i32>, target: i32) -> i32 {
        let s: i32 = nums.iter().sum();
        if s < target || (s - target) % 2 != 0 {
            return 0;
        }
        let m = nums.len();
        let n = ((s - target) / 2) as usize;
        let mut f = vec![vec![0; n + 1]; m + 1];
        f[0][0] = 1;
        for i in 1..=m {
            for j in 0..=n {
                f[i][j] = f[i - 1][j];
                if j as i32 >= nums[i - 1] {
                    f[i][j] += f[i - 1][j - nums[i - 1] as usize];
                }
            }
        }
        f[m][n]
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var findTargetSumWays = function (nums, target) {
    const s = nums.reduce((a, b) => a + b, 0);
    if (s < target || (s - target) % 2) {
        return 0;
    }
    const [m, n] = [nums.length, ((s - target) / 2) | 0];
    const f = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= m; i++) {
        for (let j = 0; j <= n; j++) {
            f[i][j] = f[i - 1][j];
            if (j >= nums[i - 1]) {
                f[i][j] += f[i - 1][j - nums[i - 1]];
            }
        }
    }
    return f[m][n];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (tối ưu bộ nhớ)

<!-- thinking:start -->

> **Tư duy**
>
> Hàng $i$ chỉ phụ thuộc vào hàng $i-1$. Cập nhật $j$ theo chiều giảm trong một mảng để không dùng lại cùng phần tử; bộ nhớ cần dùng tỷ lệ với sức chứa.

<!-- thinking:end -->

Ta thấy trong công thức chuyển trạng thái của lời giải 1, giá trị $f[i][j]$ chỉ phụ thuộc vào $f[i - 1][j]$ và $f[i - 1][j - \textit{nums}[i - 1]]$. Vì vậy, có thể bỏ chiều thứ nhất và chỉ dùng mảng một chiều.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        s = sum(nums)
        if s < target or (s - target) % 2:
            return 0
        n = (s - target) // 2
        f = [0] * (n + 1)
        f[0] = 1
        for x in nums:
            for j in range(n, x - 1, -1):
                f[j] += f[j - x]
        return f[n]
```

#### Java

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int s = Arrays.stream(nums).sum();
        if (s < target || (s - target) % 2 != 0) {
            return 0;
        }
        int n = (s - target) / 2;
        int[] f = new int[n + 1];
        f[0] = 1;
        for (int num : nums) {
            for (int j = n; j >= num; --j) {
                f[j] += f[j - num];
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int s = accumulate(nums.begin(), nums.end(), 0);
        if (s < target || (s - target) % 2) {
            return 0;
        }
        int n = (s - target) / 2;
        int f[n + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (int x : nums) {
            for (int j = n; j >= x; --j) {
                f[j] += f[j - x];
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func findTargetSumWays(nums []int, target int) int {
	s := 0
	for _, x := range nums {
		s += x
	}
	if s < target || (s-target)%2 != 0 {
		return 0
	}
	n := (s - target) / 2
	f := make([]int, n+1)
	f[0] = 1
	for _, x := range nums {
		for j := n; j >= x; j-- {
			f[j] += f[j-x]
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function findTargetSumWays(nums: number[], target: number): number {
    const s = nums.reduce((a, b) => a + b, 0);
    if (s < target || (s - target) % 2) {
        return 0;
    }
    const n = ((s - target) / 2) | 0;
    const f = Array(n + 1).fill(0);
    f[0] = 1;
    for (const x of nums) {
        for (let j = n; j >= x; j--) {
            f[j] += f[j - x];
        }
    }
    return f[n];
}
```

#### Rust

```rust
impl Solution {
    pub fn find_target_sum_ways(nums: Vec<i32>, target: i32) -> i32 {
        let s: i32 = nums.iter().sum();
        if s < target || (s - target) % 2 != 0 {
            return 0;
        }
        let n = ((s - target) / 2) as usize;
        let mut f = vec![0; n + 1];
        f[0] = 1;
        for x in nums {
            for j in (x as usize..=n).rev() {
                f[j] += f[j - x as usize];
            }
        }
        f[n]
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var findTargetSumWays = function (nums, target) {
    const s = nums.reduce((a, b) => a + b, 0);
    if (s < target || (s - target) % 2) {
        return 0;
    }
    const n = (s - target) / 2;
    const f = Array(n + 1).fill(0);
    f[0] = 1;
    for (const x of nums) {
        for (let j = n; j >= x; j--) {
            f[j] += f[j - x];
        }
    }
    return f[n];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
