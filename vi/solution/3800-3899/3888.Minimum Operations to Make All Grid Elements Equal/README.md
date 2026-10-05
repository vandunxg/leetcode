---
comments: true
difficulty: Hard
tags:
    - Array
    - Math
    - Matrix
    - Prefix Sum
---

<!-- problem:start -->

# [3888. Minimum Operations to Make All Grid Elements Equal 🔒](https://leetcode.com/problems/minimum-operations-to-make-all-grid-elements-equal)

[中文文档](/solution/3800-3899/3888.Minimum%20Operations%20to%20Make%20All%20Grid%20Elements%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>grid</code> có kích thước <code>m &times; n</code> và một số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể:</p>

<ul>
	<li>Chọn một <strong>ma trận con</strong> <code>k x k</code> bất kỳ của <code>grid</code>, rồi</li>
	<li>Tăng <strong>tất cả các phần tử</strong> bên trong <strong>ma trận con</strong> đó thêm 1.</li>
</ul>

<p>Hãy trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để tất cả các phần tử trong grid <strong>bằng nhau</strong>. Nếu không thể, hãy trả về -1.</p>
Một ma trận con <code>(x1, y1, x2, y2)</code> là một ma trận được tạo bằng cách chọn tất cả các ô <code>matrix[x][y]</code> thỏa mãn <code>x1 &lt;= x &lt;= x2</code> và <code>y1 &lt;= y &lt;= y2</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[3,3,5],[3,3,5]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p data-end="266" data-start="150">Chọn ma trận con <code>2 x 2</code> bên trái (gồm hai cột đầu tiên) và thực hiện thao tác hai lần.</p>

<ul>
	<li>Sau 1 thao tác: <code>[[4, 4, 5], [4, 4, 5]]</code></li>
	<li>Sau 2 thao tác: <code>[[5, 5, 5], [5, 5, 5]]</code></li>
</ul>

<p>Tất cả các phần tử đều trở thành 5. Do đó, số thao tác nhỏ nhất là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2],[2,3]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì <code>k = 1</code>, mỗi thao tác chỉ tăng một ô <code>grid[i][j]</code> thêm 1. Để tất cả các phần tử bằng nhau, giá trị cuối cùng phải là 3.</p>

<ul>
	<li>Tăng <code>grid[0][0] = 1</code> lên 3, cần 2 thao tác.</li>
	<li>Tăng <code>grid[0][1] = 2</code> lên 3, cần 1 thao tác.</li>
	<li>Tăng <code>grid[1][0] = 2</code> lên 3, cần 1 thao tác.</li>
</ul>

<p>Vì vậy, số thao tác nhỏ nhất là <code>2 + 1 + 1 + 0 = 4</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m == grid.length &lt;= 1000</code></li>
	<li><code>1 &lt;= n == grid[i].length &lt;= 1000</code></li>
	<li><code>-10<sup>5</sup> &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= min(m, n)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu 2D + Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác tăng một ma trận con $k \times k$ thêm $1$. Ta muốn mọi ô bằng nhau với số thao tác ít nhất. Vì các thao tác chỉ làm tăng giá trị, giá trị đích $T$ không nhỏ hơn giá trị lớn nhất hiện tại.
>
> Duyệt từ góc trên bên trái: các thao tác sau đó có góc trên bên trái nằm xa hơn về bên phải hoặc phía dưới không thể phủ lên $(i,j)$, nên phần thiếu so với $T$ phải được bù ngay tại $(i,j)$.
>
> Mảng hiệu 2D ghi nhận một lần tăng trên ma trận con $k \times k$ trong $O(1)$, còn tổng tiền tố khôi phục lượng tăng đang có hiệu lực. Nếu vượt quá $T$ hoặc thao tác tràn ra ngoài grid thì thất bại.
>
> Nếu cả $T=\max$ và $T=\max+1$ đều thất bại, không thể làm phẳng grid.

<!-- thinking:end -->

