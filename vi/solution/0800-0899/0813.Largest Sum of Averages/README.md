---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [813. Largest Sum of Averages](https://leetcode.com/problems/largest-sum-of-averages)

[中文文档](/solution/0800-0899/0813.Largest%20Sum%20of%20Averages/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>. Bạn có thể chia mảng thành <strong>nhiều nhất</strong> <code>k</code> mảng con liền kề không rỗng. <strong>Điểm số</strong> của cách chia là tổng giá trị trung bình của từng mảng con.</p>

<p>Lưu ý, cách chia phải sử dụng mọi số nguyên trong <code>nums</code>, và điểm số không nhất thiết là số nguyên.</p>

<p>Hãy trả về <em><strong>điểm số</strong> lớn nhất có thể đạt được trong tất cả các cách chia</em>. Đáp án có sai số không quá <code>10<sup>-6</sup></code> so với kết quả thực tế sẽ được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9,1,2,3,9], k = 3
<strong>Đầu ra:</strong> 20.00000
<strong>Giải thích:</strong> 
Cách chia tốt nhất là chia nums thành [9], [1, 2, 3], [9]. Đáp án là 9 + (1 + 2 + 3) / 3 + 9 = 20.
Ví dụ, ta cũng có thể chia nums thành [9, 1], [2], [3, 9].
Cách chia đó cho điểm số 5 + 2 + 6 = 13, thấp hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,6,7], k = 4
<strong>Đầu ra:</strong> 20.50000
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Memoized Search

<!-- thinking:start -->

> **Tư duy**
>
> Ta chia mảng thành nhiều nhất $k$ nhóm liên tiếp sao cho tổng giá trị trung bình của các nhóm là lớn nhất. Vị trí chia chưa biết; do $n,k\le 100$, có thể dùng memoization để tìm kiếm theo trạng thái “bắt đầu từ chỉ số $i$ khi còn $k$ nhóm”.
>
> Prefix sum giúp tính giá trị trung bình của một nhóm trong $O(1)$. Khi $k=1$, phần hậu tố còn lại là một nhóm; nếu không, ta thử mọi vị trí kết thúc của nhóm đầu tiên rồi gọi đệ quy.

<!-- thinking:end -->

Ta có thể tiền xử lý để tạo mảng prefix sum $s$, giúp tính nhanh tổng của các mảng con.

Tiếp theo, ta thiết kế hàm $\textit{dfs}(i, k)$ biểu diễn tổng giá trị trung bình lớn nhất khi chia phần mảng bắt đầu từ chỉ số $i$ thành nhiều nhất $k$ nhóm. Đáp án là $\textit{dfs}(0, k)$.

Hàm $\textit{dfs}(i, k)$ hoạt động như sau:

- Khi $i = n$, nghĩa là ta đã duyệt đến cuối mảng và trả về $0$.
- Khi $k = 1$, nghĩa là chỉ còn một nhóm, ta trả về giá trị trung bình từ chỉ số $i$ đến cuối mảng.
- Nếu không, ta xét mọi vị trí bắt đầu $j$ của nhóm tiếp theo trong khoảng $[i + 1, n)$, tính giá trị trung bình từ $i$ đến $j - 1$ là $\frac{s[j] - s[i]}{j - i}$, cộng với kết quả $\textit{dfs}(j, k - 1)$, rồi lấy giá trị lớn nhất trong các kết quả.

Độ phức tạp thời gian là $O(n^2 \times k)$ và độ phức tạp không gian là $O(n \times k)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestSumOfAverages(self, nums: List[int], k: int) -> float:
        @cache
        def dfs(i: int, k: int) -> float:
            if i == n:
                return 0
            if k == 1:
                return (s[n] - s[i]) / (n - i)
            ans = 0
            for j in range(i + 1, n):
                ans = max(ans, (s[j] - s[i]) / (j - i) + dfs(j, k - 1))
            return ans

        n = len(nums)
        s = list(accumulate(nums, initial=0))
        return dfs(0, k)
```

#### Java

```java
class Solution {
    private Double[][] f;
    private int[] s;
    private int n;

