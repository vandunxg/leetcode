---
comments: true
difficulty: Medium
rating: 2153
source: Weekly Contest 357 Q3
tags:
    - Breadth-First Search
    - Union Find
    - Array
    - Binary Search
    - Matrix
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2812. Find the Safest Path in a Grid](https://leetcode.com/problems/find-the-safest-path-in-a-grid)

[中文文档](/solution/2800-2899/2812.Find%20the%20Safest%20Path%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một ma trận 2D <code>grid</code> có kích thước <code>n x n</code>, đánh chỉ số từ <strong>0</strong>, trong đó <code>(r, c)</code> biểu diễn:</p>

<ul>
	<li>Một ô chứa tên trộm nếu <code>grid[r][c] = 1</code></li>
	<li>Một ô trống nếu <code>grid[r][c] = 0</code></li>
</ul>

<p>Ban đầu bạn đứng tại ô <code>(0, 0)</code>. Trong một bước di chuyển, bạn có thể đi đến bất kỳ ô kề nào trong ma trận, kể cả các ô chứa tên trộm.</p>

<p><strong>Hệ số an toàn</strong> của một đường đi trong ma trận được định nghĩa là <strong>khoảng cách Manhattan nhỏ nhất</strong> từ bất kỳ ô nào trên đường đi đến bất kỳ tên trộm nào trong ma trận.</p>

<p>Trả về <em><strong>hệ số an toàn lớn nhất</strong> trong tất cả các đường đi đến ô </em><code>(n - 1, n - 1)</code><em>.</em></p>

<p>Một ô <strong>kề</strong> với ô <code>(r, c)</code> là một trong các ô <code>(r, c + 1)</code>, <code>(r, c - 1)</code>, <code>(r + 1, c)</code> và <code>(r - 1, c)</code> nếu ô đó tồn tại.</p>

<p><strong>Khoảng cách Manhattan</strong> giữa hai ô <code>(a, b)</code> và <code>(x, y)</code> bằng <code>|a - x| + |b - y|</code>, trong đó <code>|val|</code> biểu diễn giá trị tuyệt đối của val.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2812.Find%20the%20Safest%20Path%20in%20a%20Grid/images/example1.png" style="width: 362px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[1,0,0],[0,0,0],[0,0,1]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mọi đường đi từ (0, 0) đến (n - 1, n - 1) đều đi qua các tên trộm ở các ô (0, 0) và (n - 1, n - 1).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2812.Find%20the%20Safest%20Path%20in%20a%20Grid/images/example2.png" style="width: 362px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,0,1],[0,0,0],[0,0,0]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đường đi được minh họa trong hình trên có hệ số an toàn là 2 vì:
- Ô gần tên trộm nhất tại ô (0, 2) là ô (0, 0). Khoảng cách giữa chúng là | 0 - 0 | + | 0 - 2 | = 2.
Có thể chứng minh rằng không có đường đi nào khác có hệ số an toàn lớn hơn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2812.Find%20the%20Safest%20Path%20in%20a%20Grid/images/example3.png" style="width: 362px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,0,0,1],[0,0,0,0],[0,0,0,0],[1,0,0,0]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đường đi được minh họa trong hình trên có hệ số an toàn là 2 vì:
- Ô gần tên trộm nhất tại ô (0, 3) là ô (1, 2). Khoảng cách giữa chúng là | 0 - 1 | + | 3 - 2 | = 2.
- Ô gần tên trộm nhất tại ô (3, 0) là ô (3, 2). Khoảng cách giữa chúng là | 3 - 3 | + | 0 - 2 | = 2.
Có thể chứng minh rằng không có đường đi nào khác có hệ số an toàn lớn hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= grid.length == n &lt;= 400</code></li>
	<li><code>grid[i].length == n</code></li>
	<li><code>grid[i][j]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li>Trong <code>grid</code> có ít nhất một tên trộm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS + Sorting + Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Độ an toàn của một đường đi là khoảng cách nhỏ nhất từ các ô trên đường đi đến tên trộm, và ta muốn giá trị lớn nhất của đại lượng này. Multi-source BFS từ tất cả tên trộm cho khoảng cách đến tên trộm của từng ô. Thêm các ô theo thứ tự khoảng cách giảm dần vào cấu trúc union-find, lần đầu tiên điểm đầu và điểm cuối được nối với nhau thì đó là đáp án.

<!-- thinking:end -->