Vì thao tác chỉ có thể tăng giá trị của các phần tử, tất cả các phần tử trong grid cuối cùng phải bằng một giá trị đích $T$, với $T \ge \max(\textit{grid})$.

Bắt đầu duyệt grid từ góc trên bên trái $(0, 0)$. Với mỗi vị trí $(i, j)$, nếu giá trị hiện tại nhỏ hơn $T$, do các thao tác tiếp theo (có vị trí làm góc trên bên trái nằm xa hơn về bên phải hoặc phía dưới) không thể phủ lên $(i, j)$, ta cần thực hiện $T - \text{current\_val}$ thao tác tại vị trí hiện tại, mỗi thao tác sử dụng $(i, j)$ làm góc trên bên trái của một lần tăng ma trận con $k \times k$.

Nếu mỗi thao tác đều duyệt qua vùng $k \times k$, độ phức tạp sẽ lên tới $O(m \cdot n \cdot k^2)$. Ta có thể dùng một mảng hiệu 2D $\textit{diff}$ để ghi nhận các thao tác. Bằng cách duy trì tổng tiền tố 2D của $\textit{diff}$ trong thời gian thực, ta có thể lấy lượng tăng tích lũy tại vị trí hiện tại trong $O(1)$, đồng thời cập nhật ảnh hưởng trong tương lai của một vùng $k \times k$ cũng trong $O(1)$.

Trong hầu hết trường hợp, $T = \max(\textit{grid})$ là đủ. Tuy nhiên, trong một số trường hợp các vùng $k \times k$ chồng lấn lên nhau, chọn $T$ nhỏ hơn có thể khiến các vị trí ở giữa bị tăng thụ động vượt quá $T$. Theo tính nhất quán toán học, nếu cả $T = \max(\textit{grid})$ và $T = \max(\textit{grid}) + 1$ đều không khả thi, thì không thể làm phẳng grid bằng các thao tác trên vùng $k \times k$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, grid: list[list[int]], k: int) -> int:
        m, n = len(grid), len(grid[0])
        mx = max(max(row) for row in grid)

        def check(target: int) -> int:
            diff = [[0] * (n + 2) for _ in range(m + 2)]
            total_ops = 0

            for i, row in enumerate(grid, 1):
                for j, val in enumerate(row, 1):
                    diff[i][j] += diff[i - 1][j] + diff[i][j - 1] - diff[i - 1][j - 1]

                    cur_val = val + diff[i][j]

                    if cur_val > target:
                        return -1

                    if cur_val < target:
                        if i + k - 1 > m or j + k - 1 > n:
                            return -1

                        needed = target - cur_val
                        total_ops += needed
                        diff[i][j] += needed
                        diff[i + k][j] -= needed
                        diff[i][j + k] -= needed
                        diff[i + k][j + k] += needed
            return total_ops

        for t in range(mx, mx + 2):
            res = check(t)
            if res != -1:
                return res

        return -1
```

#### Java

```java
class Solution {
    int[][] grid;
    int m, n, k;

    public long minOperations(int[][] grid, int k) {
        this.grid = grid;
        this.k = k;
        this.m = grid.length;
        this.n = grid[0].length;

        int mx = Integer.MIN_VALUE;
        for (int[] row : grid) {
            for (int v : row) {
                mx = Math.max(mx, v);
            }
        }

        for (int t = mx; t <= mx + 1; t++) {
            long res = check(t);
            if (res != -1) {
                return res;
            }
        }
        return -1;
    }

