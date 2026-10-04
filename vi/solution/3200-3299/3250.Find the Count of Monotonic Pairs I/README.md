---
comments: true
difficulty: Hard
rating: 1897
source: Weekly Contest 410 Q3
tags:
    - Array
    - Math
    - Dynamic Programming
    - Combinatorics
    - Prefix Sum
---

<!-- problem:start -->

# [3250. Find the Count of Monotonic Pairs I](https://leetcode.com/problems/find-the-count-of-monotonic-pairs-i)

[中文文档](/solution/3200-3299/3250.Find%20the%20Count%20of%20Monotonic%20Pairs%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>dương</strong> <code>nums</code> có độ dài <code>n</code>.</p>

<p>Một cặp gồm hai mảng số nguyên <strong>không âm</strong> <code>(arr1, arr2)</code> được gọi là <strong>đơn điệu</strong> nếu:</p>

<ul>
	<li>Độ dài của cả hai mảng đều là <code>n</code>.</li>
	<li><code>arr1</code> là mảng đơn điệu <strong>không giảm</strong>, nói cách khác, <code>arr1[0] &lt;= arr1[1] &lt;= ... &lt;= arr1[n - 1]</code>.</li>
	<li><code>arr2</code> là mảng đơn điệu <strong>không tăng</strong>, nói cách khác, <code>arr2[0] &gt;= arr2[1] &gt;= ... &gt;= arr2[n - 1]</code>.</li>
	<li><code>arr1[i] + arr2[i] == nums[i]</code> với mọi <code>0 &lt;= i &lt;= n - 1</code>.</li>
</ul>

<p>Trả về số lượng các cặp <strong>đơn điệu</strong>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án theo <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp thỏa mãn là:</p>

<ol>
	<li><code>([0, 1, 1], [2, 2, 1])</code></li>
	<li><code>([0, 1, 2], [2, 2, 0])</code></li>
	<li><code>([0, 2, 2], [2, 1, 0])</code></li>
	<li><code>([1, 2, 2], [1, 1, 0])</code></li>
</ol>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,5,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">126</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 2000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Tối ưu hóa tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Ta chia $nums[i]$ thành $arr1[i]+arr2[i]$, trong đó $arr1$ không giảm còn $arr2$ không tăng. Vì $n\le 2000$ và $nums[i]\le 50$, việc liệt kê các cách chia là dạng lũy thừa. Trạng thái là “xét đến $i$ với $arr1[i]=j$”.
>
> Tính đơn điệu của $arr1$ yêu cầu một $j'\le j$ ở bước trước; còn của $arr2$ yêu cầu $j'\le j+nums[i-1]-nums[i]$. Các $j'$ khả dĩ tạo thành một tiền tố, nên dùng tổng tiền tố của $f[i-1]$ để chuyển trạng thái trong $O(1)$. Đáp án là tổng của hàng cuối cùng.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số lượng cặp mảng đơn điệu trên đoạn con $[0, \ldots, i]$ sao cho $arr1[i] = j$. Ban đầu, $f[i][j] = 0$, và đáp án là $\sum_{j=0}^{\textit{nums}[n-1]} f[n-1][j]$.

Khi $i = 0$, ta có $f[0][j] = 1$ với $0 \leq j \leq \textit{nums}[0]$.

Khi $i > 0$, ta có thể tính $f[i][j]$ dựa trên $f[i-1][j']$. Vì $\textit{arr1}$ không giảm, $j' \leq j$. Ngoài ra, vì $\textit{arr2}$ không tăng, $\textit{nums}[i] - j \leq \textit{nums}[i - 1] - j'$. Do đó, $j' \leq \min(j, j + \textit{nums}[i - 1] - \textit{nums}[i])$.

Đáp án là $\sum_{j=0}^{\textit{nums}[n-1]} f[n-1][j]$.

Độ phức tạp thời gian là $O(n \times m)$, và độ phức tạp không gian là $O(n \times m)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$, còn $m$ là giá trị lớn nhất trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOfPairs(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        n, m = len(nums), max(nums)
        f = [[0] * (m + 1) for _ in range(n)]
        for j in range(nums[0] + 1):
            f[0][j] = 1
        for i in range(1, n):
            s = list(accumulate(f[i - 1]))
            for j in range(nums[i] + 1):
                k = min(j, j + nums[i - 1] - nums[i])
                if k >= 0:
                    f[i][j] = s[k] % mod
        return sum(f[-1][: nums[-1] + 1]) % mod
```

#### Java

```java
class Solution {
    public int countOfPairs(int[] nums) {
        final int mod = (int) 1e9 + 7;
        int n = nums.length;
        int m = Arrays.stream(nums).max().getAsInt();
        int[][] f = new int[n][m + 1];
        for (int j = 0; j <= nums[0]; ++j) {
            f[0][j] = 1;
        }
        int[] g = new int[m + 1];
        for (int i = 1; i < n; ++i) {
            g[0] = f[i - 1][0];
            for (int j = 1; j <= m; ++j) {
                g[j] = (g[j - 1] + f[i - 1][j]) % mod;
            }
            for (int j = 0; j <= nums[i]; ++j) {
                int k = Math.min(j, j + nums[i - 1] - nums[i]);
                if (k >= 0) {
                    f[i][j] = g[k];
                }
            }
        }
        int ans = 0;
        for (int j = 0; j <= nums[n - 1]; ++j) {
            ans = (ans + f[n - 1][j]) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countOfPairs(vector<int>& nums) {
        const int mod = 1e9 + 7;
        int n = nums.size();
        int m = *max_element(nums.begin(), nums.end());
        vector<vector<int>> f(n, vector<int>(m + 1));
        for (int j = 0; j <= nums[0]; ++j) {
            f[0][j] = 1;
        }
        vector<int> g(m + 1);
        for (int i = 1; i < n; ++i) {
            g[0] = f[i - 1][0];
            for (int j = 1; j <= m; ++j) {
                g[j] = (g[j - 1] + f[i - 1][j]) % mod;
            }
            for (int j = 0; j <= nums[i]; ++j) {
                int k = min(j, j + nums[i - 1] - nums[i]);
                if (k >= 0) {
                    f[i][j] = g[k];
                }
            }
        }
        int ans = 0;
        for (int j = 0; j <= nums[n - 1]; ++j) {
            ans = (ans + f[n - 1][j]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func countOfPairs(nums []int) (ans int) {
	const mod int = 1e9 + 7
	n := len(nums)
	m := slices.Max(nums)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, m+1)
	}
	for j := 0; j <= nums[0]; j++ {
		f[0][j] = 1
	}
	g := make([]int, m+1)
	for i := 1; i < n; i++ {
		g[0] = f[i-1][0]
		for j := 1; j <= m; j++ {
			g[j] = (g[j-1] + f[i-1][j]) % mod
		}
		for j := 0; j <= nums[i]; j++ {
			k := min(j, j+nums[i-1]-nums[i])
			if k >= 0 {
				f[i][j] = g[k]
			}
		}
	}
	for j := 0; j <= nums[n-1]; j++ {
		ans = (ans + f[n-1][j]) % mod
	}
	return
}
```

#### TypeScript

```ts
function countOfPairs(nums: number[]): number {
    const mod = 1e9 + 7;
    const n = nums.length;
    const m = Math.max(...nums);
    const f: number[][] = Array.from({ length: n }, () => Array(m + 1).fill(0));
    for (let j = 0; j <= nums[0]; j++) {
        f[0][j] = 1;
    }
    const g: number[] = Array(m + 1).fill(0);
    for (let i = 1; i < n; i++) {
        g[0] = f[i - 1][0];
        for (let j = 1; j <= m; j++) {
            g[j] = (g[j - 1] + f[i - 1][j]) % mod;
        }
        for (let j = 0; j <= nums[i]; j++) {
            const k = Math.min(j, j + nums[i - 1] - nums[i]);
            if (k >= 0) {
                f[i][j] = g[k];
            }
        }
    }
    let ans = 0;
    for (let j = 0; j <= nums[n - 1]; j++) {
        ans = (ans + f[n - 1][j]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
