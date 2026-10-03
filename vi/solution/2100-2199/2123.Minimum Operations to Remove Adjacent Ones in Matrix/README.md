---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Graph
    - Array
    - Matrix
    - Edmonds–Karp
    - Dinic
    - MPM
    - Push-Relabel
    - Network Flow
---

<!-- problem:start -->

# [2123. Minimum Operations to Remove Adjacent Ones in Matrix 🔒](https://leetcode.com/problems/minimum-operations-to-remove-adjacent-ones-in-matrix)

[中文文档](/solution/2100-2199/2123.Minimum%20Operations%20to%20Remove%20Adjacent%20Ones%20in%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận nhị phân <strong>0-indexed</strong> <code>grid</code>. Trong một thao tác, bạn có thể lật bất kỳ <code>1</code> nào trong <code>grid</code> thành <code>0</code>.</p>

<p>Một ma trận nhị phân được gọi là <strong>cô lập tốt</strong> nếu không có <code>1</code> nào trong ma trận được <strong>kết nối theo 4 hướng</strong> (tức là theo chiều ngang và chiều dọc) với một <code>1</code> khác.</p>

<p>Hãy trả về <em>số thao tác nhỏ nhất để biến </em><code>grid</code><em> thành một ma trận <strong>cô lập tốt</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2123.Minimum%20Operations%20to%20Remove%20Adjacent%20Ones%20in%20Matrix/images/image-20211223181501-1.png" style="width: 644px; height: 250px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,0],[0,1,1],[1,1,1]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Dùng 3 thao tác để đổi grid[0][1], grid[1][2] và grid[2][1] thành 0.
Sau đó, không còn các số 1 nào được kết nối theo 4 hướng và grid là ma trận cô lập tốt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2123.Minimum%20Operations%20to%20Remove%20Adjacent%20Ones%20in%20Matrix/images/image-20211223181518-2.png" style="height: 250px; width: 255px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,0,0],[0,0,0],[0,0,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> grid không có số 1 nào và là ma trận cô lập tốt.
Không thực hiện thao tác nào nên trả về 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2123.Minimum%20Operations%20to%20Remove%20Adjacent%20Ones%20in%20Matrix/images/image-20211223181817-3.png" style="width: 165px; height: 167px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1],[1,0]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có số 1 nào được kết nối theo 4 hướng với số 1 khác và grid là ma trận cô lập tốt.
Không thực hiện thao tác nào nên trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>grid[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Hungarian

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xóa một số $1$, và ta phải loại bỏ mọi cặp $1$ kề nhau — đây là bài toán phủ đỉnh nhỏ nhất của các cạnh đó. Ma trận tạo thành một đồ thị hai phía theo tính chẵn lẻ của $i+j$.
>
> Trong đồ thị hai phía, phủ đỉnh nhỏ nhất bằng matching cực đại, có thể được tính bằng thuật toán Hungarian với kích thước ma trận này.
>
> Nối mỗi ô chứa $1$ thuộc nhóm lẻ với các số $1$ lân cận rồi tìm các đường tăng; kích thước matching là số thao tác nhỏ nhất.

<!-- thinking:end -->

Ta nhận thấy rằng nếu hai số $1$ trong ma trận kề nhau, chúng phải thuộc hai nhóm khác nhau. Do đó, ta có thể xem tất cả số $1$ trong ma trận là các đỉnh, rồi nối một cạnh giữa hai số $1$ kề nhau để xây dựng một đồ thị hai phía.

Khi đó, bài toán được chuyển thành tìm phủ đỉnh nhỏ nhất của một đồ thị hai phía, tức là chọn số đỉnh ít nhất để phủ tất cả các cạnh. Vì phủ đỉnh nhỏ nhất của đồ thị hai phía bằng matching cực đại, ta có thể dùng thuật toán Hungarian để tìm matching cực đại của đồ thị hai phía.

Ý tưởng cốt lõi của thuật toán Hungarian là liên tục tìm các đường tăng bắt đầu từ những đỉnh chưa được ghép, cho đến khi không còn đường tăng nào, từ đó thu được matching cực đại.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $n$ và $m$ lần lượt là số lượng số $1$ và số lượng cạnh trong ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, grid: List[List[int]]) -> int:
        def find(i: int) -> int:
            for j in g[i]:
                if j not in vis:
                    vis.add(j)
                    if match[j] == -1 or find(match[j]):
                        match[j] = i
                        return 1
            return 0

        g = defaultdict(list)
        m, n = len(grid), len(grid[0])
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                if (i + j) % 2 and v:
                    x = i * n + j
                    if i < m - 1 and grid[i + 1][j]:
                        g[x].append(x + n)
                    if i and grid[i - 1][j]:
                        g[x].append(x - n)
                    if j < n - 1 and grid[i][j + 1]:
                        g[x].append(x + 1)
                    if j and grid[i][j - 1]:
                        g[x].append(x - 1)

        match = [-1] * (m * n)
        ans = 0
        for i in g.keys():
            vis = set()
            ans += find(i)
        return ans
