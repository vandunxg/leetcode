---
comments: true
difficulty: Medium
rating: 1904
source: Weekly Contest 394 Q3
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3122. Minimum Number of Operations to Satisfy Conditions](https://leetcode.com/problems/minimum-number-of-operations-to-satisfy-conditions)

[中文文档](/solution/3100-3199/3122.Minimum%20Number%20of%20Operations%20to%20Satisfy%20Conditions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một ma trận 2D <code>grid</code> có kích thước <code>m x n</code>. Trong một <strong>thao tác</strong>, bạn có thể thay đổi giá trị của <strong>bất kỳ</strong> ô nào thành một số không âm <strong>bất kỳ</strong>. Bạn cần thực hiện một số <strong>thao tác</strong> sao cho mỗi ô <code>grid[i][j]</code> thỏa mãn:</p>

<ul>
	<li>Bằng ô bên dưới nó, tức là <code>grid[i][j] == grid[i + 1][j]</code> (nếu ô đó tồn tại).</li>
	<li>Khác ô bên phải nó, tức là <code>grid[i][j] != grid[i][j + 1]</code> (nếu ô đó tồn tại).</li>
</ul>

<p>Trả về số thao tác <strong>ít nhất</strong> cần thực hiện.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,0,2],[1,0,2]]</span></p>

<p><strong>Đầu ra:</strong> 0</p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3122.Minimum%20Number%20of%20Operations%20to%20Satisfy%20Conditions/images/examplechanged.png" style="width: 254px; height: 186px;padding: 10px; background: #fff; border-radius: .5rem;" /></strong></p>

<p>Tất cả các ô trong ma trận đã thỏa mãn các tính chất.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,1,1],[0,0,0]]</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3122.Minimum%20Number%20of%20Operations%20to%20Satisfy%20Conditions/images/example21.png" style="width: 254px; height: 186px;padding: 10px; background: #fff; border-radius: .5rem;" /></strong></p>

<p>Ma trận trở thành <code>[[1,0,1],[1,0,1]]</code> và thỏa mãn các tính chất, sau 3 thao tác sau:</p>

<ul>
	<li>Đổi <code>grid[1][0]</code> thành 1.</li>
	<li>Đổi <code>grid[0][1]</code> thành 0.</li>
	<li>Đổi <code>grid[1][2]</code> thành 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1],[2],[3]]</span></p>

<p><strong>Đầu ra:</strong> 2</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3122.Minimum%20Number%20of%20Operations%20to%20Satisfy%20Conditions/images/changed.png" style="width: 86px; height: 277px;padding: 10px; background: #fff; border-radius: .5rem;" /></p>

<p>Đây là một cột duy nhất. Ta có thể đổi giá trị của mỗi ô thành 1 với 2 thao tác.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, m &lt;= 1000</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cột phải trở thành một chữ số duy nhất và hai cột kề nhau phải khác nhau. Nếu thử lần lượt một chữ số cho mỗi cột, số trạng thái sẽ là $10^n$.
>
> Chỉ chữ số của cột trước đó là quan trọng, và các giá trị chỉ nằm trong $0..9$. Sau khi đếm cột hiện tại, chi phí đổi cột đó thành $j$ là $m-cnt[j]$.
>
> Gọi $f[i][j]$ là chi phí nhỏ nhất của $i$ cột đầu tiên khi cột $i$ có giá trị $j$. Khi đó, ta lấy $\min_{k\neq j} f[i-1][k]$ cộng với chi phí của cột hiện tại. Đáp án là $\min f[n-1]$.

<!-- thinking:end -->

Ta nhận thấy các giá trị trong các ô của ma trận chỉ có 10 khả năng. Bài toán yêu cầu tìm số thao tác ít nhất để mỗi cột có cùng một số, đồng thời các số trong hai cột kề nhau phải khác nhau. Do đó, ta chỉ cần xét trường hợp đổi số thành một trong các số từ 0 đến 9.

Ta định nghĩa trạng thái $f[i][j]$ là số thao tác ít nhất để các số trong các cột đầu tiên $[0,..i]$ được xử lý, trong đó số ở cột thứ $i$ là $j$. Khi đó, ta có công thức chuyển trạng thái:

$$
f[i][j] = \min_{k \neq j} (f[i-1][k] + m - \textit{cnt}[j])
$$

Trong đó, $\textit{cnt}[j]$ là số ô trong cột thứ $i$ có giá trị bằng $j$.

Cuối cùng, ta chỉ cần tìm giá trị nhỏ nhất của $f[n-1][j]$.

Độ phức tạp thời gian là $O(n \times (m + C^2))$, còn độ phức tạp không gian là $O(n \times C)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của ma trận; còn $C$ là số loại giá trị, ở đây $C = 10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        f = [[inf] * 10 for _ in range(n)]
        for i in range(n):
            cnt = [0] * 10
            for j in range(m):
                cnt[grid[j][i]] += 1
            if i == 0:
                for j in range(10):
                    f[i][j] = m - cnt[j]
            else:
                for j in range(10):
                    for k in range(10):
                        if k != j:
                            f[i][j] = min(f[i][j], f[i - 1][k] + m - cnt[j])
        return min(f[-1])
```

#### Java

```java
class Solution {
    public int minimumOperations(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] f = new int[n][10];
        final int inf = 1 << 29;
        for (var g : f) {
            Arrays.fill(g, inf);
        }
        for (int i = 0; i < n; ++i) {
            int[] cnt = new int[10];
            for (int j = 0; j < m; ++j) {
                ++cnt[grid[j][i]];
            }
            if (i == 0) {
                for (int j = 0; j < 10; ++j) {
                    f[i][j] = m - cnt[j];
                }
            } else {
                for (int j = 0; j < 10; ++j) {
                    for (int k = 0; k < 10; ++k) {
                        if (k != j) {
                            f[i][j] = Math.min(f[i][j], f[i - 1][k] + m - cnt[j]);
                        }
                    }
                }
            }
        }
        int ans = inf;
        for (int j = 0; j < 10; ++j) {
            ans = Math.min(ans, f[n - 1][j]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int f[n][10];
        memset(f, 0x3f, sizeof(f));
        for (int i = 0; i < n; ++i) {
            int cnt[10]{};
            for (int j = 0; j < m; ++j) {
                ++cnt[grid[j][i]];
            }
            if (i == 0) {
                for (int j = 0; j < 10; ++j) {
                    f[i][j] = m - cnt[j];
                }
            } else {
                for (int j = 0; j < 10; ++j) {
                    for (int k = 0; k < 10; ++k) {
                        if (k != j) {
                            f[i][j] = min(f[i][j], f[i - 1][k] + m - cnt[j]);
                        }
                    }
                }
            }
        }
        return *min_element(f[n - 1], f[n - 1] + 10);
    }
};
```

#### Go

```go
func minimumOperations(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, 10)
		for j := range f[i] {
			f[i][j] = 1 << 29
		}
	}
	for i := 0; i < n; i++ {
		cnt := [10]int{}
		for j := 0; j < m; j++ {
			cnt[grid[j][i]]++
		}
		if i == 0 {
			for j := 0; j < 10; j++ {
				f[i][j] = m - cnt[j]
			}
		} else {
			for j := 0; j < 10; j++ {
				for k := 0; k < 10; k++ {
					if j != k {
						f[i][j] = min(f[i][j], f[i-1][k]+m-cnt[j])
					}
				}
			}
		}
	}
	return slices.Min(f[n-1])
}
```

#### TypeScript

```ts
function minimumOperations(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const f: number[][] = Array.from({ length: n }, () =>
        Array.from({ length: 10 }, () => Infinity),
    );
    for (let i = 0; i < n; ++i) {
        const cnt: number[] = Array(10).fill(0);
        for (let j = 0; j < m; ++j) {
            cnt[grid[j][i]]++;
        }
        if (i === 0) {
            for (let j = 0; j < 10; ++j) {
                f[i][j] = m - cnt[j];
            }
        } else {
            for (let j = 0; j < 10; ++j) {
                for (let k = 0; k < 10; ++k) {
                    if (j !== k) {
                        f[i][j] = Math.min(f[i][j], f[i - 1][k] + m - cnt[j]);
                    }
                }
            }
        }
    }
    return Math.min(...f[n - 1]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
