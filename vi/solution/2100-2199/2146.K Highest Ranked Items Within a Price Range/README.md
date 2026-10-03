---
comments: true
difficulty: Medium
rating: 1836
source: Biweekly Contest 70 Q3
tags:
    - Breadth-First Search
    - Array
    - Matrix
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2146. K Highest Ranked Items Within a Price Range](https://leetcode.com/problems/k-highest-ranked-items-within-a-price-range)

[Tài liệu tiếng Trung](/solution/2100-2199/2146.K%20Highest%20Ranked%20Items%20Within%20a%20Price%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>grid</code> được <strong>đánh chỉ số từ 0</strong>, kích thước <code>m x n</code>, biểu diễn bản đồ các mặt hàng trong một cửa hàng. Các số nguyên trong grid có ý nghĩa như sau:</p>

<ul>
	<li><code>0</code> biểu diễn một bức tường mà bạn không thể đi qua.</li>
	<li><code>1</code> biểu diễn một ô trống mà bạn có thể tự do di chuyển đến và đi khỏi.</li>
	<li>Mọi số nguyên dương khác đều biểu diễn giá của một mặt hàng trong ô đó. Bạn cũng có thể tự do di chuyển đến và đi khỏi các ô chứa mặt hàng này.</li>
</ul>

<p>Di chuyển giữa hai ô kề nhau trong grid mất <code>1</code> bước.</p>

<p>Bạn cũng được cho các mảng số nguyên <code>pricing</code> và <code>start</code>, trong đó <code>pricing = [low, high]</code> và <code>start = [row, col]</code> cho biết bạn bắt đầu tại vị trí <code>(row, col)</code> và chỉ quan tâm đến các mặt hàng có giá nằm trong đoạn <code>[low, high]</code> (<strong>bao gồm cả hai đầu mút</strong>). Ngoài ra, bạn được cho một số nguyên <code>k</code>.</p>

<p>Bạn cần tìm <strong>vị trí</strong> của <code>k</code> mặt hàng có <strong>thứ hạng cao nhất</strong> với giá <strong>nằm trong</strong> đoạn giá đã cho. Thứ hạng được xác định bởi <strong>tiêu chí đầu tiên</strong> khác nhau trong các tiêu chí sau:</p>

<ol>
	<li>Khoảng cách, được định nghĩa là độ dài đường đi ngắn nhất từ <code>start</code> (khoảng cách <strong>ngắn hơn</strong> có thứ hạng cao hơn).</li>
	<li>Giá (<strong>giá thấp hơn</strong> có thứ hạng cao hơn, nhưng giá phải <strong>nằm trong đoạn giá</strong>).</li>
	<li>Số hàng (<strong>số hàng nhỏ hơn</strong> có thứ hạng cao hơn).</li>
	<li>Số cột (<strong>số cột nhỏ hơn</strong> có thứ hạng cao hơn).</li>
</ol>

<p>Trả về <em>các </em><code>k</code><em> mặt hàng có thứ hạng cao nhất trong đoạn giá, được <strong>sắp xếp</strong> theo thứ hạng của chúng (từ cao xuống thấp)</em>. Nếu có ít hơn <code>k</code> mặt hàng có thể đi đến trong đoạn giá, hãy trả về <em><strong>tất cả</strong> các mặt hàng đó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2146.K%20Highest%20Ranked%20Items%20Within%20a%20Price%20Range/images/example1drawio.png" style="width: 200px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2,0,1],[1,3,0,1],[0,2,5,1]], pricing = [2,5], start = [0,0], k = 3
<strong>Đầu ra:</strong> [[0,1],[1,1],[2,1]]
<strong>Giải thích:</strong> Bạn bắt đầu tại (0,0).
Với đoạn giá [2,5], ta có thể lấy các mặt hàng tại (0,1), (1,1), (2,1) và (2,2).
Thứ hạng của các mặt hàng này là:
- (0,1) với khoảng cách 1
- (1,1) với khoảng cách 2
- (2,1) với khoảng cách 3
- (2,2) với khoảng cách 4
Do đó, 3 mặt hàng có thứ hạng cao nhất trong đoạn giá là (0,1), (1,1) và (2,1).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2146.K%20Highest%20Ranked%20Items%20Within%20a%20Price%20Range/images/example2drawio1.png" style="width: 200px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,2,0,1],[1,3,3,1],[0,2,5,1]], pricing = [2,3], start = [2,3], k = 2
<strong>Đầu ra:</strong> [[2,1],[1,2]]
<strong>Giải thích:</strong> Bạn bắt đầu tại (2,3).
Với đoạn giá [2,3], ta có thể lấy các mặt hàng tại (0,1), (1,1), (1,2) và (2,1).
Thứ hạng của các mặt hàng này là:
- (2,1) với khoảng cách 2, giá 2
- (1,2) với khoảng cách 2, giá 3
- (1,1) với khoảng cách 3
- (0,1) với khoảng cách 4
Do đó, 2 mặt hàng có thứ hạng cao nhất trong đoạn giá là (2,1) và (1,2).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2146.K%20Highest%20Ranked%20Items%20Within%20a%20Price%20Range/images/example3.png" style="width: 149px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,1,1],[0,0,1],[2,3,4]], pricing = [2,3], start = [0,0], k = 3
<strong>Đầu ra:</strong> [[2,1],[2,0]]
<strong>Giải thích:</strong> Bạn bắt đầu tại (0,0).
Với đoạn giá [2,3], ta có thể lấy các mặt hàng tại (2,0) và (2,1).
Thứ hạng của các mặt hàng này là:
- (2,1) với khoảng cách 5
- (2,0) với khoảng cách 6
Do đó, 2 mặt hàng có thứ hạng cao nhất trong đoạn giá là (2,1) và (2,0).
Lưu ý rằng k = 3 nhưng chỉ có 2 mặt hàng có thể đi đến trong đoạn giá.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>pricing.length == 2</code></li>
	<li><code>2 &lt;= low &lt;= high &lt;= 10<sup>5</sup></code></li>
	<li><code>start.length == 2</code></li>
	<li><code>0 &lt;= row &lt;= m - 1</code></li>
	<li><code>0 &lt;= col &lt;= n - 1</code></li>
	<li><code>grid[row][col] &gt; 0</code></li>
	<li><code>1 &lt;= k &lt;= m * n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Thứ hạng được xác định lần lượt bởi khoảng cách, giá, hàng rồi đến cột. Tìm đường đi ngắn nhất giữa mọi cặp đỉnh là quá tốn kém. BFS từ điểm bắt đầu cho ta khoảng cách có xét đến chướng ngại vật, và lần đầu tiên một ô được thăm chính là lúc đạt khoảng cách ngắn nhất.
