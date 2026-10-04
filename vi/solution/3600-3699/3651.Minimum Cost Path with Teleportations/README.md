---
comments: true
difficulty: Hard
rating: 2411
source: Biweekly Contest 163 Q4
tags:
    - Array
    - Dynamic Programming
    - Matrix
---

<!-- problem:start -->

# [3651. Minimum Cost Path with Teleportations](https://leetcode.com/problems/minimum-cost-path-with-teleportations)

[中文文档](/solution/3600-3699/3651.Minimum%20Cost%20Path%20with%20Teleportations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>m x n</code> <code>grid</code> và một số nguyên <code>k</code>. Bạn bắt đầu tại ô trên cùng bên trái <code>(0, 0)</code> và mục tiêu là đi đến ô dưới cùng bên phải <code>(m - 1, n - 1)</code>.</p>

<p>Có hai loại bước di chuyển:</p>

<ul>
	<li>
	<p><strong>Di chuyển thông thường</strong>: Bạn có thể đi sang phải hoặc xuống dưới từ ô hiện tại <code>(i, j)</code>, tức là đi đến <code>(i, j + 1)</code> (sang phải) hoặc <code>(i + 1, j)</code> (xuống dưới). Chi phí là giá trị của ô đích.</p>
	</li>
	<li>
	<p><strong>Dịch chuyển tức thời</strong>: Bạn có thể dịch chuyển từ bất kỳ ô nào <code>(i, j)</code> đến bất kỳ ô nào <code>(x, y)</code> sao cho <code>grid[x][y] &lt;= grid[i][j]</code>; chi phí của bước này là 0. Bạn có thể dịch chuyển tức thời nhiều nhất <code>k</code> lần.</p>
	</li>
</ul>

<p>Hãy trả về tổng chi phí <strong>nhỏ nhất</strong> để đi từ ô <code>(0, 0)</code> đến ô <code>(m - 1, n - 1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,3,3],[2,5,4],[4,3,5]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, chúng ta ở tại (0, 0) và chi phí là 0.</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Vị trí hiện tại</th>
			<th style="border: 1px solid black;">Bước di chuyển</th>
			<th style="border: 1px solid black;">Vị trí mới</th>
			<th style="border: 1px solid black;">Tổng chi phí</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>(0, 0)</code></td>
			<td style="border: 1px solid black;">Đi xuống</td>
			<td style="border: 1px solid black;"><code>(1, 0)</code></td>
			<td style="border: 1px solid black;"><code>0 + 2 = 2</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>(1, 0)</code></td>
			<td style="border: 1px solid black;">Đi sang phải</td>
			<td style="border: 1px solid black;"><code>(1, 1)</code></td>
			<td style="border: 1px solid black;"><code>2 + 5 = 7</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>(1, 1)</code></td>
			<td style="border: 1px solid black;">Dịch chuyển tức thời đến <code>(2, 2)</code></td>
			<td style="border: 1px solid black;"><code>(2, 2)</code></td>
			<td style="border: 1px solid black;"><code>7 + 0 = 7</code></td>
		</tr>
	</tbody>
</table>

<p>Chi phí nhỏ nhất để đến ô dưới cùng bên phải là 7.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">grid = [[1,2],[2,3],[3,4]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích: </strong></p>

<p>Ban đầu, chúng ta ở tại (0, 0) và chi phí là 0.</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Vị trí hiện tại</th>
			<th style="border: 1px solid black;">Bước di chuyển</th>
			<th style="border: 1px solid black;">Vị trí mới</th>
			<th style="border: 1px solid black;">Tổng chi phí</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>(0, 0)</code></td>
			<td style="border: 1px solid black;">Đi xuống</td>
			<td style="border: 1px solid black;"><code>(1, 0)</code></td>
			<td style="border: 1px solid black;"><code>0 + 2 = 2</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>(1, 0)</code></td>
			<td style="border: 1px solid black;">Đi sang phải</td>
			<td style="border: 1px solid black;"><code>(1, 1)</code></td>
			<td style="border: 1px solid black;"><code>2 + 3 = 5</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>(1, 1)</code></td>
			<td style="border: 1px solid black;">Đi xuống</td>
			<td style="border: 1px solid black;"><code>(2, 1)</code></td>
			<td style="border: 1px solid black;"><code>5 + 4 = 9</code></td>
		</tr>
	</tbody>
</table>

<p>Chi phí nhỏ nhất để đến ô dưới cùng bên phải là 9.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= m, n &lt;= 80</code></li>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ngoài việc phải trả chi phí của một ô khi đi sang phải hoặc xuống dưới, chúng ta có thể dịch chuyển tức thời từ một giá trị lớn đến một giá trị nhỏ hơn nhiều nhất $k$ lần. Số lần dịch chuyển tức thời còn lại là một phần của trạng thái.
>
> $f[t][i][j]$ là chi phí nhỏ nhất để đi đến $(i,j)$ với $t$ lần dịch chuyển tức thời. Lớp $t=0$ chỉ sử dụng các bước di chuyển trên lưới. Vì dịch chuyển tức thời yêu cầu giá trị không tăng, các ô được duyệt từ lớn đến nhỏ và chi phí tốt nhất của lớp trước trên những ô đó được ghi vào lớp hiện tại.
>
> Sau khi cập nhật, một lượt duyệt sang phải/xuống dưới thứ hai cho phép tiếp tục đường đi. Đáp án là chi phí nhỏ nhất tại đích trên mọi $t$.

<!-- thinking:end -->

Ta định nghĩa $f[t][i][j]$ là chi phí nhỏ nhất để đi đến ô $(i, j)$ bằng chính xác $t$ lần dịch chuyển tức thời. Ban đầu, $f[0][0][0] = 0$, còn tất cả trạng thái khác là vô cùng.

Đầu tiên, ta cần khởi tạo $f[0][i][j]$. Khi không sử dụng dịch chuyển tức thời, ta chỉ có thể đến ô $(i, j)$ bằng cách đi sang phải hoặc xuống dưới.

Nếu $i > 0$, ta có thể đi từ ô phía trên $(i-1, j)$ và cập nhật trạng thái như sau:

$$f[0][i][j] = \min(f[0][i][j], f[0][i-1][j] + grid[i][j])$$

Nếu $j > 0$, ta có thể đi từ ô bên trái $(i, j-1)$ và cập nhật trạng thái như sau:

$$f[0][i][j] = \min(f[0][i][j], f[0][i][j-1] + grid[i][j])$$

Để xử lý dịch chuyển tức thời, ta cần nhóm các ô trong lưới theo giá trị của chúng. Ta sử dụng một hash map $g$, trong đó key là giá trị của ô và value là danh sách tọa độ của các ô có cùng giá trị.

Với mỗi số lần dịch chuyển tức thời $t$ từ $1$ đến $k$, ta xử lý các nhóm theo thứ tự giảm dần của giá trị ô. Với mỗi ô $(i, j)$ trong một nhóm, trước tiên ta cập nhật giá trị nhỏ nhất toàn cục $mn$, biểu diễn chi phí nhỏ nhất để đến các ô này bằng $t-1$ lần dịch chuyển tức thời:

$$mn = \min(mn, f[t-1][i][j])$$

Sau đó, ta cập nhật trạng thái của tất cả các ô trong nhóm thành $mn$, biểu diễn chi phí nhỏ nhất để đến các ô này bằng dịch chuyển tức thời.

Tiếp theo, ta duyệt lại toàn bộ lưới để cập nhật $f[t][i][j]$, xét các bước di chuyển từ ô phía trên hoặc bên trái:

Nếu $i > 0$, ta có:

$$f[t][i][j] = \min(f[t][i][j], f[t][i-1][j] + grid[i][j])$$

Nếu $j > 0$, ta có:

$$f[t][i][j] = \min(f[t][i][j], f[t][i][j-1] + grid[i][j])$$

Cuối cùng, đáp án là $\min(f[t][m-1][n-1])$, trong đó $t$ chạy từ $0$ đến $k$.

Độ phức tạp thời gian là $O((k + \log mn) \times mn)$, và độ phức tạp không gian là $O(k \times mn)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của lưới, còn $k$ là số lần dịch chuyển tức thời tối đa được phép.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, grid: List[List[int]], k: int) -> int:
        m, n = len(grid), len(grid[0])
        f = [[[inf] * n for _ in range(m)] for _ in range(k + 1)]
        f[0][0][0] = 0
        for i in range(m):
            for j in range(n):
                if i:
                    f[0][i][j] = min(f[0][i][j], f[0][i - 1][j] + grid[i][j])
                if j:
                    f[0][i][j] = min(f[0][i][j], f[0][i][j - 1] + grid[i][j])
        g = defaultdict(list)
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                g[x].append((i, j))
        keys = sorted(g, reverse=True)
        for t in range(1, k + 1):
            mn = inf
            for key in keys:
                pos = g[key]
                for i, j in pos:
                    mn = min(mn, f[t - 1][i][j])
                for i, j in pos:
                    f[t][i][j] = mn
            for i in range(m):
                for j in range(n):
                    if i:
                        f[t][i][j] = min(f[t][i][j], f[t][i - 1][j] + grid[i][j])
                    if j:
                        f[t][i][j] = min(f[t][i][j], f[t][i][j - 1] + grid[i][j])
        return min(f[t][m - 1][n - 1] for t in range(k + 1))