Trước tiên, ta có thể tìm vị trí của tất cả tên trộm, sau đó bắt đầu multi-source BFS từ các vị trí này để tìm khoảng cách ngắn nhất từ mỗi vị trí đến tên trộm. Tiếp theo, sắp xếp theo khoảng cách giảm dần rồi lần lượt thêm từng vị trí vào tập union-find. Nếu điểm đầu và điểm cuối nằm trong cùng một thành phần liên thông, khoảng cách hiện tại chính là đáp án.

Độ phức tạp thời gian là $O(n^2 \times \log n)$, và độ phức tạp không gian là $O(n^2)$. Trong đó $n$ là kích thước của ma trận.

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a, b):
        pa, pb = self.find(a), self.find(b)
        if pa == pb:
            return False
        if self.size[pa] > self.size[pb]:
            self.p[pb] = pa
            self.size[pa] += self.size[pb]
        else:
            self.p[pa] = pb
            self.size[pb] += self.size[pa]
        return True


class Solution:
    def maximumSafenessFactor(self, grid: List[List[int]]) -> int:
        n = len(grid)
        if grid[0][0] or grid[n - 1][n - 1]:
            return 0
        q = deque()
        dist = [[inf] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                if grid[i][j]:
                    q.append((i, j))
                    dist[i][j] = 0
        dirs = (-1, 0, 1, 0, -1)
        while q:
            i, j = q.popleft()
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < n and 0 <= y < n and dist[x][y] == inf:
                    dist[x][y] = dist[i][j] + 1
                    q.append((x, y))

        q = ((dist[i][j], i, j) for i in range(n) for j in range(n))
        q = sorted(q, reverse=True)
        uf = UnionFind(n * n)
        for d, i, j in q:
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if 0 <= x < n and 0 <= y < n and dist[x][y] >= d:
                    uf.union(i * n + j, x * n + y)
            if uf.find(0) == uf.find(n * n - 1):
                return int(d)
        return 0
```

#### Java

```java
class Solution {
    public int maximumSafenessFactor(List<List<Integer>> grid) {
        int n = grid.size();
        if (grid.get(0).get(0) == 1 || grid.get(n - 1).get(n - 1) == 1) {
            return 0;
        }
        Deque<int[]> q = new ArrayDeque<>();
        int[][] dist = new int[n][n];
        final int inf = 1 << 30;
        for (int[] d : dist) {
            Arrays.fill(d, inf);
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid.get(i).get(j) == 1) {
                    dist[i][j] = 0;
                    q.offer(new int[] {i, j});
                }
            }
        }
        int[] dirs = {-1, 0, 1, 0, -1};
        while (!q.isEmpty()) {
            int[] p = q.poll();
            int i = p[0], j = p[1];
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < n && dist[x][y] == inf) {
                    dist[x][y] = dist[i][j] + 1;
                    q.offer(new int[] {x, y});
                }
            }
        }
        List<int[]> t = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                t.add(new int[] {dist[i][j], i, j});
            }
        }
        t.sort((a, b) -> Integer.compare(b[0], a[0]));
        UnionFind uf = new UnionFind(n * n);
        for (int[] p : t) {
            int d = p[0], i = p[1], j = p[2];
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < n && dist[x][y] >= d) {
                    uf.union(i * n + j, x * n + y);
                }
            }
            if (uf.find(0) == uf.find(n * n - 1)) {
                return d;
            }
        }
        return 0;
    }
}

class UnionFind {
    public int[] p;
    public int n;

    public UnionFind(int n) {
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        this.n = n;
    }

    public boolean union(int a, int b) {
        int pa = find(a);
        int pb = find(b);
        if (pa == pb) {
            return false;
        }
        p[pa] = pb;
        --n;
        return true;
    }

    public int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    vector<int> p;
    int n;

    UnionFind(int _n)
        : n(_n)
        , p(_n) {
        iota(p.begin(), p.end(), 0);
    }

    bool unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) return false;
        p[pa] = pb;
        --n;
        return true;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
};

class Solution {
public:
    int maximumSafenessFactor(vector<vector<int>>& grid) {
        int n = grid.size();
        if (grid[0][0] || grid[n - 1][n - 1]) {
            return 0;
        }
        queue<pair<int, int>> q;
        int dist[n][n];
        memset(dist, 0x3f, sizeof(dist));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    dist[i][j] = 0;
                    q.emplace(i, j);
                }
            }
        }
        int dirs[5] = {-1, 0, 1, 0, -1};
        while (!q.empty()) {
            auto [i, j] = q.front();
            q.pop();
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < n && dist[x][y] == 0x3f3f3f3f) {
                    dist[x][y] = dist[i][j] + 1;
                    q.emplace(x, y);
                }
            }
        }
        vector<tuple<int, int, int>> t;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                t.emplace_back(dist[i][j], i, j);
            }
        }
        sort(t.begin(), t.end());
        reverse(t.begin(), t.end());
        UnionFind uf(n * n);
        for (auto [d, i, j] : t) {
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < n && y >= 0 && y < n && dist[x][y] >= d) {
                    uf.unite(i * n + j, x * n + y);
                }
            }
            if (uf.find(0) == uf.find(n * n - 1)) {
                return d;
            }
        }
        return 0;
    }
};
```

#### Go

```go
type unionFind struct {
	p []int
	n int
}

