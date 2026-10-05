---
comments: true
difficulty: Medium
rating: 1745
source: Weekly Contest 494 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3877. Minimum Removals to Achieve Target XOR](https://leetcode.com/problems/minimum-removals-to-achieve-target-xor)

[中文文档](/solution/3800-3899/3877.Minimum%20Removals%20to%20Achieve%20Target%20XOR/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>target</code>.</p>

<p>Bạn có thể xóa <strong>bất kỳ</strong> số lượng phần tử nào khỏi <code>nums</code> (có thể không xóa phần tử nào).</p>

<p>Hãy trả về số lượng phần tử bị xóa <strong>ít nhất</strong> cần thiết để <strong>XOR bitwise</strong> của các phần tử còn lại bằng <code>target</code>. Nếu không thể đạt được <code>target</code>, hãy trả về -1.</p>

<p>XOR bitwise của một mảng rỗng là 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], target = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xóa <code>nums[1] = 2</code> sẽ còn lại <code>[nums[0], nums[2]] = [1, 3]</code>.</li>
	<li>XOR của <code>[1, 3]</code> là 2, bằng với <code>target</code>.</li>
	<li>Không thể đạt được XOR = 2 nếu xóa ít hơn một phần tử, vì vậy đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4], target = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể xóa các phần tử để đạt được <code>target</code>. Vì vậy, đáp án là -1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7], target = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>XOR của tất cả phần tử là <code>nums[0] = 7</code>, bằng với <code>target</code>. Vì vậy, không cần xóa phần tử nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 40</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= target &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Xóa ít phần tử nhất sao cho XOR của các phần tử còn lại bằng $\textit{target}$. Vì $n \le 40$ và các giá trị $\le 10^4$, không gian XOR có kích thước khoảng $2^{14}$.
>
> Tương đương, ta chọn nhiều phần tử nhất có XOR bằng $\textit{target}$, rồi chuyển số phần tử còn lại thành số phần tử cần xóa.
>
> Gọi $f[i][j]$ là số phần tử nhiều nhất trong $i$ phần tử đầu tiên có XOR bằng $j$, với mỗi phần tử hiện tại ta có thể chọn hoặc bỏ qua.
>
> Nếu $\textit{target}$ đã vượt quá không gian các bit của giá trị thì không thể đạt được; ngược lại, đáp án là $n-f[n][\textit{target}]$.

<!-- thinking:end -->

Ta định nghĩa một mảng 2 chiều $f$, trong đó $f[i][j]$ biểu diễn số phần tử lớn nhất có thể chọn từ $i$ phần tử đầu tiên sao cho XOR của chúng bằng $j$. Ban đầu, $f[0][0] = 0$ và mọi $f[0][j]$ khác đều là âm vô cùng.

Với mỗi phần tử $nums[i - 1]$, ta có thể không sử dụng nó, khi đó $f[i][j]$ bằng $f[i - 1][j]$; hoặc sử dụng nó, khi đó $f[i][j]$ bằng $f[i - 1][j \oplus nums[i - 1]] + 1$. Do đó, công thức chuyển trạng thái là:

$$
\begin{aligned}
f[i][j] = \max(f[i - 1][j], f[i - 1][j \oplus nums[i - 1]] + 1)
\end{aligned}
$$

Cuối cùng, nếu $f[n][target]$ nhỏ hơn $0$, điều đó có nghĩa là không thể đạt được giá trị XOR của target, và ta trả về $-1$; ngược lại, ta trả về $n - f[n][target]$, là số phần tử cần xóa.

Độ phức tạp thời gian là $O(n \times 2^m)$ và độ phức tạp không gian là $O(n \times 2^m)$, trong đó $n$ là độ dài của mảng và $m$ là số bit nhị phân của phần tử lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minRemovals(self, nums: List[int], target: int) -> int:
        m = max(nums).bit_length()
        if (1 << m) <= target:
            return -1
        n = len(nums)
        f = [[-inf] * (1 << m) for _ in range(n + 1)]
        f[0][0] = 0
        for i, x in enumerate(nums, 1):
            for j in range(1 << m):
                f[i][j] = max(f[i - 1][j], f[i - 1][j ^ x] + 1)
        if f[n][target] < 0:
            return -1
        return n - f[n][target]
```

#### Java

```java
class Solution {
    public int minRemovals(int[] nums, int target) {
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        int m = 32 - Integer.numberOfLeadingZeros(mx);
        if ((1 << m) <= target) {
            return -1;
        }

        int n = nums.length;
        int[][] f = new int[n + 1][1 << m];
        for (int i = 0; i <= n; i++) {
            Arrays.fill(f[i], Integer.MIN_VALUE);
        }
        f[0][0] = 0;

        for (int i = 1; i <= n; i++) {
            int x = nums[i - 1];
            for (int j = 0; j < (1 << m); j++) {
                f[i][j] = Math.max(f[i - 1][j], f[i - 1][j ^ x] + 1);
            }
        }

        if (f[n][target] < 0) {
            return -1;
        }
        return n - f[n][target];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minRemovals(vector<int>& nums, int target) {
        int mx = ranges::max(nums);
        int m = 0;
        while ((1 << m) <= mx) {
            ++m;
        }
        if ((1 << m) <= target) {
            return -1;
        }

        int n = nums.size();
        vector<vector<int>> f(n + 1, vector<int>(1 << m, INT_MIN));
        f[0][0] = 0;

        for (int i = 1; i <= n; i++) {
            int x = nums[i - 1];
            for (int j = 0; j < (1 << m); j++) {
                f[i][j] = max(f[i - 1][j], f[i - 1][j ^ x] + 1);
            }
        }

        if (f[n][target] < 0) {
            return -1;
        }
        return n - f[n][target];
    }
};
```

#### Go

```go
func minRemovals(nums []int, target int) int {
	m := bits.Len(uint(slices.Max(nums)))
	if (1 << m) <= target {
		return -1
	}

	n := len(nums)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, 1<<m)
		for j := range f[i] {
			f[i][j] = math.MinInt
		}
	}
	f[0][0] = 0

	for i := 1; i <= n; i++ {
		x := nums[i-1]
		for j := 0; j < (1 << m); j++ {
			f[i][j] = max(f[i-1][j], f[i-1][j^x]+1)
		}
	}

	if f[n][target] < 0 {
		return -1
	}
	return n - f[n][target]
}
```

#### TypeScript

```ts
function minRemovals(nums: number[], target: number): number {
    let mx = Math.max(...nums);

    let m = 0;
    while (1 << m <= mx) {
        m++;
    }
    if (1 << m <= target) {
        return -1;
    }

    const n = nums.length;
    const f = Array.from({ length: n + 1 }, () => Array(1 << m).fill(-Infinity));

    f[0][0] = 0;

    for (let i = 1; i <= n; i++) {
        const x = nums[i - 1];
        for (let j = 0; j < 1 << m; j++) {
            f[i][j] = Math.max(f[i - 1][j], f[i - 1][j ^ x] + 1);
        }
    }

    if (f[n][target] < 0) {
        return -1;
    }
    return n - f[n][target];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
