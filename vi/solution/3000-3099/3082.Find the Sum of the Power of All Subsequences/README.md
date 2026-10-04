---
comments: true
difficulty: Hard
rating: 2241
source: Biweekly Contest 126 Q4
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - 0-1 Knapsack
---

<!-- problem:start -->

# [3082. Find the Sum of the Power of All Subsequences](https://leetcode.com/problems/find-the-sum-of-the-power-of-all-subsequences)

[中文文档](/solution/3000-3099/3082.Find%20the%20Sum%20of%20the%20Power%20of%20All%20Subsequences/README.md)

## Mô tả

<!-- description:start -->
<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p><strong>Power</strong> của một mảng số nguyên được định nghĩa là số lượng <span data-keyword="subsequence-array">dãy con</span> có tổng <strong>bằng</strong> <code>k</code>.</p>

<p>Hãy trả về <em><strong>tổng</strong> <strong>power</strong> của tất cả các dãy con của</em> <code>nums</code><em>.</em></p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>chia lấy dư</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> nums = [1,2,3], k = 3 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 6 </span></p>

<p><strong>Giải thích:</strong></p>

<p>Có <code>5</code> dãy con của nums có power khác 0:</p>

<ul>
	<li>Dãy con <code>[<u><strong>1</strong></u>,<u><strong>2</strong></u>,<u><strong>3</strong></u>]</code> có <code>2</code> dãy con có <code>sum == 3</code>: <code>[1,2,<u>3</u>]</code> và <code>[<u>1</u>,<u>2</u>,3]</code>.</li>
	<li>Dãy con <code>[<u><strong>1</strong></u>,2,<u><strong>3</strong></u>]</code> có <code>1</code> dãy con có <code>sum == 3</code>: <code>[1,2,<u>3</u>]</code>.</li>
	<li>Dãy con <code>[1,<u><strong>2</strong></u>,<u><strong>3</strong></u>]</code> có <code>1</code> dãy con có <code>sum == 3</code>: <code>[1,2,<u>3</u>]</code>.</li>
	<li>Dãy con <code>[<u><strong>1</strong></u>,<u><strong>2</strong></u>,3]</code> có <code>1</code> dãy con có <code>sum == 3</code>: <code>[<u>1</u>,<u>2</u>,3]</code>.</li>
	<li>Dãy con <code>[1,2,<u><strong>3</strong></u>]</code> có <code>1</code> dãy con có <code>sum == 3</code>: <code>[1,2,<u>3</u>]</code>.</li>
</ul>

<p>Vậy đáp án là <code>2 + 1 + 1 + 1 + 1 = 6</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> nums = [2,3,3], k = 5 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 4 </span></p>

<p><strong>Giải thích:</strong></p>

<p>Có <code>3</code> dãy con của nums có power khác 0:</p>

<ul>
	<li>Dãy con <code>[<u><strong>2</strong></u>,<u><strong>3</strong></u>,<u><strong>3</strong></u>]</code> có 2 dãy con có <code>sum == 5</code>: <code>[<u>2</u>,3,<u>3</u>]</code> và <code>[<u>2</u>,<u>3</u>,3]</code>.</li>
	<li>Dãy con <code>[<u><strong>2</strong></u>,3,<u><strong>3</strong></u>]</code> có 1 dãy con có <code>sum == 5</code>: <code>[<u>2</u>,3,<u>3</u>]</code>.</li>
	<li>Dãy con <code>[<u><strong>2</strong></u>,<u><strong>3</strong></u>,3]</code> có 1 dãy con có <code>sum == 5</code>: <code>[<u>2</u>,<u>3</u>,3]</code>.</li>
</ul>

<p>Vậy đáp án là <code>2 + 1 + 1 = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> nums = [1,2,3], k = 7 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 0 </span></p>

<p><strong>Giải thích:&nbsp;</strong>Không tồn tại dãy con nào có tổng bằng <code>7</code>. Do đó, mọi dãy con của nums đều có <code>power = 0</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi dãy con $S$, ta đếm các dãy con của $S$ có tổng bằng $k$, sau đó cộng các số lượng này lại. $n,k \le 100$.
>
> Mỗi phần tử có thể nằm ngoài $S$, nằm trong $S$ nhưng không thuộc dãy con được tính tổng $T$, hoặc nằm trong $T$. Hai trường hợp đầu có cùng tổng và đều sao chép trạng thái trước đó.
>
> Do đó, $f[i][j]=2f[i-1][j]+f[i-1][j-x]$ với $f[0][0]=1$.

<!-- thinking:end -->

Bài toán yêu cầu tìm tất cả các dãy con $\textit{S}$ trong mảng $\textit{nums}$ đã cho, sau đó tính số cách tạo dãy con $\textit{T}$ sao cho tổng các phần tử của $\textit{T}$ bằng $\textit{k}$.

Ta định nghĩa $f[i][j]$ là số cách tạo các dãy con bằng $i$ số đầu tiên sao cho tổng của mỗi dãy con bằng $j$. Ban đầu, $f[0][0] = 1$, các vị trí còn lại đều bằng $0$.

Với số thứ $i$ là $x$, có ba trường hợp:

1. Không nằm trong dãy con $\textit{S}$, khi đó $f[i][j] = f[i-1][j]$;
2. Nằm trong dãy con $\textit{S}$ nhưng không nằm trong dãy con $\textit{T}$, khi đó $f[i][j] = f[i-1][j]$;
3. Nằm trong cả dãy con $\textit{S}$ và dãy con $\textit{T}$, khi đó $f[i][j] = f[i-1][j-x]$.

