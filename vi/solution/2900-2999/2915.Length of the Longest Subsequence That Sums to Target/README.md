---
comments: true
difficulty: Medium
rating: 1658
source: Biweekly Contest 116 Q3
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - 0-1 Knapsack
---

<!-- problem:start -->

# [2915. Length of the Longest Subsequence That Sums to Target](https://leetcode.com/problems/length-of-the-longest-subsequence-that-sums-to-target)

[中文文档](/solution/2900-2999/2915.Length%20of%20the%20Longest%20Subsequence%20That%20Sums%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên <code>target</code>.</p>

<p>Trả về <em><strong>độ dài dãy con dài nhất</strong> của</em> <code>nums</code> <em>có tổng bằng</em> <code>target</code>. <em>Nếu không tồn tại dãy con như vậy, trả về</em> <code>-1</code>.</p>

<p><strong>Dãy con</strong> là một mảng có thể được tạo ra từ một mảng khác bằng cách xóa một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5], target = 9
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 dãy con có tổng bằng 9: [4,5], [1,3,5] và [2,3,4]. Dãy con dài nhất là [1,3,5] và [2,3,4]. Vì vậy, đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,1,3,2,1,5], target = 7
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 5 dãy con có tổng bằng 7: [4,3], [4,1,2], [4,2,1], [1,1,5] và [1,3,2,1]. Dãy con dài nhất là [1,3,2,1]. Vì vậy, đáp án là 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,5,4,5], target = 3
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng nums không có dãy con nào có tổng bằng 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= target &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con dài nhất có tổng chính xác bằng $target$ là một bài toán knapsack 0-1 ($n,target \le 1000$). Gọi $f[i][j]$ là độ dài tốt nhất khi dùng $i$ số đầu tiên để tạo tổng $j$, trong đó các trạng thái không thể đạt được có giá trị $-\infty$.
>
> Công thức chuyển trạng thái chọn phương án tốt hơn giữa bỏ qua $x$ và chọn phần tử đó. Nếu $f[n][target]$ không dương, thì không tồn tại lời giải.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là độ dài dãy con dài nhất chọn một số phần tử từ $i$ phần tử đầu tiên sao cho tổng của các phần tử đó chính xác bằng $j$. Ban đầu, $f[0][0]=0$, còn tất cả vị trí khác có giá trị $-\infty$.

Với $f[i][j]$, ta xét số thứ $i$ là $x$. Nếu không chọn $x$, thì $f[i][j]=f[i-1][j]$. Nếu chọn $x$, thì $f[i][j]=f[i-1][j-x]+1$, với $j\ge x$. Do đó, ta có công thức chuyển trạng thái:

$$
f[i][j]=\max\{f[i-1][j],f[i-1][j-x]+1\}
$$

Đáp án cuối cùng là $f[n][target]$. Nếu $f[n][target]\le0$, không tồn tại dãy con có tổng bằng $target$, nên trả về $-1$.

Độ phức tạp thời gian là $O(n\times target)$, còn độ phức tạp không gian là $O(n\times target)$. Ở đây, $n$ là độ dài của mảng, còn $target$ là giá trị mục tiêu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lengthOfLongestSubsequence(self, nums: List[int], target: int) -> int:
        n = len(nums)
        f = [[-inf] * (target + 1) for _ in range(n + 1)]
        f[0][0] = 0
        for i, x in enumerate(nums, 1):
            for j in range(target + 1):
                f[i][j] = f[i - 1][j]
                if j >= x:
                    f[i][j] = max(f[i][j], f[i - 1][j - x] + 1)
        return -1 if f[n][target] <= 0 else f[n][target]
