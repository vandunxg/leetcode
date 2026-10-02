---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - 0-1 Knapsack
---

<!-- problem:start -->

# [879. Profitable Schemes](https://leetcode.com/problems/profitable-schemes)

[中文文档](/solution/0800-0899/0879.Profitable%20Schemes/README.md)

## Mô tả

<!-- description:start -->

<p>Có một nhóm gồm <code>n</code> thành viên và danh sách các phi vụ mà họ có thể thực hiện. Phi vụ thứ <code>i<sup>th</sup></code> tạo ra lợi nhuận <code>profit[i]</code> và cần <code>group[i]</code> thành viên tham gia. Nếu một thành viên tham gia một phi vụ, người đó không thể tham gia phi vụ khác.</p>

<p>Ta gọi một tập con bất kỳ của các phi vụ này là <strong>phương án có lợi nhuận</strong> nếu tạo ra lợi nhuận ít nhất <code>minProfit</code> và tổng số thành viên tham gia các phi vụ trong tập con đó không vượt quá <code>n</code>.</p>

<p>Hãy trả về số phương án có thể chọn. Vì đáp án có thể rất lớn, hãy <strong>trả về phần dư khi chia cho</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 5, minProfit = 3, group = [2,2], profit = [2,3]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Để đạt lợi nhuận ít nhất 3, nhóm có thể thực hiện cả phi vụ 0 và 1, hoặc chỉ thực hiện phi vụ 1.
Có tổng cộng 2 phương án.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 10, minProfit = 5, group = [2,3,5], profit = [6,7,8]
<strong>Output:</strong> 7
<strong>Giải thích:</strong> Để đạt lợi nhuận ít nhất 5, nhóm có thể thực hiện bất kỳ phi vụ nào, miễn là có ít nhất một phi vụ.
Có 7 phương án khả dĩ: (0), (1), (2), (0,1), (0,2), (1,2) và (0,1,2).</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= minProfit &lt;= 100</code></li>
	<li><code>1 &lt;= group.length &lt;= 100</code></li>
	<li><code>1 &lt;= group[i] &lt;= 100</code></li>
	<li><code>profit.length == group.length</code></li>
	<li><code>0 &lt;= profit[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi phi vụ, ta có thể chọn hoặc bỏ qua, đồng thời không vượt quá giới hạn thành viên và cần đạt ngưỡng lợi nhuận. Trạng thái gồm “phi vụ $i$, đã dùng $j$ thành viên, lợi nhuận được chặn ở mức $\textit{minProfit}$”.
>
> $dfs(i,j,k)$ xét hai lựa chọn: bỏ qua phi vụ hoặc thực hiện nếu còn đủ thành viên. Chặn lợi nhuận giúp gộp các trạng thái đã đạt ngưỡng. Cuối cùng, $k$ phải bằng $\textit{minProfit}$.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(i, j, k)$, trong đó ta bắt đầu xét phi vụ thứ $i$, đã chọn $j$ thành viên và lợi nhuận hiện tại là $k$. Số phương án cần tìm là $dfs(0, 0, 0)$.

Hàm $dfs(i, j, k)$ hoạt động như sau:

- Nếu đã xét hết các phi vụ, tức $i = n$, thì nếu $k \geq minProfit$, có $1$ phương án; ngược lại, có $0$ phương án;
- Nếu $i < n$, ta có thể bỏ qua phi vụ thứ $i$, khi đó số phương án là $dfs(i + 1, j, k)$. Nếu $j + group[i] \leq n$, ta cũng có thể chọn phi vụ thứ $i$, khi đó số phương án là $dfs(i + 1, j + group[i], \min(k + profit[i], minProfit))$. Ta chặn lợi nhuận tối đa ở mức $minProfit$ vì phần lợi nhuận vượt quá ngưỡng này không ảnh hưởng đến đáp án.

Cuối cùng, trả về $dfs(0, 0, 0)$.

Để tránh tính toán lặp lại, ta dùng memoization. Mảng ba chiều $f$ lưu kết quả của các trạng thái $dfs(i, j, k)$. Sau khi tính $dfs(i, j, k)$, ta lưu kết quả vào $f[i][j][k]$. Khi gọi lại trạng thái này, nếu $f[i][j][k]$ đã có giá trị thì trả về ngay.

Độ phức tạp thời gian là $O(m \times n \times minProfit)$ và độ phức tạp không gian là $O(m \times n \times minProfit)$. Trong đó, $m$ và $n$ lần lượt là số phi vụ và số thành viên, còn $minProfit$ là mức lợi nhuận tối thiểu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def profitableSchemes(
        self, n: int, minProfit: int, group: List[int], profit: List[int]
    ) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if i >= len(group):
                return 1 if k == minProfit else 0
            ans = dfs(i + 1, j, k)
            if j + group[i] <= n:
                ans += dfs(i + 1, j + group[i], min(k + profit[i], minProfit))
            return ans % (10**9 + 7)

        return dfs(0, 0, 0)
```

#### Java

```java
class Solution {
    private Integer[][][] f;
    private int m;
    private int n;
    private int minProfit;
    private int[] group;
    private int[] profit;
    private final int mod = (int) 1e9 + 7;

    public int profitableSchemes(int n, int minProfit, int[] group, int[] profit) {
        m = group.length;
        this.n = n;
        f = new Integer[m][n + 1][minProfit + 1];
        this.minProfit = minProfit;
        this.group = group;
        this.profit = profit;
        return dfs(0, 0, 0);
    }

    private int dfs(int i, int j, int k) {
        if (i >= m) {
            return k == minProfit ? 1 : 0;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        int ans = dfs(i + 1, j, k);
        if (j + group[i] <= n) {
            ans += dfs(i + 1, j + group[i], Math.min(k + profit[i], minProfit));
        }
        ans %= mod;
        return f[i][j][k] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int profitableSchemes(int n, int minProfit, vector<int>& group, vector<int>& profit) {
        int m = group.size();
        int f[m][n + 1][minProfit + 1];
        memset(f, -1, sizeof(f));
        const int mod = 1e9 + 7;
        function<int(int, int, int)> dfs = [&](int i, int j, int k) -> int {
            if (i >= m) {
                return k == minProfit ? 1 : 0;
            }
            if (f[i][j][k] != -1) {
                return f[i][j][k];
            }
            int ans = dfs(i + 1, j, k);
            if (j + group[i] <= n) {
                ans += dfs(i + 1, j + group[i], min(k + profit[i], minProfit));
            }
            ans %= mod;
            return f[i][j][k] = ans;
        };
        return dfs(0, 0, 0);
    }
};
```

#### Go

```go
func profitableSchemes(n int, minProfit int, group []int, profit []int) int {
	m := len(group)
	f := make([][][]int, m)
	for i := range f {
		f[i] = make([][]int, n+1)
		for j := range f[i] {
			f[i][j] = make([]int, minProfit+1)
			for k := range f[i][j] {
				f[i][j][k] = -1
			}
		}
	}
	const mod = 1e9 + 7
	var dfs func(i, j, k int) int
	dfs = func(i, j, k int) int {
		if i >= m {
			if k >= minProfit {
				return 1
			}
			return 0
		}
		if f[i][j][k] != -1 {
			return f[i][j][k]
		}
		ans := dfs(i+1, j, k)
		if j+group[i] <= n {
			ans += dfs(i+1, j+group[i], min(k+profit[i], minProfit))
		}
		ans %= mod
		f[i][j][k] = ans
		return ans
	}
	return dfs(0, 0, 0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta điền các trạng thái ba chiều theo chỉ số phi vụ. $f[i][j][k]$ là số phương án dùng $j$ thành viên trong $i$ phi vụ đầu tiên và đạt lợi nhuận ít nhất $k$.
>
> Bỏ qua phi vụ thì lấy trạng thái của lớp trước; chọn phi vụ thì cộng $f[i-1][j-x][\max(0,k-p)]$. Phương án rỗng đóng góp $1$ ở mức lợi nhuận $0$. Đáp án là $f[m][n][\textit{minProfit}]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j][k]$ là số phương án đạt lợi nhuận ít nhất $k$, xét $i$ phi vụ và dùng $j$ thành viên. Ban đầu, $f[0][j][0] = 1$, nghĩa là khi chưa thực hiện phi vụ nào, chỉ có một phương án tạo ra lợi nhuận $0$.

Với phi vụ thứ $i$, ta có thể chọn thực hiện hoặc bỏ qua. Nếu bỏ qua, $f[i][j][k] = f[i - 1][j][k]$; nếu thực hiện, $f[i][j][k] = f[i - 1][j - group[i - 1]][max(0, k - profit[i - 1])]$. Ta duyệt các giá trị $j$ và $k$, rồi cộng số phương án tương ứng.

Đáp án cuối cùng là $f[m][n][minProfit]$.

Độ phức tạp thời gian là $O(m \times n \times minProfit)$ và độ phức tạp không gian là $O(m \times n \times minProfit)$. Trong đó, $m$ và $n$ lần lượt là số phi vụ và số thành viên, còn $minProfit$ là lợi nhuận tối thiểu cần đạt.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def profitableSchemes(
        self, n: int, minProfit: int, group: List[int], profit: List[int]
    ) -> int:
        mod = 10**9 + 7
        m = len(group)
        f = [[[0] * (minProfit + 1) for _ in range(n + 1)] for _ in range(m + 1)]
        for j in range(n + 1):
            f[0][j][0] = 1
        for i, (x, p) in enumerate(zip(group, profit), 1):
            for j in range(n + 1):
                for k in range(minProfit + 1):
                    f[i][j][k] = f[i - 1][j][k]
                    if j >= x:
                        f[i][j][k] = (f[i][j][k] + f[i - 1][j - x][max(0, k - p)]) % mod
        return f[m][n][minProfit]
```

#### Java

```java
class Solution {
    public int profitableSchemes(int n, int minProfit, int[] group, int[] profit) {
        final int mod = (int) 1e9 + 7;
        int m = group.length;
        int[][][] f = new int[m + 1][n + 1][minProfit + 1];
        for (int j = 0; j <= n; ++j) {
            f[0][j][0] = 1;
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                for (int k = 0; k <= minProfit; ++k) {
                    f[i][j][k] = f[i - 1][j][k];
                    if (j >= group[i - 1]) {
                        f[i][j][k]
                            = (f[i][j][k]
                                  + f[i - 1][j - group[i - 1]][Math.max(0, k - profit[i - 1])])
                            % mod;
                    }
                }
            }
        }
        return f[m][n][minProfit];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int profitableSchemes(int n, int minProfit, vector<int>& group, vector<int>& profit) {
        int m = group.size();
        int f[m + 1][n + 1][minProfit + 1];
        memset(f, 0, sizeof(f));
        for (int j = 0; j <= n; ++j) {
            f[0][j][0] = 1;
        }
        const int mod = 1e9 + 7;
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                for (int k = 0; k <= minProfit; ++k) {
                    f[i][j][k] = f[i - 1][j][k];
                    if (j >= group[i - 1]) {
                        f[i][j][k] = (f[i][j][k] + f[i - 1][j - group[i - 1]][max(0, k - profit[i - 1])]) % mod;
                    }
                }
            }
        }
        return f[m][n][minProfit];
    }
};
```

#### Go

```go
func profitableSchemes(n int, minProfit int, group []int, profit []int) int {
	m := len(group)
	f := make([][][]int, m+1)
	for i := range f {
		f[i] = make([][]int, n+1)
		for j := range f[i] {
			f[i][j] = make([]int, minProfit+1)
		}
	}
	for j := 0; j <= n; j++ {
		f[0][j][0] = 1
	}
	const mod = 1e9 + 7
	for i := 1; i <= m; i++ {
		for j := 0; j <= n; j++ {
			for k := 0; k <= minProfit; k++ {
				f[i][j][k] = f[i-1][j][k]
				if j >= group[i-1] {
					f[i][j][k] += f[i-1][j-group[i-1]][max(0, k-profit[i-1])]
					f[i][j][k] %= mod
				}
			}
		}
	}
	return f[m][n][minProfit]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