Tóm lại, công thức chuyển trạng thái là:

$$
f[i][j] = f[i-1][j] \times 2 + f[i-1][j-x]
$$

Đáp án cuối cùng là $f[n][k]$.

Độ phức tạp thời gian là $O(n \times k)$, độ phức tạp không gian là $O(n \times k)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$ và $k$ là số nguyên dương đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfPower(self, nums: List[int], k: int) -> int:
        mod = 10**9 + 7
        n = len(nums)
        f = [[0] * (k + 1) for _ in range(n + 1)]
        f[0][0] = 1
        for i, x in enumerate(nums, 1):
            for j in range(k + 1):
                f[i][j] = f[i - 1][j] * 2 % mod
                if j >= x:
                    f[i][j] = (f[i][j] + f[i - 1][j - x]) % mod
        return f[n][k]
```

#### Java

```java
class Solution {
    public int sumOfPower(int[] nums, int k) {
        final int mod = (int) 1e9 + 7;
        int n = nums.length;
        int[][] f = new int[n + 1][k + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= k; ++j) {
                f[i][j] = (f[i - 1][j] * 2) % mod;
                if (j >= nums[i - 1]) {
                    f[i][j] = (f[i][j] + f[i - 1][j - nums[i - 1]]) % mod;
                }
            }
        }
        return f[n][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfPower(vector<int>& nums, int k) {
        const int mod = 1e9 + 7;
        int n = nums.size();
        int f[n + 1][k + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= k; ++j) {
                f[i][j] = (f[i - 1][j] * 2) % mod;
                if (j >= nums[i - 1]) {
                    f[i][j] = (f[i][j] + f[i - 1][j - nums[i - 1]]) % mod;
                }
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func sumOfPower(nums []int, k int) int {
	const mod int = 1e9 + 7
	n := len(nums)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
	}
	f[0][0] = 1
	for i := 1; i <= n; i++ {
		for j := 0; j <= k; j++ {
			f[i][j] = (f[i-1][j] * 2) % mod
			if j >= nums[i-1] {
				f[i][j] = (f[i][j] + f[i-1][j-nums[i-1]]) % mod
			}
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function sumOfPower(nums: number[], k: number): number {
    const mod = 10 ** 9 + 7;
    const n = nums.length;
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(k + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= n; ++i) {
        for (let j = 0; j <= k; ++j) {
            f[i][j] = (f[i - 1][j] * 2) % mod;
            if (j >= nums[i - 1]) {
                f[i][j] = (f[i][j] + f[i - 1][j - nums[i - 1]]) % mod;
            }
        }
    }
    return f[n][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming (Tối ưu hóa)

<!-- thinking:start -->

> **Tư duy**
>
> Công thức chuyển ở Phần I chỉ đọc hàng trước đó tại $j$ và $j-x$, vì vậy có thể loại bỏ chiều thứ nhất.
>
> Cập nhật mảng một chiều từ phía sau giúp tránh sử dụng lại $j-x$ vừa được ghi và giảm không gian phụ xuống còn $O(k)$.

<!-- thinking:end -->

Trong công thức chuyển trạng thái của Lời giải 1, giá trị của $f[i][j]$ chỉ phụ thuộc vào $f[i-1][j]$ và $f[i-1][j-x]$. Vì vậy, ta có thể tối ưu chiều thứ nhất của không gian, giảm độ phức tạp không gian xuống còn $O(k)$.

Độ phức tạp thời gian là $O(n \times k)$, độ phức tạp không gian là $O(k)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$ và $k$ là số nguyên dương đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfPower(self, nums: List[int], k: int) -> int:
        mod = 10**9 + 7
        f = [1] + [0] * k
        for x in nums:
            for j in range(k, -1, -1):
                f[j] = (f[j] * 2 + (0 if j < x else f[j - x])) % mod
        return f[k]
```

#### Java

```java
class Solution {
    public int sumOfPower(int[] nums, int k) {
        final int mod = (int) 1e9 + 7;
        int[] f = new int[k + 1];
        f[0] = 1;
        for (int x : nums) {
            for (int j = k; j >= 0; --j) {
                f[j] = (f[j] * 2 % mod + (j >= x ? f[j - x] : 0)) % mod;
            }
        }
        return f[k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfPower(vector<int>& nums, int k) {
        const int mod = 1e9 + 7;
        int f[k + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (int x : nums) {
            for (int j = k; j >= 0; --j) {
                f[j] = (f[j] * 2 % mod + (j >= x ? f[j - x] : 0)) % mod;
            }
        }
        return f[k];
    }
};
```

#### Go

```go
func sumOfPower(nums []int, k int) int {
	const mod int = 1e9 + 7
	f := make([]int, k+1)
	f[0] = 1
	for _, x := range nums {
		for j := k; j >= 0; j-- {
			f[j] = f[j] * 2 % mod
			if j >= x {
				f[j] = (f[j] + f[j-x]) % mod
			}
		}
	}
	return f[k]
}
```

#### TypeScript

```ts
function sumOfPower(nums: number[], k: number): number {
    const mod = 10 ** 9 + 7;
    const f: number[] = Array(k + 1).fill(0);
    f[0] = 1;
    for (const x of nums) {
        for (let j = k; ~j; --j) {
            f[j] = (f[j] * 2) % mod;
            if (j >= x) {
                f[j] = (f[j] + f[j - x]) % mod;
            }
        }
    }
    return f[k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