    public double largestSumOfAverages(int[] nums, int k) {
        n = nums.length;
        s = new int[n + 1];
        f = new Double[n][k + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        return dfs(0, k);
    }

    private double dfs(int i, int k) {
        if (i == n) {
            return 0;
        }
        if (k == 1) {
            return (s[n] - s[i]) * 1.0 / (n - i);
        }
        if (f[i][k] != null) {
            return f[i][k];
        }
        double ans = 0;
        for (int j = i + 1; j < n; ++j) {
            ans = Math.max(ans, (s[j] - s[i]) * 1.0 /(j - i) + dfs(j, k - 1));
        }
        return f[i][k] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double largestSumOfAverages(vector<int>& nums, int k) {
        int n = nums.size();
        int s[n + 1];
        double f[n][k + 1];
        memset(f, 0, sizeof(f));
        s[0] = 0;
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        auto dfs = [&](this auto&& dfs, int i, int k) -> double {
            if (i == n) {
                return 0;
            }
            if (k == 1) {
                return (s[n] - s[i]) * 1.0 / (n - i);
            }
            if (f[i][k] > 0) {
                return f[i][k];
            }
            double ans = 0;
            for (int j = i + 1; j < n; ++j) {
                ans = max(ans, (s[j] - s[i]) * 1.0 / (j - i) + dfs(j, k - 1));
            }
            return f[i][k] = ans;
        };
        return dfs(0, k);
    }
};
```

#### Go

```go
func largestSumOfAverages(nums []int, k int) float64 {
	n := len(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	f := make([][]float64, n)
	for i := range f {
		f[i] = make([]float64, k+1)
	}
	var dfs func(int, int) float64
	dfs = func(i, k int) float64 {
		if i == n {
			return 0
		}
		if f[i][k] > 0 {
			return f[i][k]
		}
		if k == 1 {
			return float64(s[n]-s[i]) / float64(n-i)
		}
		ans := 0.0
		for j := i + 1; j < n; j++ {
			ans = math.Max(ans, float64(s[j]-s[i])/float64(j-i)+dfs(j, k-1))
		}
		f[i][k] = ans
		return ans
	}
	return dfs(0, k)
}
```

#### TypeScript

```ts
function largestSumOfAverages(nums: number[], k: number): number {
    const n = nums.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; i++) {
        s[i + 1] = s[i] + nums[i];
    }
    const f: number[][] = Array.from({ length: n }, () => Array(k + 1).fill(0));
    const dfs = (i: number, k: number): number => {
        if (i === n) {
            return 0;
        }
        if (f[i][k] > 0) {
            return f[i][k];
        }
        if (k === 1) {
            return (s[n] - s[i]) / (n - i);
        }
        for (let j = i + 1; j < n; j++) {
            f[i][k] = Math.max(f[i][k], dfs(j, k - 1) + (s[j] - s[i]) / (j - i));
        }
        return f[i][k];
    };
    return dfs(0, k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Có thể lập bảng từ cùng công thức truy hồi để tránh overhead đệ quy. $f[i][j]$ là điểm số tốt nhất khi chia $i$ số đầu tiên thành $j$ nhóm, bằng cách xét vị trí cắt trước đó $h$.
>
> $f[i][1]$ là giá trị trung bình của prefix. Đáp án là $f[n][k]$, độ phức tạp vẫn là $O(n^2k)$.

<!-- thinking:end -->

Ta có thể chuyển lời giải tìm kiếm có memoization ở Lời giải 1 thành quy hoạch động.

Định nghĩa $f[i][j]$ là tổng giá trị trung bình lớn nhất khi chia $i$ phần tử đầu tiên của mảng $\textit{nums}$ thành nhiều nhất $j$ nhóm. Đáp án là $f[n][k]$.

Để tính $f[i][j]$, ta xét mọi vị trí kết thúc $h$ của nhóm trước đó, tính $f[h][j-1]$, cộng với $\frac{s[i] - s[h]}{i - h}$, rồi lấy giá trị lớn nhất trong các kết quả.

Độ phức tạp thời gian là $O(n^2 \times k)$ và độ phức tạp không gian là $O(n \times k)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestSumOfAverages(self, nums: List[int], k: int) -> float:
        n = len(nums)
        f = [[0] * (k + 1) for _ in range(n + 1)]
        s = list(accumulate(nums, initial=0))
        for i in range(1, n + 1):
            f[i][1] = s[i] / i
            for j in range(2, min(i + 1, k + 1)):
                for h in range(i):
                    f[i][j] = max(f[i][j], f[h][j - 1] + (s[i] - s[h]) / (i - h))
        return f[n][k]
```

#### Java

```java
class Solution {
    public double largestSumOfAverages(int[] nums, int k) {
        int n = nums.length;
        double[][] f = new double[n + 1][k + 1];
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        for (int i = 1; i <= n; ++i) {
            f[i][1] = s[i] * 1.0 / i;
            for (int j = 2; j <= Math.min(i, k); ++j) {
                for (int h = 0; h < i; ++h) {
                    f[i][j] = Math.max(f[i][j], f[h][j - 1] + (s[i] - s[h]) * 1.0 / (i - h));
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
    double largestSumOfAverages(vector<int>& nums, int k) {
        int n = nums.size();
        int s[n + 1];
        s[0] = 0;
        double f[n + 1][k + 1];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        for (int i = 1; i <= n; ++i) {
            f[i][1] = s[i] * 1.0 / i;
            for (int j = 2; j <= min(i, k); ++j) {
                for (int h = 0; h < i; ++h) {
                    f[i][j] = max(f[i][j], f[h][j - 1] + (s[i] - s[h]) * 1.0 / (i - h));
                }
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func largestSumOfAverages(nums []int, k int) float64 {
	n := len(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	f := make([][]float64, n+1)
	for i := range f {
		f[i] = make([]float64, k+1)
	}
	for i := 1; i <= n; i++ {
		f[i][1] = float64(s[i]) / float64(i)
		for j := 2; j <= min(i, k); j++ {
			for h := 0; h < i; h++ {
				f[i][j] = max(f[i][j], f[h][j-1]+float64(s[i]-s[h])/float64(i-h))
			}
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function largestSumOfAverages(nums: number[], k: number): number {
    const n = nums.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; i++) {
        s[i + 1] = s[i] + nums[i];
    }
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(k + 1).fill(0));
    for (let i = 1; i <= n; ++i) {
        f[i][1] = s[i] / i;
        for (let j = 2; j <= Math.min(i, k); ++j) {
            for (let h = 0; h < i; ++h) {
                f[i][j] = Math.max(f[i][j], f[h][j - 1] + (s[i] - s[h]) / (i - h));
            }
        }
    }
    return f[n][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