func newUnionFind(n int) *unionFind {
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	return &unionFind{p, n}
}

func (uf *unionFind) find(x int) int {
	if uf.p[x] != x {
		uf.p[x] = uf.find(uf.p[x])
	}
	return uf.p[x]
}

func (uf *unionFind) union(a, b int) bool {
	if uf.find(a) == uf.find(b) {
		return false
	}
	uf.p[uf.find(a)] = uf.find(b)
	uf.n--
	return true
}

func maximumSafenessFactor(grid [][]int) int {
	n := len(grid)
	if grid[0][0] == 1 || grid[n-1][n-1] == 1 {
		return 0
	}
	q := [][2]int{}
	dist := make([][]int, n)
	const inf = 1 << 30
	for i := range dist {
		dist[i] = make([]int, n)
		for j := range dist[i] {
			dist[i][j] = inf
		}
	}
	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == 1 {
				dist[i][j] = 0
				q = append(q, [2]int{i, j})
			}
		}
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		i, j := p[0], p[1]
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < n && y >= 0 && y < n && dist[x][y] == inf {
				dist[x][y] = dist[i][j] + 1
				q = append(q, [2]int{x, y})
			}
		}
	}
	t := [][3]int{}
	for i := 0; i < n; i++ {
		for j := 0; j < n; j++ {
			t = append(t, [3]int{dist[i][j], i, j})
		}
	}
	sort.Slice(t, func(i, j int) bool {
		return t[i][0] > t[j][0]
	})
	uf := newUnionFind(n * n)
	for _, p := range t {
		d, i, j := p[0], p[1], p[2]
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < n && y >= 0 && y < n && dist[x][y] >= d {
				uf.union(i*n+j, x*n+y)
			}
		}
		if uf.find(0) == uf.find(n*n-1) {
			return d
		}
	}
	return 0
}
```

#### TypeScript

```ts
class UnionFind {
    private p: number[];
    private n: number;

    constructor(n: number) {
        this.n = n;
        this.p = Array(n)
            .fill(0)
            .map((_, i) => i);
    }

    find(x: number): number {
        if (this.p[x] !== x) {
            this.p[x] = this.find(this.p[x]);
        }
        return this.p[x];
    }

    union(a: number, b: number): boolean {
        const pa = this.find(a);
        const pb = this.find(b);
        if (pa !== pb) {
            this.p[pa] = pb;
            this.n--;
            return true;
        }
        return false;
    }
}