>
> Thu thập các ô có giá nằm trong $[\textit{low},\textit{high}]$ cùng với khoảng cách của chúng, sắp xếp các bộ bốn phần tử, rồi lấy $k$ phần tử đầu tiên.
>
> Đánh dấu các ô đã thăm bằng $0$ để chúng không được thêm vào queue hai lần.

<!-- thinking:end -->

Bắt đầu từ $(\textit{row}, \textit{col})$ và sử dụng tìm kiếm theo chiều rộng để tìm tất cả mặt hàng có giá nằm trong đoạn $[\textit{low}, \textit{high}]$. Lưu khoảng cách, giá, tọa độ hàng và tọa độ cột của các mặt hàng này vào mảng $\textit{pq}$.

Cuối cùng, sắp xếp $\textit{pq}$ theo khoảng cách, giá, tọa độ hàng và tọa độ cột, rồi trả về tọa độ của $k$ mặt hàng đầu tiên.

Độ phức tạp thời gian là $O(m \times n \times \log (m \times n))$, và độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của mảng 2 chiều $\textit{grid}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def highestRankedKItems(
        self, grid: List[List[int]], pricing: List[int], start: List[int], k: int
    ) -> List[List[int]]:
        m, n = len(grid), len(grid[0])
        row, col = start
        low, high = pricing
        q = deque([(row, col)])
        pq = []
        if low <= grid[row][col] <= high:
            pq.append((0, grid[row][col], row, col))
        grid[row][col] = 0
        dirs = (-1, 0, 1, 0, -1)
        step = 0
        while q:
            step += 1
            for _ in range(len(q)):
                x, y = q.popleft()
                for a, b in pairwise(dirs):
                    nx, ny = x + a, y + b
                    if 0 <= nx < m and 0 <= ny < n and grid[nx][ny] > 0:
                        if low <= grid[nx][ny] <= high:
                            pq.append((step, grid[nx][ny], nx, ny))
                        grid[nx][ny] = 0
                        q.append((nx, ny))
        pq.sort()
        return [list(x[2:]) for x in pq[:k]]
```

#### Java

```java
class Solution {
    public List<List<Integer>> highestRankedKItems(
        int[][] grid, int[] pricing, int[] start, int k) {
        int m = grid.length;
        int n = grid[0].length;
        int row = start[0], col = start[1];
        int low = pricing[0], high = pricing[1];
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {row, col});
        List<int[]> pq = new ArrayList<>();
        if (low <= grid[row][col] && grid[row][col] <= high) {
            pq.add(new int[] {0, grid[row][col], row, col});
        }
        grid[row][col] = 0;
        final int[] dirs = {-1, 0, 1, 0, -1};
        for (int step = 1; !q.isEmpty(); ++step) {
            for (int size = q.size(); size > 0; --size) {
                int[] curr = q.poll();
                int x = curr[0], y = curr[1];
                for (int j = 0; j < 4; j++) {
                    int nx = x + dirs[j];
                    int ny = y + dirs[j + 1];
                    if (0 <= nx && nx < m && 0 <= ny && ny < n && grid[nx][ny] > 0) {
                        if (low <= grid[nx][ny] && grid[nx][ny] <= high) {
                            pq.add(new int[] {step, grid[nx][ny], nx, ny});
                        }
                        grid[nx][ny] = 0;
                        q.offer(new int[] {nx, ny});
                    }
                }
            }
        }

