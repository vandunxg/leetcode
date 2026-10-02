---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow)

[中文文档](/solution/0400-0499/0417.Pacific%20Atlantic%20Water%20Flow/README.md)

## Mô tả

<!-- description:start -->

<p>Có một hòn đảo hình chữ nhật gồm <code>m x n</code> ô, giáp cả <strong>Thái Bình Dương</strong> và <strong>Đại Tây Dương</strong>. Thái Bình Dương tiếp giáp cạnh trái và cạnh trên của đảo, còn Đại Tây Dương tiếp giáp cạnh phải và cạnh dưới.</p>

<p>Hòn đảo được chia thành một lưới ô vuông. Cho ma trận số nguyên <code>heights</code> kích thước <code>m x n</code>, trong đó <code>heights[r][c]</code> biểu diễn <strong>độ cao so với mực nước biển</strong> của ô tại tọa độ <code>(r, c)</code>.</p>

<p>Đảo có nhiều mưa; nước mưa có thể chảy sang các ô kề theo hướng bắc, nam, đông hoặc tây nếu ô kế bên có độ cao <strong>nhỏ hơn hoặc bằng</strong> ô hiện tại. Nước từ bất kỳ ô nào tiếp giáp đại dương đều có thể chảy ra đại dương.</p>

<p>Trả về <em>danh sách tọa độ ô dạng <strong>2D</strong> </em><code>result</code><em>, trong đó </em><code>result[i] = [r<sub>i</sub>, c<sub>i</sub>]</code><em> cho biết nước mưa từ ô </em><code>(r<sub>i</sub>, c<sub>i</sub>)</code><em> có thể chảy đến <strong>cả hai</strong> đại dương Thái Bình Dương và Đại Tây Dương</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0417.Pacific%20Atlantic%20Water%20Flow/images/waterflow-grid.jpg" style="width: 400px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
<strong>Đầu ra:</strong> [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
<strong>Giải thích:</strong> Các ô sau có thể chảy ra cả Thái Bình Dương và Đại Tây Dương, như minh họa dưới đây:
[0,4]: [0,4] -&gt; Thái Bình Dương 
&nbsp;      [0,4] -&gt; Đại Tây Dương
[1,3]: [1,3] -&gt; [0,3] -&gt; Thái Bình Dương 
&nbsp;      [1,3] -&gt; [1,4] -&gt; Đại Tây Dương
[1,4]: [1,4] -&gt; [1,3] -&gt; [0,3] -&gt; Thái Bình Dương 
&nbsp;      [1,4] -&gt; Đại Tây Dương
[2,2]: [2,2] -&gt; [1,2] -&gt; [0,2] -&gt; Thái Bình Dương 
&nbsp;      [2,2] -&gt; [2,3] -&gt; [2,4] -&gt; Đại Tây Dương
[3,0]: [3,0] -&gt; Thái Bình Dương 
&nbsp;      [3,0] -&gt; [4,0] -&gt; Đại Tây Dương
[3,1]: [3,1] -&gt; [3,0] -&gt; Thái Bình Dương 
&nbsp;      [3,1] -&gt; [4,1] -&gt; Đại Tây Dương
[4,0]: [4,0] -&gt; Thái Bình Dương 
       [4,0] -&gt; Đại Tây Dương
Lưu ý rằng các ô này còn có những đường đi khác để nước chảy ra Thái Bình Dương và Đại Tây Dương.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [[1]]
<strong>Đầu ra:</strong> [[0,0]]
<strong>Giải thích:</strong> Nước có thể chảy từ ô duy nhất này ra cả Thái Bình Dương và Đại Tây Dương.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == heights.length</code></li>
	<li><code>n == heights[r].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>0 &lt;= heights[r][c] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Nếu duyệt từ từng ô theo chiều nước chảy ra đại dương, ta sẽ ghé lại nhiều ô. Với $m,n\le 200$, việc lặp lại này khá tốn kém.
>
> Đảo ngược hướng chảy xuống là đi lên các ô lân cận có độ cao lớn hơn hoặc bằng. Chạy BFS từ biên giáp Thái Bình Dương và từ biên giáp Đại Tây Dương; giao của hai tập ô tìm được là những ô có thể đến cả hai đại dương.
>
> Chỉ xét ô lân cận khi $\textit{heights}[nx][ny]\ge \textit{heights}[x][y]$. Bắt đầu từ biên đảm bảo có đường đi ngược về đại dương tương ứng.

<!-- thinking:end -->

Ta có thể bắt đầu từ các biên giáp Thái Bình Dương và Đại Tây Dương, lần lượt chạy tìm kiếm theo chiều rộng (BFS) để tìm mọi ô có thể chảy ra từng đại dương. Cuối cùng, lấy giao của hai tập kết quả để tìm các ô có thể chảy ra cả hai đại dương.

Cụ thể, ta dùng queue $q_1$ lưu các ô giáp Thái Bình Dương và ma trận boolean $vis_1$ đánh dấu những ô có thể chảy ra đại dương này. Tương tự, queue $q_2$ và ma trận boolean $vis_2$ dùng cho Đại Tây Dương. Ban đầu, thêm tất cả ô giáp Thái Bình Dương vào $q_1$ và đánh dấu chúng trong $vis_1$; làm tương tự với các ô giáp Đại Tây Dương, $q_2$ và $vis_2$.

Sau đó, lần lượt chạy BFS trên $q_1$ và $q_2$. Trong mỗi lượt BFS, lấy ô $(x, y)$ khỏi queue rồi kiểm tra bốn ô kề $(nx, ny)$. Nếu ô kề nằm trong phạm vi ma trận, chưa được thăm và có độ cao không thấp hơn ô hiện tại (tức nước có thể chảy đến đó khi duyệt ngược), ta thêm ô ấy vào queue và đánh dấu đã thăm.

Cuối cùng, duyệt toàn bộ ma trận để tìm các ô được đánh dấu đã thăm trong cả $vis_1$ và $vis_2$. Đó là kết quả cần trả về.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        def bfs(q: Deque[Tuple[int, int]], vis: List[List[bool]]) -> None:
            while q:
                x, y = q.popleft()
                for dx, dy in pairwise(dirs):
                    nx, ny = x + dx, y + dy
                    if (
                        0 <= nx < m
                        and 0 <= ny < n
                        and not vis[nx][ny]
                        and heights[nx][ny] >= heights[x][y]
                    ):
                        vis[nx][ny] = True
                        q.append((nx, ny))

        m, n = len(heights), len(heights[0])
        vis1 = [[False] * n for _ in range(m)]
        vis2 = [[False] * n for _ in range(m)]
        q1: Deque[Tuple[int, int]] = deque()
        q2: Deque[Tuple[int, int]] = deque()
        dirs = (-1, 0, 1, 0, -1)

        for i in range(m):
            q1.append((i, 0))
            vis1[i][0] = True
            q2.append((i, n - 1))
            vis2[i][n - 1] = True

        for j in range(n):
            q1.append((0, j))
            vis1[0][j] = True
            q2.append((m - 1, j))
            vis2[m - 1][j] = True

        bfs(q1, vis1)
        bfs(q2, vis2)

        return [(i, j) for i in range(m) for j in range(n) if vis1[i][j] and vis2[i][j]]
```

#### Java

```java
class Solution {
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        int m = heights.length, n = heights[0].length;
        boolean[][] vis1 = new boolean[m][n];
        boolean[][] vis2 = new boolean[m][n];
        Deque<int[]> q1 = new ArrayDeque<>();
        Deque<int[]> q2 = new ArrayDeque<>();
        int[] dirs = {-1, 0, 1, 0, -1};

        for (int i = 0; i < m; ++i) {
            q1.offer(new int[] {i, 0});
            vis1[i][0] = true;
            q2.offer(new int[] {i, n - 1});
            vis2[i][n - 1] = true;
        }
        for (int j = 0; j < n; ++j) {
            q1.offer(new int[] {0, j});
            vis1[0][j] = true;
            q2.offer(new int[] {m - 1, j});
            vis2[m - 1][j] = true;
        }

        BiConsumer<Deque<int[]>, boolean[][]> bfs = (q, vis) -> {
            while (!q.isEmpty()) {
                var cell = q.poll();
                int x = cell[0], y = cell[1];
                for (int k = 0; k < 4; ++k) {
                    int nx = x + dirs[k], ny = y + dirs[k + 1];
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n && !vis[nx][ny]
                        && heights[nx][ny] >= heights[x][y]) {
                        vis[nx][ny] = true;
                        q.offer(new int[] {nx, ny});
                    }
                }
            }
        };

        bfs.accept(q1, vis1);
        bfs.accept(q2, vis2);

        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (vis1[i][j] && vis2[i][j]) {
                    ans.add(List.of(i, j));
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
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int m = heights.size(), n = heights[0].size();
        vector<vector<bool>> vis1(m, vector<bool>(n, false)), vis2(m, vector<bool>(n, false));
        queue<pair<int, int>> q1, q2;
        vector<int> dirs = {-1, 0, 1, 0, -1};

        for (int i = 0; i < m; ++i) {
            q1.emplace(i, 0);
            vis1[i][0] = true;
            q2.emplace(i, n - 1);
            vis2[i][n - 1] = true;
        }
        for (int j = 0; j < n; ++j) {
            q1.emplace(0, j);
            vis1[0][j] = true;
            q2.emplace(m - 1, j);
            vis2[m - 1][j] = true;
        }

        auto bfs = [&](queue<pair<int, int>>& q, vector<vector<bool>>& vis) {
            while (!q.empty()) {
                auto [x, y] = q.front();
                q.pop();
                for (int k = 0; k < 4; ++k) {
                    int nx = x + dirs[k], ny = y + dirs[k + 1];
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n
                        && !vis[nx][ny]
                        && heights[nx][ny] >= heights[x][y]) {
                        vis[nx][ny] = true;
                        q.emplace(nx, ny);
                    }
                }
            }
        };

        bfs(q1, vis1);
        bfs(q2, vis2);

        vector<vector<int>> ans;
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                if (vis1[i][j] && vis2[i][j])
                    ans.push_back({i, j});
        return ans;
    }
};
```

#### Go

```go
func pacificAtlantic(heights [][]int) [][]int {
	m, n := len(heights), len(heights[0])
	vis1 := make([][]bool, m)
	vis2 := make([][]bool, m)
	for i := range vis1 {
		vis1[i] = make([]bool, n)
		vis2[i] = make([]bool, n)
	}
	q1, q2 := [][2]int{}, [][2]int{}
	dirs := [5]int{-1, 0, 1, 0, -1}

	for i := 0; i < m; i++ {
		q1 = append(q1, [2]int{i, 0})
		vis1[i][0] = true
		q2 = append(q2, [2]int{i, n - 1})
		vis2[i][n-1] = true
	}
	for j := 0; j < n; j++ {
		q1 = append(q1, [2]int{0, j})
		vis1[0][j] = true
		q2 = append(q2, [2]int{m - 1, j})
		vis2[m-1][j] = true
	}

	bfs := func(q [][2]int, vis [][]bool) {
		for len(q) > 0 {
			x, y := q[0][0], q[0][1]
			q = q[1:]
			for k := 0; k < 4; k++ {
				nx, ny := x+dirs[k], y+dirs[k+1]
				if nx >= 0 && nx < m && ny >= 0 && ny < n &&
					!vis[nx][ny] && heights[nx][ny] >= heights[x][y] {
					vis[nx][ny] = true
					q = append(q, [2]int{nx, ny})
				}
			}
		}
	}

	bfs(q1, vis1)
	bfs(q2, vis2)

	var ans [][]int
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if vis1[i][j] && vis2[i][j] {
				ans = append(ans, []int{i, j})
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function pacificAtlantic(heights: number[][]): number[][] {
    const m = heights.length,
        n = heights[0].length;
    const vis1: boolean[][] = Array.from({ length: m }, () => Array(n).fill(false));
    const vis2: boolean[][] = Array.from({ length: m }, () => Array(n).fill(false));
    const q1: [number, number][] = [];
    const q2: [number, number][] = [];
    const dirs = [-1, 0, 1, 0, -1];

    for (let i = 0; i < m; ++i) {
        q1.push([i, 0]);
        vis1[i][0] = true;
        q2.push([i, n - 1]);
        vis2[i][n - 1] = true;
    }
    for (let j = 0; j < n; ++j) {
        q1.push([0, j]);
        vis1[0][j] = true;
        q2.push([m - 1, j]);
        vis2[m - 1][j] = true;
    }

    const bfs = (q: [number, number][], vis: boolean[][]) => {
        while (q.length) {
            const [x, y] = q.shift()!;
            for (let k = 0; k < 4; ++k) {
                const nx = x + dirs[k],
                    ny = y + dirs[k + 1];
                if (
                    nx >= 0 &&
                    nx < m &&
                    ny >= 0 &&
                    ny < n &&
                    !vis[nx][ny] &&
                    heights[nx][ny] >= heights[x][y]
                ) {
                    vis[nx][ny] = true;
                    q.push([nx, ny]);
                }
            }
        }
    };

    bfs(q1, vis1);
    bfs(q2, vis2);

    const ans: number[][] = [];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (vis1[i][j] && vis2[i][j]) {
                ans.push([i, j]);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn pacific_atlantic(heights: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let (m, n) = (heights.len(), heights[0].len());
        let mut vis1 = vec![vec![false; n]; m];
        let mut vis2 = vec![vec![false; n]; m];
        let mut q1 = VecDeque::new();
        let mut q2 = VecDeque::new();
        let dirs = [-1, 0, 1, 0, -1];

        for i in 0..m {
            q1.push_back((i, 0));
            vis1[i][0] = true;
            q2.push_back((i, n - 1));
            vis2[i][n - 1] = true;
        }
        for j in 0..n {
            q1.push_back((0, j));
            vis1[0][j] = true;
            q2.push_back((m - 1, j));
            vis2[m - 1][j] = true;
        }

        let bfs = |q: &mut VecDeque<(usize, usize)>, vis: &mut Vec<Vec<bool>>| {
            while let Some((x, y)) = q.pop_front() {
                for k in 0..4 {
                    let nx = x as i32 + dirs[k];
                    let ny = y as i32 + dirs[k + 1];
                    if nx >= 0
                        && nx < m as i32
                        && ny >= 0
                        && ny < n as i32
                        && !vis[nx as usize][ny as usize]
                        && heights[nx as usize][ny as usize] >= heights[x][y]
                    {
                        vis[nx as usize][ny as usize] = true;
                        q.push_back((nx as usize, ny as usize));
                    }
                }
            }
        };

        bfs(&mut q1, &mut vis1);
        bfs(&mut q2, &mut vis2);

        let mut ans = vec![];
        for i in 0..m {
            for j in 0..n {
                if vis1[i][j] && vis2[i][j] {
                    ans.push(vec![i as i32, j as i32]);
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