function maximumSafenessFactor(grid: number[][]): number {
    const n = grid.length;
    if (grid[0][0] === 1 || grid[n - 1][n - 1] === 1) {
        return 0;
    }
    const q: number[][] = [];
    const inf = 1 << 30;
    const dist: number[][] = Array(n)
        .fill(0)
        .map(() => Array(n).fill(inf));
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 1) {
                dist[i][j] = 0;
                q.push([i, j]);
            }
        }
    }
    const dirs = [-1, 0, 1, 0, -1];
    while (q.length) {
        const [i, j] = q.shift()!;
        for (let k = 0; k < 4; ++k) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x >= 0 && x < n && y >= 0 && y < n && dist[x][y] === inf) {
                dist[x][y] = dist[i][j] + 1;
                q.push([x, y]);
            }
        }
    }
    const t: number[][] = [];
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            t.push([dist[i][j], i, j]);
        }
    }
    t.sort((a, b) => b[0] - a[0]);
    const uf = new UnionFind(n * n);
    for (const [d, i, j] of t) {
        for (let k = 0; k < 4; ++k) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x >= 0 && x < n && y >= 0 && y < n && dist[x][y] >= d) {
                uf.union(i * n + j, x * n + y);
            }
        }
        if (uf.find(0) == uf.find(n * n - 1)) {
            return d;
        }
    }
    return 0;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn maximum_safeness_factor(grid: Vec<Vec<i32>>) -> i32 {
        let n = grid.len();
        if grid[0][0] == 1 || grid[n - 1][n - 1] == 1 {
            return 0;
        }

        let inf: i32 = 1 << 30;
        let mut dist = vec![vec![inf; n]; n];
        let mut q = VecDeque::new();

        for i in 0..n {
            for j in 0..n {
                if grid[i][j] == 1 {
                    dist[i][j] = 0;
                    q.push_back((i as i32, j as i32));
                }
            }
        }

        let dirs = [-1, 0, 1, 0, -1];

        while let Some((i, j)) = q.pop_front() {
            for k in 0..4 {
                let x = i + dirs[k];
                let y = j + dirs[k + 1];
                if x >= 0 && x < n as i32 && y >= 0 && y < n as i32 {
                    let (x, y) = (x as usize, y as usize);
                    let (i, j) = (i as usize, j as usize);
                    if dist[x][y] == inf {
                        dist[x][y] = dist[i][j] + 1;
                        q.push_back((x as i32, y as i32));
                    }
                }
            }
        }

        let mut t: Vec<(i32, usize, usize)> = Vec::new();
        for i in 0..n {
            for j in 0..n {
                t.push((dist[i][j], i, j));
            }
        }

        t.sort_by(|a, b| b.0.cmp(&a.0));

        let mut uf = UnionFind::new(n * n);
        let dirs = [-1, 0, 1, 0, -1];

        for (d, i, j) in t {
            for k in 0..4 {
                let x = i as i32 + dirs[k];
                let y = j as i32 + dirs[k + 1];
                if x >= 0 && x < n as i32 && y >= 0 && y < n as i32 {
                    let (x, y) = (x as usize, y as usize);
                    if dist[x][y] >= d {
                        uf.union(i * n + j, x * n + y);
                    }
                }
            }

            if uf.find(0) == uf.find(n * n - 1) {
                return d;
            }
        }

        0
    }
}

struct UnionFind {
    p: Vec<usize>,
}

impl UnionFind {
    fn new(n: usize) -> Self {
        let mut p = vec![0; n];
        for i in 0..n {
            p[i] = i;
        }
        Self { p }
    }

    fn find(&mut self, x: usize) -> usize {
        if self.p[x] != x {
            self.p[x] = self.find(self.p[x]);
        }
        self.p[x]
    }

    fn union(&mut self, a: usize, b: usize) -> bool {
        let pa = self.find(a);
        let pb = self.find(b);
        if pa == pb {
            return false;
        }
        self.p[pa] = pb;
        true
    }
}
```

#### C#

```cs
public class Solution {
    public int MaximumSafenessFactor(IList<IList<int>> grid) {
        int n = grid.Count;
        if (grid[0][0] == 1 || grid[n - 1][n - 1] == 1) {
            return 0;
        }

        int inf = 1 << 30;
        int[,] dist = new int[n, n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                dist[i, j] = inf;
            }
        }

        Queue<(int x, int y)> q = new Queue<(int x, int y)>();

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    dist[i, j] = 0;
                    q.Enqueue((i, j));
                }
            }
        }

        int[] dirs = new int[] { -1, 0, 1, 0, -1 };

        while (q.Count > 0) {
            var (i, j) = q.Dequeue();

            for (int k = 0; k < 4; k++) {
                int x = i + dirs[k];
                int y = j + dirs[k + 1];

                if (x >= 0 && x < n && y >= 0 && y < n && dist[x, y] == inf) {
                    dist[x, y] = dist[i, j] + 1;
                    q.Enqueue((x, y));
                }
            }
        }

        List<(int d, int i, int j)> t = new List<(int d, int i, int j)>();

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                t.Add((dist[i, j], i, j));
            }
        }

        t.Sort((a, b) => b.d.CompareTo(a.d));

        UnionFind uf = new UnionFind(n * n);

        foreach (var (d, i, j) in t) {
            for (int k = 0; k < 4; k++) {
                int x = i + dirs[k];
                int y = j + dirs[k + 1];

                if (x >= 0 && x < n && y >= 0 && y < n && dist[x, y] >= d) {
                    uf.Union(i * n + j, x * n + y);
                }
            }

            if (uf.Find(0) == uf.Find(n * n - 1)) {
                return d;
            }
        }

        return 0;
    }
}

public class UnionFind {
    public int[] p;

    public UnionFind(int n) {
        p = new int[n];
        for (int i = 0; i < n; i++) {
            p[i] = i;
        }
    }

    public int Find(int x) {
        if (p[x] != x) {
            p[x] = Find(p[x]);
        }
        return p[x];
    }

    public bool Union(int a, int b) {
        int pa = Find(a);
        int pb = Find(b);
        if (pa == pb) {
            return false;
        }
        p[pa] = pb;
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
