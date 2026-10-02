---
comments: true
difficulty: Medium
rating: 1762
source: Weekly Contest 163 Q3
tags:
    - Greedy
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [1262. Greatest Sum Divisible by Three](https://leetcode.com/problems/greatest-sum-divisible-by-three)

[中文文档](/solution/1200-1299/1262.Greatest%20Sum%20Divisible%20by%20Three/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em><strong>tổng lớn nhất có thể</strong> của một số phần tử trong mảng sao cho tổng đó chia hết cho ba</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,6,5,1,8]
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Chọn các số 3, 6, 1 và 8 thì tổng bằng 18 (là tổng lớn nhất chia hết cho 3).</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Vì 4 không chia hết cho 3 nên không chọn số nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,4]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Chọn các số 1, 3, 4 và 4 thì tổng bằng 12 (là tổng lớn nhất chia hết cho 3).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng lớn nhất của một dãy con chia hết cho $3$. Vì $n \le 4\times 10^4$, không thể xét mọi tập con. Một tổng chỉ có ba số dư khi chia cho $3$; chọn hoặc bỏ phần tử hiện tại sẽ chuyển trạng thái giữa ba số dư này.
>
> $f[i][j]$ là tổng lớn nhất có thể chọn từ $i$ số đầu tiên sao cho số dư là $j$. Nếu bỏ qua phần tử thì giữ nguyên trạng thái; nếu chọn $x$, cộng $x$ vào trạng thái có số dư $j-x$. Đáp án là $f[n][0]$. Chỉ cần lưu số dư nên số trạng thái là hằng số.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là tổng lớn nhất có thể đạt được khi chọn một số phần tử trong $i$ số đầu tiên sao cho tổng chia cho $3$ dư $j$. Ban đầu, $f[0][0]=0$, các trạng thái còn lại bằng $-\infty$.

Để tính $f[i][j]$, ta xét hai khả năng đối với số thứ $i$, ký hiệu là $x$:

- Nếu không chọn $x$, ta có $f[i][j]=f[i-1][j]$;
- Nếu chọn $x$, ta có $f[i][j]=f[i-1][(j-x \bmod 3 + 3)\bmod 3]+x$.

Do đó, ta có công thức chuyển trạng thái:

$$
f[i][j]=\max\{f[i-1][j],f[i-1][(j-x \bmod 3 + 3)\bmod 3]+x\}
$$

Đáp án cuối cùng là $f[n][0]$.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumDivThree(self, nums: List[int]) -> int:
        n = len(nums)
        f = [[-inf] * 3 for _ in range(n + 1)]
        f[0][0] = 0
        for i, x in enumerate(nums, 1):
            for j in range(3):
                f[i][j] = max(f[i - 1][j], f[i - 1][(j - x) % 3] + x)
        return f[n][0]
```

#### Java

```java
class Solution {
    public int maxSumDivThree(int[] nums) {
        int n = nums.length;
        final int inf = 1 << 30;
        int[][] f = new int[n + 1][3];
        f[0][1] = f[0][2] = -inf;
        for (int i = 1; i <= n; ++i) {
            int x = nums[i - 1];
            for (int j = 0; j < 3; ++j) {
                f[i][j] = Math.max(f[i - 1][j], f[i - 1][(j - x % 3 + 3) % 3] + x);
            }
        }
        return f[n][0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSumDivThree(vector<int>& nums) {
        int n = nums.size();
        const int inf = 1 << 30;
        int f[n + 1][3];
        f[0][0] = 0;
        f[0][1] = f[0][2] = -inf;
        for (int i = 1; i <= n; ++i) {
            int x = nums[i - 1];
            for (int j = 0; j < 3; ++j) {
                f[i][j] = max(f[i - 1][j], f[i - 1][(j - x % 3 + 3) % 3] + x);
            }
        }
        return f[n][0];
    }
};
```

#### Go

```go
func maxSumDivThree(nums []int) int {
	n := len(nums)
	const inf = 1 << 30
	f := make([][3]int, n+1)
	f[0] = [3]int{0, -inf, -inf}
	for i, x := range nums {
		i++
		for j := 0; j < 3; j++ {
			f[i][j] = max(f[i-1][j], f[i-1][(j-x%3+3)%3]+x)
		}
	}
	return f[n][0]
}
```

#### TypeScript

```ts
function maxSumDivThree(nums: number[]): number {
    const n = nums.length;
    const inf = 1 << 30;
    const f: number[][] = Array(n + 1)
        .fill(0)
        .map(() => Array(3).fill(-inf));
    f[0][0] = 0;
    for (let i = 1; i <= n; ++i) {
        const x = nums[i - 1];
        for (let j = 0; j < 3; ++j) {
            f[i][j] = Math.max(f[i - 1][j], f[i - 1][(j - (x % 3) + 3) % 3] + x);
        }
    }
    return f[n][0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming (Rolling Array)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu $O(n)$ hàng. Hàng $i$ chỉ đọc ba giá trị ở hàng trước, nên có thể dùng rolling array độ dài $3$. Không gian phụ thêm là hằng số; công thức chuyển trạng thái không đổi.

<!-- thinking:end -->

$f[i][j]$ chỉ phụ thuộc vào ba số dư của hàng trước, nên chỉ cần một mảng độ dài $3$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumDivThree(self, nums: List[int]) -> int:
        f = [0, -inf, -inf]
        for x in nums:
            g = f[:]
            for j in range(3):
                g[j] = max(f[j], f[(j - x) % 3] + x)
            f = g
        return f[0]
```

#### Java

```java
class Solution {
    public int maxSumDivThree(int[] nums) {
        final int inf = 1 << 30;
        int[] f = new int[] {0, -inf, -inf};
        for (int x : nums) {
            int[] g = f.clone();
            for (int j = 0; j < 3; ++j) {
                g[j] = Math.max(f[j], f[(j - x % 3 + 3) % 3] + x);
            }
            f = g;
        }
        return f[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSumDivThree(vector<int>& nums) {
        const int inf = 1 << 30;
        vector<int> f = {0, -inf, -inf};
        for (int& x : nums) {
            vector<int> g = f;
            for (int j = 0; j < 3; ++j) {
                g[j] = max(f[j], f[(j - x % 3 + 3) % 3] + x);
            }
            f = move(g);
        }
        return f[0];
    }
};
```

#### Go

```go
func maxSumDivThree(nums []int) int {
	const inf = 1 << 30
	f := [3]int{0, -inf, -inf}
	for _, x := range nums {
		g := [3]int{}
		for j := range f {
			g[j] = max(f[j], f[(j-x%3+3)%3]+x)
		}
		f = g
	}
	return f[0]
}
```

#### TypeScript

```ts
function maxSumDivThree(nums: number[]): number {
    const inf = 1 << 30;
    const f: number[] = [0, -inf, -inf];
    for (const x of nums) {
        const g = [...f];
        for (let j = 0; j < 3; ++j) {
            f[j] = Math.max(g[j], g[(j - (x % 3) + 3) % 3] + x);
        }
    }
    return f[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
