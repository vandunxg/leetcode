---
comments: true
difficulty: Hard
rating: 2581
source: Weekly Contest 340 Q4
tags:
    - Stack
    - Breadth-First Search
    - Union Find
    - Array
    - Dynamic Programming
    - Matrix
    - Monotonic Stack
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2617. Minimum Number of Visited Cells in a Grid](https://leetcode.com/problems/minimum-number-of-visited-cells-in-a-grid)

[中文文档](/solution/2600-2699/2617.Minimum%20Number%20of%20Visited%20Cells%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận số nguyên <code>m x n</code> được đánh chỉ số từ <strong>0</strong> là <code>grid</code>. Vị trí ban đầu của bạn là ô <strong>trên cùng bên trái</strong> <code>(0, 0)</code>.</p>

<p>Bắt đầu từ ô <code>(i, j)</code>, bạn có thể di chuyển đến một trong các ô sau:</p>

<ul>
	<li>Các ô <code>(i, k)</code> với <code>j &lt; k &lt;= grid[i][j] + j</code> (di chuyển sang phải), hoặc</li>
	<li>Các ô <code>(k, j)</code> với <code>i &lt; k &lt;= grid[i][j] + i</code> (di chuyển xuống dưới).</li>
</ul>

<p>Trả về <em>số ô nhỏ nhất cần đi qua để đến ô <strong>dưới cùng bên phải</strong></em> <code>(m - 1, n - 1)</code>. Nếu không tồn tại đường đi hợp lệ, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2617.Minimum%20Number%20of%20Visited%20Cells%20in%20a%20Grid/images/ex1.png" style="width: 271px; height: 171px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[3,4,2,1],[4,2,3,1],[2,1,0,0],[2,4,0,0]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Hình ảnh bên trên minh họa một đường đi qua đúng 4 ô.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2617.Minimum%20Number%20of%20Visited%20Cells%20in%20a%20Grid/images/ex2.png" style="width: 271px; height: 171px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[3,4,2,1],[4,2,1,1],[2,1,1,0],[3,4,1,0]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Hình ảnh bên trên minh họa một đường đi qua đúng 3 ô.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2617.Minimum%20Number%20of%20Visited%20Cells%20in%20a%20Grid/images/ex3.png" style="width: 181px; height: 81px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[2,1,0],[1,0,0]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không tồn tại đường đi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= grid[i][j] &lt; m * n</code></li>
	<li><code>grid[m - 1][n - 1] == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàng đợi ưu tiên

<!-- thinking:start -->

> **Tư duy**
>
> Từ $(0,0)$, ta có thể nhảy sang phải hoặc xuống dưới nhiều nhất bằng khoảng cách được ghi trong ô, với mục tiêu đi qua ít ô nhất. BFS ngây thơ sẽ duyệt mọi ô có thể đến và, với tối đa $10^5$ ô, phải xét rất nhiều bước nhảy đã không còn hiệu lực.
>
> Với $(i,j)$, ta chỉ cần tìm ô tiền nhiệm gần nhất trong hàng có thể vẫn đến được cột $j$, hoặc trong cột có thể vẫn đến được hàng $i$. Dùng một min-heap cho mỗi hàng và mỗi cột, sắp xếp theo khoảng cách, rồi loại các phần tử đầu không còn đến được chỉ số hiện tại.
>
> Điền $dist$ theo thứ tự từng hàng và khi một ô có thể đến được, đưa ô đó vào heap của hàng và heap của cột tương ứng.

<!-- thinking:end -->

Gọi số hàng của ma trận là $m$ và số cột là $n$. Đặt $dist[i][j]$ là khoảng cách ngắn nhất từ tọa độ $(0, 0)$ đến tọa độ $(i, j)$. Ban đầu, $dist[0][0]=1$ và $dist[i][j]=-1$ với mọi $i$ và $j$ khác.

Với mỗi ô $(i, j)$, ta có thể đi đến đó từ ô phía trên hoặc ô bên trái. Nếu đi từ ô phía trên $(i', j)$, trong đó $0 \leq i' \lt i$, thì $(i', j)$ phải thỏa mãn $grid[i'][j] + i' \geq i$. Ta cần chọn ô gần nhất trong số các ô này.

Vì vậy, ta duy trì một priority queue (min-heap) cho mỗi cột $j$. Mỗi phần tử của priority queue là một cặp $(dist[i][j], i)$, biểu diễn rằng khoảng cách ngắn nhất từ tọa độ $(0, 0)$ đến tọa độ $(i, j)$ là $dist[i][j]$. Khi xét tọa độ $(i, j)$, ta chỉ cần lấy phần tử đầu $(dist[i'][j], i')$ của priority queue. Nếu $grid[i'][j] + i' \geq i$, ta có thể di chuyển từ tọa độ $(i', j)$ đến tọa độ $(i, j)$. Khi đó, ta cập nhật giá trị của $dist[i][j]$, tức là $dist[i][j] = dist[i'][j] + 1$, rồi thêm $(dist[i][j], i)$ vào priority queue.

Tương tự, ta có thể duy trì một priority queue cho mỗi hàng $i$ và thực hiện thao tác tương tự.

Cuối cùng, ta nhận được khoảng cách ngắn nhất từ tọa độ $(0, 0)$ đến tọa độ $(m - 1, n - 1)$, tức là $dist[m - 1][n - 1]$, chính là đáp án.

Độ phức tạp thời gian là $O(m \times n \times \log (m \times n))$ và độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumVisitedCells(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        dist = [[-1] * n for _ in range(m)]
        dist[0][0] = 1
        row = [[] for _ in range(m)]
        col = [[] for _ in range(n)]
        for i in range(m):
            for j in range(n):
                while row[i] and grid[i][row[i][0][1]] + row[i][0][1] < j:
                    heappop(row[i])
                if row[i] and (dist[i][j] == -1 or dist[i][j] > row[i][0][0] + 1):
                    dist[i][j] = row[i][0][0] + 1
                while col[j] and grid[col[j][0][1]][j] + col[j][0][1] < i:
                    heappop(col[j])
                if col[j] and (dist[i][j] == -1 or dist[i][j] > col[j][0][0] + 1):
                    dist[i][j] = col[j][0][0] + 1
                if dist[i][j] != -1:
                    heappush(row[i], (dist[i][j], j))
                    heappush(col[j], (dist[i][j], i))
        return dist[-1][-1]
```

#### Java

```java
class Solution {
    public int minimumVisitedCells(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] dist = new int[m][n];
        PriorityQueue<int[]>[] row = new PriorityQueue[m];
        PriorityQueue<int[]>[] col = new PriorityQueue[n];
        for (int i = 0; i < m; ++i) {
            Arrays.fill(dist[i], -1);
            row[i] = new PriorityQueue<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        }
        for (int i = 0; i < n; ++i) {
            col[i] = new PriorityQueue<>((a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        }
        dist[0][0] = 1;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                while (!row[i].isEmpty() && grid[i][row[i].peek()[1]] + row[i].peek()[1] < j) {
                    row[i].poll();
                }
                if (!row[i].isEmpty() && (dist[i][j] == -1 || row[i].peek()[0] + 1 < dist[i][j])) {
                    dist[i][j] = row[i].peek()[0] + 1;
                }
                while (!col[j].isEmpty() && grid[col[j].peek()[1]][j] + col[j].peek()[1] < i) {
                    col[j].poll();
                }
                if (!col[j].isEmpty() && (dist[i][j] == -1 || col[j].peek()[0] + 1 < dist[i][j])) {
                    dist[i][j] = col[j].peek()[0] + 1;
                }
                if (dist[i][j] != -1) {
                    row[i].offer(new int[] {dist[i][j], j});
                    col[j].offer(new int[] {dist[i][j], i});
                }
            }
        }
        return dist[m - 1][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumVisitedCells(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dist(m, vector<int>(n, -1));
        using pii = pair<int, int>;
        priority_queue<pii, vector<pii>, greater<pii>> row[m];
        priority_queue<pii, vector<pii>, greater<pii>> col[n];
        dist[0][0] = 1;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                while (!row[i].empty() && grid[i][row[i].top().second] + row[i].top().second < j) {
                    row[i].pop();
                }
                if (!row[i].empty() && (dist[i][j] == -1 || row[i].top().first + 1 < dist[i][j])) {
                    dist[i][j] = row[i].top().first + 1;
                }
                while (!col[j].empty() && grid[col[j].top().second][j] + col[j].top().second < i) {
                    col[j].pop();
                }
                if (!col[j].empty() && (dist[i][j] == -1 || col[j].top().first + 1 < dist[i][j])) {
                    dist[i][j] = col[j].top().first + 1;
                }
                if (dist[i][j] != -1) {
                    row[i].emplace(dist[i][j], j);
                    col[j].emplace(dist[i][j], i);
                }
            }
        }
        return dist[m - 1][n - 1];
    }
};
```

#### Go

```go
func minimumVisitedCells(grid [][]int) int {
	m, n := len(grid), len(grid[0])
	dist := make([][]int, m)
	row := make([]hp, m)
	col := make([]hp, n)
	for i := range dist {
		dist[i] = make([]int, n)
		for j := range dist[i] {
			dist[i][j] = -1
		}
	}
	dist[0][0] = 1
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			for len(row[i]) > 0 && grid[i][row[i][0].second]+row[i][0].second < j {
				heap.Pop(&row[i])
			}
			if len(row[i]) > 0 && (dist[i][j] == -1 || row[i][0].first+1 < dist[i][j]) {
				dist[i][j] = row[i][0].first + 1
			}
			for len(col[j]) > 0 && grid[col[j][0].second][j]+col[j][0].second < i {
				heap.Pop(&col[j])
			}
			if len(col[j]) > 0 && (dist[i][j] == -1 || col[j][0].first+1 < dist[i][j]) {
				dist[i][j] = col[j][0].first + 1
			}
			if dist[i][j] != -1 {
				heap.Push(&row[i], pair{dist[i][j], j})
				heap.Push(&col[j], pair{dist[i][j], i})
			}
		}
	}
	return dist[m-1][n-1]
}

type pair struct {
	first  int
	second int
}

type hp []pair

func (a hp) Len() int      { return len(a) }
func (a hp) Swap(i, j int) { a[i], a[j] = a[j], a[i] }
func (a hp) Less(i, j int) bool {
	return a[i].first < a[j].first || (a[i].first == a[j].first && a[i].second < a[j].second)
}
func (a *hp) Push(x any) { *a = append(*a, x.(pair)) }
func (a *hp) Pop() any   { l := len(*a); t := (*a)[l-1]; *a = (*a)[:l-1]; return t }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