```

#### Java

```java
class Solution {
    public int lengthOfLongestSubsequence(List<Integer> nums, int target) {
        int n = nums.size();
        int[][] f = new int[n + 1][target + 1];
        final int inf = 1 << 30;
        for (int[] g : f) {
            Arrays.fill(g, -inf);
        }
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            int x = nums.get(i - 1);
            for (int j = 0; j <= target; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= x) {
                    f[i][j] = Math.max(f[i][j], f[i - 1][j - x] + 1);
                }
            }
        }
        return f[n][target] <= 0 ? -1 : f[n][target];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lengthOfLongestSubsequence(vector<int>& nums, int target) {
        int n = nums.size();
        int f[n + 1][target + 1];
        memset(f, -0x3f, sizeof(f));
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            int x = nums[i - 1];
            for (int j = 0; j <= target; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= x) {
                    f[i][j] = max(f[i][j], f[i - 1][j - x] + 1);
                }
            }
        }
        return f[n][target] <= 0 ? -1 : f[n][target];
    }
};
```

#### Go

```go
func lengthOfLongestSubsequence(nums []int, target int) int {
	n := len(nums)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, target+1)
		for j := range f[i] {
			f[i][j] = -(1 << 30)
		}
	}
	f[0][0] = 0
	for i := 1; i <= n; i++ {
		x := nums[i-1]
		for j := 0; j <= target; j++ {
			f[i][j] = f[i-1][j]
			if j >= x {
				f[i][j] = max(f[i][j], f[i-1][j-x]+1)
			}
		}
	}
	if f[n][target] <= 0 {
		return -1
	}
	return f[n][target]
}
```

#### TypeScript

```ts
function lengthOfLongestSubsequence(nums: number[], target: number): number {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(target + 1).fill(-Infinity));
    f[0][0] = 0;
    for (let i = 1; i <= n; ++i) {
        const x = nums[i - 1];
        for (let j = 0; j <= target; ++j) {
            f[i][j] = f[i - 1][j];
            if (j >= x) {
                f[i][j] = Math.max(f[i][j], f[i - 1][j - x] + 1);
            }
        }
    }
    return f[n][target] <= 0 ? -1 : f[n][target];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> $f[i][j]$ của phương pháp 1 chỉ phụ thuộc vào hàng trước đó, nên có thể loại bỏ chỉ số đầu tiên. Mỗi số được sử dụng nhiều nhất một lần, vì vậy vòng lặp bên trong duyệt sức chứa theo thứ tự giảm dần. Độ phức tạp không gian giảm xuống còn $O(target)$ mà vẫn cho cùng đáp án.

<!-- thinking:end -->

$f[i][j]$ chỉ phụ thuộc vào hàng trước đó $f[i-1][\cdot]$, nên có thể loại bỏ chiều đầu tiên. Mỗi số chỉ được sử dụng nhiều nhất một lần, vì vậy $j$ được cập nhật từ lớn đến nhỏ. Độ phức tạp không gian trở thành $O(target)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lengthOfLongestSubsequence(self, nums: List[int], target: int) -> int:
        f = [0] + [-inf] * target
        for x in nums:
            for j in range(target, x - 1, -1):
                f[j] = max(f[j], f[j - x] + 1)
        return -1 if f[-1] <= 0 else f[-1]
```

#### Java

```java
class Solution {
    public int lengthOfLongestSubsequence(List<Integer> nums, int target) {
        int[] f = new int[target + 1];
        final int inf = 1 << 30;
        Arrays.fill(f, -inf);
        f[0] = 0;
        for (int x : nums) {
            for (int j = target; j >= x; --j) {
                f[j] = Math.max(f[j], f[j - x] + 1);
            }
        }
        return f[target] <= 0 ? -1 : f[target];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lengthOfLongestSubsequence(vector<int>& nums, int target) {
        int f[target + 1];
        memset(f, -0x3f, sizeof(f));
        f[0] = 0;
        for (int x : nums) {
            for (int j = target; j >= x; --j) {
                f[j] = max(f[j], f[j - x] + 1);
            }
        }
        return f[target] <= 0 ? -1 : f[target];
    }
};
```

#### Go

```go
func lengthOfLongestSubsequence(nums []int, target int) int {
	f := make([]int, target+1)
	for i := range f {
		f[i] = -(1 << 30)
	}
	f[0] = 0
	for _, x := range nums {
		for j := target; j >= x; j-- {
			f[j] = max(f[j], f[j-x]+1)
		}
	}
	if f[target] <= 0 {
		return -1
	}
	return f[target]
}
```

#### TypeScript

```ts
function lengthOfLongestSubsequence(nums: number[], target: number): number {
    const f: number[] = Array(target + 1).fill(-Infinity);
    f[0] = 0;
    for (const x of nums) {
        for (let j = target; j >= x; --j) {
            f[j] = Math.max(f[j], f[j - x] + 1);
        }
    }
    return f[target] <= 0 ? -1 : f[target];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