```

#### Java

```java
class Solution {
    private Map<Integer, List<Integer>> g = new HashMap<>();
    private Set<Integer> vis = new HashSet<>();
    private int[] match;

    public int minimumOperations(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i + j) % 2 == 1 && grid[i][j] == 1) {
                    int x = i * n + j;
                    if (i < m - 1 && grid[i + 1][j] == 1) {
                        g.computeIfAbsent(x, z -> new ArrayList<>()).add(x + n);
                    }
                    if (i > 0 && grid[i - 1][j] == 1) {
                        g.computeIfAbsent(x, z -> new ArrayList<>()).add(x - n);
                    }
                    if (j < n - 1 && grid[i][j + 1] == 1) {
                        g.computeIfAbsent(x, z -> new ArrayList<>()).add(x + 1);
                    }
                    if (j > 0 && grid[i][j - 1] == 1) {
                        g.computeIfAbsent(x, z -> new ArrayList<>()).add(x - 1);
                    }
                }
            }
        }
        match = new int[m * n];
        Arrays.fill(match, -1);
        int ans = 0;
        for (int i : g.keySet()) {
            ans += find(i);
            vis.clear();
        }
        return ans;
    }

    private int find(int i) {
        for (int j : g.get(i)) {
            if (vis.add(j)) {
                if (match[j] == -1 || find(match[j]) == 1) {
                    match[j] = i;
                    return 1;
                }
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int> match(m * n, -1);
        unordered_set<int> vis;
        unordered_map<int, vector<int>> g;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i + j) % 2 && grid[i][j]) {
                    int x = i * n + j;
                    if (i < m - 1 && grid[i + 1][j]) {
                        g[x].push_back(x + n);
                    }
                    if (i && grid[i - 1][j]) {
                        g[x].push_back(x - n);
                    }
                    if (j < n - 1 && grid[i][j + 1]) {
                        g[x].push_back(x + 1);
                    }
                    if (j && grid[i][j - 1]) {
                        g[x].push_back(x - 1);
                    }
                }
            }
        }
        int ans = 0;
        function<int(int)> find = [&](int i) -> int {
            for (int& j : g[i]) {
                if (!vis.count(j)) {
                    vis.insert(j);
                    if (match[j] == -1 || find(match[j])) {
                        match[j] = i;
                        return 1;
                    }
                }
            }
            return 0;
        };
        for (auto& [i, _] : g) {
            ans += find(i);
            vis.clear();
        }
        return ans;
    }
};
```

#### Go

```go
func minimumOperations(grid [][]int) (ans int) {
	m, n := len(grid), len(grid[0])
	vis := map[int]bool{}
	match := make([]int, m*n)
	for i := range match {
		match[i] = -1
	}
	g := map[int][]int{}
	for i, row := range grid {
		for j, v := range row {
			if (i+j)&1 == 1 && v == 1 {
				x := i*n + j
				if i < m-1 && grid[i+1][j] == 1 {
					g[x] = append(g[x], x+n)
				}
				if i > 0 && grid[i-1][j] == 1 {
					g[x] = append(g[x], x-n)
				}
				if j < n-1 && grid[i][j+1] == 1 {
					g[x] = append(g[x], x+1)
				}
				if j > 0 && grid[i][j-1] == 1 {
					g[x] = append(g[x], x-1)
				}
			}
		}
	}
	var find func(int) int
	find = func(i int) int {
		for _, j := range g[i] {
			if !vis[j] {
				vis[j] = true
				if match[j] == -1 || find(match[j]) == 1 {
					match[j] = i
					return 1
				}
			}
		}
		return 0
	}
	for i := range g {
		ans += find(i)
		vis = map[int]bool{}
	}
	return
}
```

#### TypeScript

```ts
function minimumOperations(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const match: number[] = Array(m * n).fill(-1);
    const vis: Set<number> = new Set();
    const g: Map<number, number[]> = new Map();
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if ((i + j) % 2 && grid[i][j]) {
                const x = i * n + j;
                g.set(x, []);
                if (i < m - 1 && grid[i + 1][j]) {
                    g.get(x)!.push(x + n);
                }
                if (i && grid[i - 1][j]) {
                    g.get(x)!.push(x - n);
                }
                if (j < n - 1 && grid[i][j + 1]) {
                    g.get(x)!.push(x + 1);
                }
                if (j && grid[i][j - 1]) {
                    g.get(x)!.push(x - 1);
                }
            }
        }
    }
    const find = (i: number): number => {
        for (const j of g.get(i)!) {
            if (!vis.has(j)) {
                vis.add(j);
                if (match[j] === -1 || find(match[j])) {
                    match[j] = i;
                    return 1;
                }
            }
        }
        return 0;
    };
    let ans = 0;
    for (const i of g.keys()) {
        ans += find(i);
        vis.clear();
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