        pq.sort((a, b) -> {
            if (a[0] != b[0]) return Integer.compare(a[0], b[0]);
            if (a[1] != b[1]) return Integer.compare(a[1], b[1]);
            if (a[2] != b[2]) return Integer.compare(a[2], b[2]);
            return Integer.compare(a[3], b[3]);
        });

        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < Math.min(k, pq.size()); i++) {
            ans.add(List.of(pq.get(i)[2], pq.get(i)[3]));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> highestRankedKItems(vector<vector<int>>& grid, vector<int>& pricing, vector<int>& start, int k) {
        int m = grid.size(), n = grid[0].size();
        int row = start[0], col = start[1];
        int low = pricing[0], high = pricing[1];
        queue<pair<int, int>> q;
        q.push({row, col});
        vector<tuple<int, int, int, int>> pq;
        if (low <= grid[row][col] && grid[row][col] <= high) {
            pq.push_back({0, grid[row][col], row, col});
        }
        grid[row][col] = 0;
        vector<int> dirs = {-1, 0, 1, 0, -1};
        for (int step = 1; q.size(); ++step) {
            int sz = q.size();
            for (int i = 0; i < sz; ++i) {
                auto [x, y] = q.front();
                q.pop();
                for (int j = 0; j < 4; ++j) {
                    int nx = x + dirs[j];
                    int ny = y + dirs[j + 1];
                    if (0 <= nx && nx < m && 0 <= ny && ny < n && grid[nx][ny] > 0) {
                        if (low <= grid[nx][ny] && grid[nx][ny] <= high) {
                            pq.push_back({step, grid[nx][ny], nx, ny});
                        }
                        grid[nx][ny] = 0;
                        q.push({nx, ny});
                    }
                }
            }
        }
        sort(pq.begin(), pq.end());
        vector<vector<int>> ans;
        for (int i = 0; i < min(k, (int) pq.size()); ++i) {
            ans.push_back({get<2>(pq[i]), get<3>(pq[i])});
        }
        return ans;
    }
};
```

#### Go

```go
func highestRankedKItems(grid [][]int, pricing []int, start []int, k int) (ans [][]int) {
	m, n := len(grid), len(grid[0])
	row, col := start[0], start[1]
	low, high := pricing[0], pricing[1]
	q := [][2]int{{row, col}}
	pq := [][]int{}
	if low <= grid[row][col] && grid[row][col] <= high {
		pq = append(pq, []int{0, grid[row][col], row, col})
	}
	grid[row][col] = 0
	dirs := [5]int{-1, 0, 1, 0, -1}
	for step := 1; len(q) > 0; step++ {
		for sz := len(q); sz > 0; sz-- {
			x, y := q[0][0], q[0][1]
			q = q[1:]
			for j := 0; j < 4; j++ {
				nx, ny := x+dirs[j], y+dirs[j+1]
				if nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] > 0 {
					if low <= grid[nx][ny] && grid[nx][ny] <= high {
						pq = append(pq, []int{step, grid[nx][ny], nx, ny})
					}
					grid[nx][ny] = 0
					q = append(q, [2]int{nx, ny})
				}
			}
		}
	}
	sort.Slice(pq, func(i, j int) bool {
		a, b := pq[i], pq[j]
		if a[0] != b[0] {
			return a[0] < b[0]
		}
		if a[1] != b[1] {
			return a[1] < b[1]
		}
		if a[2] != b[2] {
			return a[2] < b[2]
		}
		return a[3] < b[3]
	})
	for i := 0; i < len(pq) && i < k; i++ {
		ans = append(ans, pq[i][2:])
	}
	return
}
```

#### TypeScript

```ts
function highestRankedKItems(
    grid: number[][],
    pricing: number[],
    start: number[],
    k: number,
): number[][] {
    const [m, n] = [grid.length, grid[0].length];
    const [row, col] = start;
    const [low, high] = pricing;
    let q: [number, number][] = [[row, col]];
    const pq: [number, number, number, number][] = [];
    if (low <= grid[row][col] && grid[row][col] <= high) {
        pq.push([0, grid[row][col], row, col]);
    }
    grid[row][col] = 0;
    const dirs = [-1, 0, 1, 0, -1];
    for (let step = 1; q.length > 0; ++step) {
        const nq: [number, number][] = [];
        for (const [x, y] of q) {
            for (let j = 0; j < 4; j++) {
                const nx = x + dirs[j];
                const ny = y + dirs[j + 1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] > 0) {
                    if (low <= grid[nx][ny] && grid[nx][ny] <= high) {
                        pq.push([step, grid[nx][ny], nx, ny]);
                    }
                    grid[nx][ny] = 0;
                    nq.push([nx, ny]);
                }
            }
        }
        q = nq;
    }
    pq.sort((a, b) => {
        if (a[0] !== b[0]) return a[0] - b[0];
        if (a[1] !== b[1]) return a[1] - b[1];
        if (a[2] !== b[2]) return a[2] - b[2];
        return a[3] - b[3];
    });
    const ans: number[][] = [];
    for (let i = 0; i < Math.min(k, pq.length); i++) {
        ans.push([pq[i][2], pq[i][3]]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