    private long check(int target) {
        long[][] diff = new long[m + 2][n + 2];
        long totalOps = 0;

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                diff[i][j] += diff[i - 1][j] + diff[i][j - 1] - diff[i - 1][j - 1];
                long cur = grid[i - 1][j - 1] + diff[i][j];

                if (cur > target) {
                    return -1;
                }

                if (cur < target) {
                    if (i + k - 1 > m || j + k - 1 > n) {
                        return -1;
                    }

                    long need = target - cur;
                    totalOps += need;

                    diff[i][j] += need;
                    diff[i + k][j] -= need;
                    diff[i][j + k] -= need;
                    diff[i + k][j + k] += need;
                }
            }
        }
        return totalOps;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minOperations(vector<vector<int>>& grid, int k) {
        int m = grid.size();
        int n = grid[0].size();
        int mx = grid[0][0];
        for (auto& row : grid) {
            for (int val : row) {
                mx = max(mx, val);
            }
        }

        auto check = [&](int target) -> long long {
            vector<vector<long long>> diff(m + 2, vector<long long>(n + 2, 0));
            long long total_ops = 0;

            for (int i = 1; i <= m; ++i) {
                for (int j = 1; j <= n; ++j) {
                    diff[i][j] += diff[i - 1][j] + diff[i][j - 1] - diff[i - 1][j - 1];
                    long long cur_val = grid[i - 1][j - 1] + diff[i][j];

                    if (cur_val > target) {
                        return -1;
                    }

                    if (cur_val < target) {
                        if (i + k - 1 > m || j + k - 1 > n) {
                            return -1;
                        }

                        long long needed = target - cur_val;
                        total_ops += needed;
                        diff[i][j] += needed;
                        diff[i + k][j] -= needed;
                        diff[i][j + k] -= needed;
                        diff[i + k][j + k] += needed;
                    }
                }
            }

            return total_ops;
        };

        for (int t = mx; t <= mx + 1; ++t) {
            long long res = check(t);
            if (res != -1) {
                return res;
            }
        }

        return -1;
    }
};
```

#### Go

```go
func minOperations(grid [][]int, k int) int64 {
	m, n := len(grid), len(grid[0])
	maxVal := grid[0][0]
	for _, row := range grid {
		maxVal = max(maxVal, slices.Max(row))
	}

	check := func(target int) int64 {
		diff := make([][]int64, m+2)
		for i := range diff {
			diff[i] = make([]int64, n+2)
		}
		var totalOps int64

		for i := 1; i <= m; i++ {
			for j := 1; j <= n; j++ {
				diff[i][j] += diff[i-1][j] + diff[i][j-1] - diff[i-1][j-1]
				curVal := int64(grid[i-1][j-1]) + diff[i][j]

				if curVal > int64(target) {
					return -1
				}

				if curVal < int64(target) {
					if i+k-1 > m || j+k-1 > n {
						return -1
					}
					needed := int64(target) - curVal
					totalOps += needed
					diff[i][j] += needed
					diff[i+k][j] -= needed
					diff[i][j+k] -= needed
					diff[i+k][j+k] += needed
				}
			}
		}
		return totalOps
	}

	for t := maxVal; t <= maxVal+1; t++ {
		if res := check(t); res != -1 {
			return res
		}
	}

	return -1
}
```

#### TypeScript

```ts
function minOperations(grid: number[][], k: number): number {
    const m = grid.length;
    const n = grid[0].length;
    let maxVal = grid[0][0];

    for (const row of grid) {
        for (const val of row) {
            maxVal = Math.max(maxVal, val);
        }
    }

    const check = (target: number): number => {
        const diff: number[][] = Array.from({ length: m + 2 }, () => Array(n + 2).fill(0));
        let totalOps = 0;

        for (let i = 1; i <= m; i++) {
            for (let j = 1; j <= n; j++) {
                diff[i][j] += diff[i - 1][j] + diff[i][j - 1] - diff[i - 1][j - 1];
                const curVal = grid[i - 1][j - 1] + diff[i][j];

                if (curVal > target) return -1;

                if (curVal < target) {
                    if (i + k - 1 > m || j + k - 1 > n) return -1;

                    const needed = target - curVal;
                    totalOps += needed;
                    diff[i][j] += needed;
                    diff[i + k][j] -= needed;
                    diff[i][j + k] -= needed;
                    diff[i + k][j + k] += needed;
                }
            }
        }

        return totalOps;
    };

    for (let t = maxVal; t <= maxVal + 1; t++) {
        const res = check(t);
        if (res !== -1) return res;
    }

    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