```

#### Java

```java
class Solution {
    public int minCost(int[][] grid, int k) {
        int m = grid.length, n = grid[0].length;
        int inf = Integer.MAX_VALUE / 2;

        int[][][] f = new int[k + 1][m][n];
        for (int t = 0; t <= k; t++) {
            for (int i = 0; i < m; i++) {
                Arrays.fill(f[t][i], inf);
            }
        }

        f[0][0][0] = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i > 0) {
                    f[0][i][j] = Math.min(f[0][i][j], f[0][i - 1][j] + grid[i][j]);
                }
                if (j > 0) {
                    f[0][i][j] = Math.min(f[0][i][j], f[0][i][j - 1] + grid[i][j]);
                }
            }
        }

        Map<Integer, List<int[]>> g = new HashMap<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int x = grid[i][j];
                g.computeIfAbsent(x, z -> new ArrayList<>()).add(new int[] {i, j});
            }
        }

        List<Integer> keys = new ArrayList<>(g.keySet());
        keys.sort(Collections.reverseOrder());

        for (int t = 1; t <= k; t++) {
            int mn = inf;
            for (int key : keys) {
                List<int[]> pos = g.get(key);
                for (int[] p : pos) {
                    mn = Math.min(mn, f[t - 1][p[0]][p[1]]);
                }
                for (int[] p : pos) {
                    f[t][p[0]][p[1]] = mn;
                }
            }
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    if (i > 0) {
                        f[t][i][j] = Math.min(f[t][i][j], f[t][i - 1][j] + grid[i][j]);
                    }
                    if (j > 0) {
                        f[t][i][j] = Math.min(f[t][i][j], f[t][i][j - 1] + grid[i][j]);
                    }
                }
            }
        }

        int ans = inf;
        for (int t = 0; t <= k; t++) {
            ans = Math.min(ans, f[t][m - 1][n - 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        int inf = INT_MAX / 2;

        vector<vector<vector<int>>> f(k + 1, vector<vector<int>>(m, vector<int>(n, inf)));

        f[0][0][0] = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i > 0) {
                    f[0][i][j] = min(f[0][i][j], f[0][i - 1][j] + grid[i][j]);
                }
                if (j > 0) {
                    f[0][i][j] = min(f[0][i][j], f[0][i][j - 1] + grid[i][j]);
                }
            }
        }

        unordered_map<int, vector<pair<int, int>>> g;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int x = grid[i][j];
                g[x].push_back({i, j});
            }
        }

        vector<int> keys;
        keys.reserve(g.size());
        for (auto& e : g) {
            keys.push_back(e.first);
        }
        sort(keys.begin(), keys.end(), greater<int>());

        for (int t = 1; t <= k; t++) {
            int mn = inf;
            for (int key : keys) {
                auto& pos = g[key];
                for (auto& p : pos) {
                    mn = min(mn, f[t - 1][p.first][p.second]);
                }
                for (auto& p : pos) {
                    f[t][p.first][p.second] = mn;
                }
            }
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    if (i > 0) {
                        f[t][i][j] = min(f[t][i][j], f[t][i - 1][j] + grid[i][j]);
                    }
                    if (j > 0) {
                        f[t][i][j] = min(f[t][i][j], f[t][i][j - 1] + grid[i][j]);
                    }
                }
            }
        }

        int ans = inf;
        for (int t = 0; t <= k; t++) {
            ans = min(ans, f[t][m - 1][n - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func minCost(grid [][]int, k int) int {
	m, n := len(grid), len(grid[0])
	inf := int(^uint(0)>>1) / 4

	f := make([][][]int, k+1)
	for t := 0; t <= k; t++ {
		f[t] = make([][]int, m)
		for i := 0; i < m; i++ {
			f[t][i] = make([]int, n)
			for j := 0; j < n; j++ {
				f[t][i][j] = inf
			}
		}
	}

	f[0][0][0] = 0
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if i > 0 {
				f[0][i][j] = min(f[0][i][j], f[0][i-1][j]+grid[i][j])
			}
			if j > 0 {
				f[0][i][j] = min(f[0][i][j], f[0][i][j-1]+grid[i][j])
			}
		}
	}

	g := make(map[int][][]int)
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			x := grid[i][j]
			g[x] = append(g[x], []int{i, j})
		}
	}

	keys := make([]int, 0, len(g))
	for key := range g {
		keys = append(keys, key)
	}
	sort.Sort(sort.Reverse(sort.IntSlice(keys)))

	for t := 1; t <= k; t++ {
		mn := inf
		for _, key := range keys {
			pos := g[key]
			for _, p := range pos {
				mn = min(mn, f[t-1][p[0]][p[1]])
			}
			for _, p := range pos {
				f[t][p[0]][p[1]] = mn
			}
		}
		for i := 0; i < m; i++ {
			for j := 0; j < n; j++ {
				if i > 0 {
					f[t][i][j] = min(f[t][i][j], f[t][i-1][j]+grid[i][j])
				}
				if j > 0 {
					f[t][i][j] = min(f[t][i][j], f[t][i][j-1]+grid[i][j])
				}
			}
		}
	}

	ans := inf
	for t := 0; t <= k; t++ {
		ans = min(ans, f[t][m-1][n-1])
	}
	return ans
}
```

#### TypeScript

```ts
function minCost(grid: number[][], k: number): number {
    const [m, n] = [grid.length, grid[0].length];
    const INF = 1e18;

    const f: number[][][] = Array.from({ length: k + 1 }, () =>
        Array.from({ length: m }, () => Array(n).fill(INF)),
    );

    f[0][0][0] = 0;
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (i > 0) f[0][i][j] = Math.min(f[0][i][j], f[0][i - 1][j] + grid[i][j]);
            if (j > 0) f[0][i][j] = Math.min(f[0][i][j], f[0][i][j - 1] + grid[i][j]);
        }
    }

    const g = new Map<number, Array<[number, number]>>();
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            const x = grid[i][j];
            if (!g.has(x)) {
                g.set(x, []);
            }
            g.get(x)!.push([i, j]);
        }
    }

    const keys = Array.from(g.keys()).sort((a, b) => b - a);

    for (let t = 1; t <= k; t++) {
        let mn = INF;

        for (const key of keys) {
            const pos = g.get(key)!;
            for (const [x, y] of pos) {
                mn = Math.min(mn, f[t - 1][x][y]);
            }
            for (const [x, y] of pos) {
                f[t][x][y] = mn;
            }
        }

        for (let i = 0; i < m; i++) {
            for (let j = 0; j < n; j++) {
                if (i > 0) {
                    f[t][i][j] = Math.min(f[t][i][j], f[t][i - 1][j] + grid[i][j]);
                }
                if (j > 0) {
                    f[t][i][j] = Math.min(f[t][i][j], f[t][i][j - 1] + grid[i][j]);
                }
            }
        }
    }

    let ans = INF;
    for (let t = 0; t <= k; t++) {
        ans = Math.min(ans, f[t][m - 1][n - 1]);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn min_cost(grid: Vec<Vec<i32>>, k: i32) -> i32 {
        let m = grid.len();
        let n = grid[0].len();
        let k = k as usize;
        let inf: i32 = i32::MAX / 2;

        let mut f = vec![vec![vec![inf; n]; m]; k + 1];

        f[0][0][0] = 0;
        for i in 0..m {
            for j in 0..n {
                if i > 0 {
                    f[0][i][j] = f[0][i][j].min(f[0][i - 1][j] + grid[i][j]);
                }
                if j > 0 {
                    f[0][i][j] = f[0][i][j].min(f[0][i][j - 1] + grid[i][j]);
                }
            }
        }

        let mut g: HashMap<i32, Vec<(usize, usize)>> = HashMap::new();
        for i in 0..m {
            for j in 0..n {
                g.entry(grid[i][j]).or_default().push((i, j));
            }
        }

        let mut keys: Vec<i32> = g.keys().cloned().collect();
        keys.sort_by(|a, b| b.cmp(a));

        for t in 1..=k {
            let mut mn = inf;
            for &key in &keys {
                let pos = &g[&key];
                for &(i, j) in pos {
                    mn = mn.min(f[t - 1][i][j]);
                }
                for &(i, j) in pos {
                    f[t][i][j] = mn;
                }
            }
            for i in 0..m {
                for j in 0..n {
                    if i > 0 {
                        f[t][i][j] = f[t][i][j].min(f[t][i - 1][j] + grid[i][j]);
                    }
                    if j > 0 {
                        f[t][i][j] = f[t][i][j].min(f[t][i][j - 1] + grid[i][j]);
                    }
                }
            }
        }

        let mut ans = inf;
        for t in 0..=k {
            ans = ans.min(f[t][m - 1][n - 1]);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
