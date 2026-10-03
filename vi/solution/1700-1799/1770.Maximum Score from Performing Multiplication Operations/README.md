---
comments: true
difficulty: Hard
rating: 2068
source: Weekly Contest 229 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1770. Maximum Score from Performing Multiplication Operations](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations)

[中文文档](/solution/1700-1799/1770.Maximum%20Score%20from%20Performing%20Multiplication%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>đánh số từ 0</strong> <code>nums</code> và <code>multipliers</code><strong> </strong>, lần lượt có kích thước <code>n</code> và <code>m</code>, trong đó <code>n &gt;= m</code>.</p>

<p>Ban đầu điểm số là <code>0</code>. Bạn cần thực hiện <strong>chính xác</strong> <code>m</code> thao tác. Ở thao tác thứ <code>i<sup>th</sup></code> (<strong>đánh số từ 0</strong>), bạn sẽ:</p>

<ul>
<li>Chọn một số nguyên <code>x</code> ở <strong>đầu hoặc cuối</strong> mảng <code>nums</code>.</li>
<li>Cộng <code>multipliers[i] * x</code> vào điểm số.
    <ul>
<li>Lưu ý rằng <code>multipliers[0]</code> tương ứng với thao tác đầu tiên, <code>multipliers[1]</code> với thao tác thứ hai, và tương tự.</li>
    </ul>
    </li>
<li>Xóa <code>x</code> khỏi <code>nums</code>.</li>
</ul>

<p>Trả về <em>điểm số <strong>lớn nhất</strong> sau khi thực hiện </em><code>m</code> <em>thao tác.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], multipliers = [3,2,1]
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong>&nbsp;Một cách tối ưu là:
- Chọn ở cuối, [1,2,<strong><u>3</u></strong>], cộng 3 * 3 = 9 vào điểm số.
- Chọn ở cuối, [1,<strong><u>2</u></strong>], cộng 2 * 2 = 4 vào điểm số.
- Chọn ở cuối, [<strong><u>1</u></strong>], cộng 1 * 1 = 1 vào điểm số.
Tổng điểm là 9 + 4 + 1 = 14.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-5,-3,-3,-2,7,1], multipliers = [-10,-5,3,4,6]
<strong>Đầu ra:</strong> 102
<strong>Giải thích: </strong>Một cách tối ưu là:
- Chọn ở đầu, [<u><strong>-5</strong></u>,-3,-3,-2,7,1], cộng -5 * -10 = 50 vào điểm số.
- Chọn ở đầu, [<strong><u>-3</u></strong>,-3,-2,7,1], cộng -3 * -5 = 15 vào điểm số.
- Chọn ở đầu, [<strong><u>-3</u></strong>,-2,7,1], cộng -3 * 3 = -9 vào điểm số.
- Chọn ở cuối, [-2,7,<strong><u>1</u></strong>], cộng 1 * 4 = 4 vào điểm số.
- Chọn ở cuối, [-2,<strong><u>7</u></strong>], cộng 7 * 6 = 42 vào điểm số.
Tổng điểm là 50 + 15 - 9 + 4 + 42 = 102.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>m == multipliers.length</code></li>
	<li><code>1 &lt;= m &lt;= 300</code></li>
	<li><code>m &lt;= n &lt;= 10<sup>5</sup></code><code> </code></li>
	<li><code>-1000 &lt;= nums[i], multipliers[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước nhân một đầu của $nums$ với $\textit{multipliers}[k]$. Sau $m$ bước, chỉ hai đầu thay đổi, nên một trạng thái được xác định bởi số phần tử đã lấy ở bên trái và bên phải.
>
> Ghi nhớ $f(i,j,k)$ với chỉ số trái $i$, chỉ số phải $j$ và hệ số $k$. Đệ quy theo việc lấy bên trái hoặc bên phải, dừng khi $k=m$. Có $O(m^2)$ trạng thái.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, nums: List[int], multipliers: List[int]) -> int:
        @cache
        def f(i, j, k):
            if k >= m or i >= n or j < 0:
                return 0
            a = f(i + 1, j, k + 1) + nums[i] * multipliers[k]
            b = f(i, j - 1, k + 1) + nums[j] * multipliers[k]
            return max(a, b)

        n = len(nums)
        m = len(multipliers)
        return f(0, n - 1, 0)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int[] multipliers;
    private int[] nums;
    private int n;
    private int m;

    public int maximumScore(int[] nums, int[] multipliers) {
        n = nums.length;
        m = multipliers.length;
        f = new Integer[m][m];
        this.nums = nums;
        this.multipliers = multipliers;
        return dfs(0, 0);
    }

    private int dfs(int i, int j) {
        if (i >= m || j >= m || (i + j) >= m) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int k = i + j;
        int a = dfs(i + 1, j) + nums[i] * multipliers[k];
        int b = dfs(i, j + 1) + nums[n - 1 - j] * multipliers[k];
        f[i][j] = Math.max(a, b);
        return f[i][j];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumScore(vector<int>& nums, vector<int>& multipliers) {
        int n = nums.size(), m = multipliers.size();
        int f[m][m];
        memset(f, 0x3f, sizeof f);
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i >= m || j >= m || (i + j) >= m) return 0;
            if (f[i][j] != 0x3f3f3f3f) return f[i][j];
            int k = i + j;
            int a = dfs(i + 1, j) + nums[i] * multipliers[k];
            int b = dfs(i, j + 1) + nums[n - j - 1] * multipliers[k];
            return f[i][j] = max(a, b);
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func maximumScore(nums []int, multipliers []int) int {
	n, m := len(nums), len(multipliers)
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, m)
		for j := range f[i] {
			f[i][j] = 1 << 30
		}
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i >= m || j >= m || i+j >= m {
			return 0
		}
		if f[i][j] != 1<<30 {
			return f[i][j]
		}
		k := i + j
		a := dfs(i+1, j) + nums[i]*multipliers[k]
		b := dfs(i, j+1) + nums[n-j-1]*multipliers[k]
		f[i][j] = max(a, b)
		return f[i][j]
	}
	return dfs(0, 0)
}
```

#### TypeScript

```ts
function maximumScore(nums: number[], multipliers: number[]): number {
    const inf = 1 << 30;
    const n = nums.length;
    const m = multipliers.length;
    const f = new Array(m + 1).fill(0).map(() => new Array(m + 1).fill(-inf));
    f[0][0] = 0;
    let ans = -inf;
    for (let i = 0; i <= m; ++i) {
        for (let j = 0; j <= m - i; ++j) {
            const k = i + j - 1;
            if (i > 0) {
                f[i][j] = Math.max(f[i][j], f[i - 1][j] + nums[i - 1] * multipliers[k]);
            }
            if (j > 0) {
                f[i][j] = Math.max(f[i][j], f[i][j - 1] + nums[n - j] * multipliers[k]);
            }
            if (i + j === m) {
                ans = Math.max(ans, f[i][j]);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lập bảng theo cùng ý tưởng: $f[i][j]$ là điểm tốt nhất sau khi lấy $i$ phần tử đầu và $j$ phần tử cuối. Điền bảng theo thứ tự tăng của $i+j$ và ghi nhận đáp án khi $i+j=m$. Độ phức tạp vẫn là $O(m^2)$ nhưng không cần đệ quy.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, nums: List[int], multipliers: List[int]) -> int:
        n, m = len(nums), len(multipliers)
        f = [[-inf] * (m + 1) for _ in range(m + 1)]
        f[0][0] = 0
        ans = -inf
        for i in range(m + 1):
            for j in range(m - i + 1):
                k = i + j - 1
                if i > 0:
                    f[i][j] = max(f[i][j], f[i - 1][j] + multipliers[k] * nums[i - 1])
                if j > 0:
                    f[i][j] = max(f[i][j], f[i][j - 1] + multipliers[k] * nums[n - j])
                if i + j == m:
                    ans = max(ans, f[i][j])
        return ans
```

#### Java

```java
class Solution {
    public int maximumScore(int[] nums, int[] multipliers) {
        final int inf = 1 << 30;
        int n = nums.length, m = multipliers.length;
        int[][] f = new int[m + 1][m + 1];
        for (int i = 0; i <= m; i++) {
            Arrays.fill(f[i], -inf);
        }
        f[0][0] = 0;
        int ans = -inf;
        for (int i = 0; i <= m; ++i) {
            for (int j = 0; j <= m - i; ++j) {
                int k = i + j - 1;
                if (i > 0) {
                    f[i][j] = Math.max(f[i][j], f[i - 1][j] + multipliers[k] * nums[i - 1]);
                }
                if (j > 0) {
                    f[i][j] = Math.max(f[i][j], f[i][j - 1] + multipliers[k] * nums[n - j]);
                }
                if (i + j == m) {
                    ans = Math.max(ans, f[i][j]);
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumScore(vector<int>& nums, vector<int>& multipliers) {
        const int inf = 1 << 30;
        int n = nums.size(), m = multipliers.size();
        vector<vector<int>> f(m + 1, vector<int>(m + 1, -inf));
        f[0][0] = 0;
        int ans = -inf;
        for (int i = 0; i <= m; ++i) {
            for (int j = 0; j <= m - i; ++j) {
                int k = i + j - 1;
                if (i > 0) {
                    f[i][j] = max(f[i][j], f[i - 1][j] + multipliers[k] * nums[i - 1]);
                }
                if (j > 0) {
                    f[i][j] = max(f[i][j], f[i][j - 1] + multipliers[k] * nums[n - j]);
                }
                if (i + j == m) {
                    ans = max(ans, f[i][j]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumScore(nums []int, multipliers []int) int {
	const inf int = 1 << 30
	n, m := len(nums), len(multipliers)
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, m+1)
		for j := range f {
			f[i][j] = -inf
		}
	}
	f[0][0] = 0
	ans := -inf
	for i := 0; i <= m; i++ {
		for j := 0; j <= m-i; j++ {
			k := i + j - 1
			if i > 0 {
				f[i][j] = max(f[i][j], f[i-1][j]+multipliers[k]*nums[i-1])
			}
			if j > 0 {
				f[i][j] = max(f[i][j], f[i][j-1]+multipliers[k]*nums[n-j])
			}
			if i+j == m {
				ans = max(ans, f[i][j])
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
