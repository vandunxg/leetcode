---
comments: true
difficulty: Hard
rating: 2123
source: Weekly Contest 254 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Binary Search
    - Matrix
---

<!-- problem:start -->

# [1970. Last Day Where You Can Still Cross](https://leetcode.com/problems/last-day-where-you-can-still-cross)

[中文文档](/solution/1900-1999/1970.Last%20Day%20Where%20You%20Can%20Still%20Cross/README.md)

## Mô tả

<!-- description:start -->

<p>Có một ma trận nhị phân được đánh chỉ số từ <strong>1</strong>, trong đó <code>0</code> biểu thị đất và <code>1</code> biểu thị nước. Cho hai số nguyên <code>row</code> và <code>col</code> lần lượt biểu thị số hàng và số cột của ma trận.</p>

<p>Ban đầu, vào ngày <code>0</code>, <strong>toàn bộ</strong> ma trận là <strong>đất</strong>. Tuy nhiên, mỗi ngày sẽ có thêm một ô bị ngập thành <strong>nước</strong>. Cho mảng hai chiều <code>cells</code> được đánh chỉ số từ <strong>1</strong>, trong đó <code>cells[i] = [r<sub>i</sub>, c<sub>i</sub>]</code> biểu thị rằng vào ngày thứ <code>i<sup>th</sup></code>, ô ở hàng thứ <code>r<sub>i</sub><sup>th</sup></code> và cột thứ <code>c<sub>i</sub><sup>th</sup></code> (tọa độ được đánh chỉ số từ <strong>1</strong>) sẽ bị phủ bởi <strong>nước</strong> (tức là chuyển thành <code>1</code>).</p>

<p>Bạn cần tìm <strong>ngày cuối cùng</strong> mà vẫn có thể đi bộ từ <strong>hàng trên cùng</strong> xuống <strong>hàng dưới cùng</strong> chỉ bằng cách đi qua các ô đất. Bạn có thể bắt đầu từ <strong>bất kỳ</strong> ô nào ở hàng trên cùng và kết thúc tại <strong>bất kỳ</strong> ô nào ở hàng dưới cùng. Bạn chỉ được di chuyển theo<strong> bốn</strong> hướng chính (trái, phải, lên và xuống).</p>

<p>Trả về <em><strong>ngày cuối cùng</strong> mà vẫn có thể đi bộ từ <strong>hàng trên cùng</strong> xuống <strong>hàng dưới cùng</strong> chỉ bằng cách đi qua các ô đất</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1970.Last%20Day%20Where%20You%20Can%20Still%20Cross/images/1.png" style="width: 624px; height: 162px;" />
<pre>
<strong>Đầu vào:</strong> row = 2, col = 2, cells = [[1,1],[2,1],[1,2],[2,2]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hình ảnh trên mô tả cách ma trận thay đổi mỗi ngày, bắt đầu từ ngày 0.
Ngày cuối cùng mà vẫn có thể đi từ trên xuống dưới là ngày 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1970.Last%20Day%20Where%20You%20Can%20Still%20Cross/images/2.png" style="width: 504px; height: 178px;" />
<pre>
<strong>Đầu vào:</strong> row = 2, col = 2, cells = [[1,1],[1,2],[2,1],[2,2]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Hình ảnh trên mô tả cách ma trận thay đổi mỗi ngày, bắt đầu từ ngày 0.
Ngày cuối cùng mà vẫn có thể đi từ trên xuống dưới là ngày 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1970.Last%20Day%20Where%20You%20Can%20Still%20Cross/images/3.png" style="width: 666px; height: 167px;" />
<pre>
<strong>Đầu vào:</strong> row = 3, col = 3, cells = [[1,2],[2,1],[3,3],[2,2],[1,1],[1,3],[2,3],[3,2],[3,1]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Hình ảnh trên mô tả cách ma trận thay đổi mỗi ngày, bắt đầu từ ngày 0.
Ngày cuối cùng mà vẫn có thể đi từ trên xuống dưới là ngày 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= row, col &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>4 &lt;= row * col &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>cells.length == row * col</code></li>
	<li><code>1 &lt;= r<sub>i</sub> &lt;= row</code></li>
	<li><code>1 &lt;= c<sub>i</sub> &lt;= col</code></li>
	<li>Tất cả các giá trị trong <code>cells</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Tính chất có thể đi từ hàng trên cùng xuống hàng dưới cùng vào ngày nào đó là đơn điệu: nếu ngày $k$ thỏa mãn thì mọi ngày trước đó cũng thỏa mãn. Vì vậy, ta tìm kiếm nhị phân $k$ và dùng BFS trên các ô đất còn lại.
>
> Với $k$ ở giữa, ta làm ngập $k$ ô đầu tiên, bắt đầu từ mọi ô khô ở hàng $0$, rồi kiểm tra xem có đến được hàng $row-1$ hay không.
>
> Mỗi lần kiểm tra chạy tuyến tính theo kích thước lưới; số lần kiểm tra là logarit.

<!-- thinking:end -->

Ta nhận thấy rằng nếu có thể đi từ hàng trên cùng xuống hàng dưới cùng vào ngày $k$, thì với mọi $0 < k' < k$, ta cũng có thể đi từ hàng trên cùng xuống hàng dưới cùng vào ngày $k'$. Đây là tính chất đơn điệu, nên ta có thể dùng tìm kiếm nhị phân để tìm $k$ lớn nhất sao cho vẫn có thể đi từ hàng trên cùng xuống hàng dưới cùng vào ngày $k$.

Ta đặt biên trái của khoảng tìm kiếm nhị phân là $l = 1$ và biên phải là $r = |cells|$, trong đó $|cells|$ là độ dài của mảng $\textit{cells}$. Sau đó, ta tìm kiếm nhị phân trên $k$. Với mỗi $k$, ta lấy $k$ phần tử đầu tiên của $\textit{cells}$, biến các ô tương ứng thành nước, rồi dùng tìm kiếm theo chiều rộng (BFS) để thử đi từ hàng trên cùng xuống hàng dưới cùng. Nếu đến được hàng dưới cùng, nghĩa là có thể đi từ hàng trên cùng xuống hàng dưới cùng vào ngày $k$, nên ta cập nhật biên trái $l$ thành $k$. Ngược lại, ta cập nhật biên phải $r$ thành $k - 1$.

Độ phức tạp thời gian là $O(m \times n \times \log (m \times n))$, còn độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def latestDayToCross(self, row: int, col: int, cells: List[List[int]]) -> int:
        def check(k: int) -> bool:
            g = [[0] * col for _ in range(row)]
            for i, j in cells[:k]:
                g[i - 1][j - 1] = 1
            q = [(0, j) for j in range(col) if g[0][j] == 0]
            for x, y in q:
                if x == row - 1:
                    return True
                for a, b in pairwise(dirs):
                    nx, ny = x + a, y + b
                    if 0 <= nx < row and 0 <= ny < col and g[nx][ny] == 0:
                        q.append((nx, ny))
                        g[nx][ny] = 1
            return False

        n = row * col
        l, r = 1, n
        dirs = (-1, 0, 1, 0, -1)
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    private int[][] cells;
    private int m;
    private int n;

    public int latestDayToCross(int row, int col, int[][] cells) {
        int l = 1, r = cells.length;
        this.cells = cells;
        this.m = row;
        this.n = col;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int k) {
        int[][] g = new int[m][n];
        for (int i = 0; i < k; i++) {
            g[cells[i][0] - 1][cells[i][1] - 1] = 1;
        }
        final int[] dirs = {-1, 0, 1, 0, -1};
        Deque<int[]> q = new ArrayDeque<>();
        for (int j = 0; j < n; j++) {
            if (g[0][j] == 0) {
                q.offer(new int[] {0, j});
                g[0][j] = 1;
            }
        }
        while (!q.isEmpty()) {
            int[] p = q.poll();
            int x = p[0], y = p[1];
            if (x == m - 1) {
                return true;
            }
            for (int i = 0; i < 4; i++) {
                int nx = x + dirs[i], ny = y + dirs[i + 1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && g[nx][ny] == 0) {
                    q.offer(new int[] {nx, ny});
                    g[nx][ny] = 1;
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int latestDayToCross(int row, int col, vector<vector<int>>& cells) {
        int l = 1, r = cells.size();
        int g[row][col];
        int dirs[5] = {0, 1, 0, -1, 0};
        auto check = [&](int k) -> bool {
            memset(g, 0, sizeof(g));
            for (int i = 0; i < k; ++i) {
                g[cells[i][0] - 1][cells[i][1] - 1] = 1;
            }
            queue<pair<int, int>> q;
            for (int j = 0; j < col; ++j) {
                if (g[0][j] == 0) {
                    q.emplace(0, j);
                    g[0][j] = 1;
                }
            }
            while (!q.empty()) {
                auto [x, y] = q.front();
                q.pop();
                if (x == row - 1) {
                    return true;
                }
                for (int i = 0; i < 4; ++i) {
                    int nx = x + dirs[i];
                    int ny = y + dirs[i + 1];
                    if (nx >= 0 && nx < row && ny >= 0 && ny < col && g[nx][ny] == 0) {
                        q.emplace(nx, ny);
                        g[nx][ny] = 1;
                    }
                }
            }
            return false;
        };
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func latestDayToCross(row int, col int, cells [][]int) int {
	l, r := 1, len(cells)
	dirs := [5]int{-1, 0, 1, 0, -1}
	check := func(k int) bool {
		g := make([][]int, row)
		for i := range g {
			g[i] = make([]int, col)
		}
		for i := 0; i < k; i++ {
			g[cells[i][0]-1][cells[i][1]-1] = 1
		}
		q := [][2]int{}
		for j := 0; j < col; j++ {
			if g[0][j] == 0 {
				g[0][j] = 1
				q = append(q, [2]int{0, j})
			}
		}
		for len(q) > 0 {
			x, y := q[0][0], q[0][1]
			q = q[1:]
			if x == row-1 {
				return true
			}
			for i := 0; i < 4; i++ {
				nx, ny := x+dirs[i], y+dirs[i+1]
				if nx >= 0 && nx < row && ny >= 0 && ny < col && g[nx][ny] == 0 {
					g[nx][ny] = 1
					q = append(q, [2]int{nx, ny})
				}
			}
		}
		return false
	}
	for l < r {
		mid := (l + r + 1) >> 1
		if check(mid) {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function latestDayToCross(row: number, col: number, cells: number[][]): number {
    let [l, r] = [1, cells.length];
    const check = (k: number): boolean => {
        const g: number[][] = Array.from({ length: row }, () => Array(col).fill(0));
        for (let i = 0; i < k; ++i) {
            const [x, y] = cells[i];
            g[x - 1][y - 1] = 1;
        }
        const q: number[][] = [];
        for (let j = 0; j < col; ++j) {
            if (g[0][j] === 0) {
                q.push([0, j]);
                g[0][j] = 1;
            }
        }
        const dirs: number[] = [-1, 0, 1, 0, -1];
        for (const [x, y] of q) {
            if (x === row - 1) {
                return true;
            }
            for (let i = 0; i < 4; ++i) {
                const nx = x + dirs[i];
                const ny = y + dirs[i + 1];
                if (nx >= 0 && nx < row && ny >= 0 && ny < col && g[nx][ny] === 0) {
                    q.push([nx, ny]);
                    g[nx][ny] = 1;
                }
            }
        }
        return false;
    };
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn latest_day_to_cross(row: i32, col: i32, cells: Vec<Vec<i32>>) -> i32 {
        let mut l: i32 = 1;
        let mut r: i32 = cells.len() as i32;
        let m = row as usize;
        let n = col as usize;

        let check = |k: i32, cells: &Vec<Vec<i32>>| -> bool {
            let mut g = vec![vec![0i32; n]; m];
            for i in 0..k as usize {
                let x = (cells[i][0] - 1) as usize;
                let y = (cells[i][1] - 1) as usize;
                g[x][y] = 1;
            }
            let dirs = [-1, 0, 1, 0, -1];
            let mut q: VecDeque<(usize, usize)> = VecDeque::new();
            for j in 0..n {
                if g[0][j] == 0 {
                    q.push_back((0, j));
                    g[0][j] = 1;
                }
            }
            while let Some((x, y)) = q.pop_front() {
                if x == m - 1 {
                    return true;
                }
                for i in 0..4 {
                    let nx = x as i32 + dirs[i];
                    let ny = y as i32 + dirs[i + 1];
                    if nx >= 0
                        && nx < m as i32
                        && ny >= 0
                        && ny < n as i32
                        && g[nx as usize][ny as usize] == 0
                    {
                        q.push_back((nx as usize, ny as usize));
                        g[nx as usize][ny as usize] = 1;
                    }
                }
            }
            false
        };

        while l < r {
            let mid = (l + r + 1) >> 1;
            if check(mid, &cells) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        l
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Việc xây dựng lại lưới ở mỗi lần thử giữa là lãng phí. Ta xử lý các ô theo thứ tự ngược, gộp các ô đất kề nhau, đồng thời nối thêm hai node ảo đại diện cho hàng trên cùng và hàng dưới cùng.
>
> Lần đầu tiên hai node đặc biệt này thuộc cùng một component, ngày ngay trước đó là ngày cuối cùng còn có thể đi qua. Nhờ vậy, một lượt duyệt tuyến tính có thể thay thế các lần tìm kiếm nhị phân.

<!-- thinking:end -->

Trước tiên, ta có thể khởi tạo tất cả các ô đất bằng $1$, sau đó duyệt mảng $\textit{cells}$ theo thứ tự ngược, chuyển từng ô tương ứng thành $0$ và gộp nó với các ô đất kề nhau (lên, xuống, trái, phải). Ta cũng cần duy trì hai node ảo $s$ và $t$, lần lượt đại diện cho hàng trên cùng và hàng dưới cùng. Nếu $s$ và $t$ được kết nối trong tập union-find, điều đó có nghĩa là vào ngày $i$ ta có thể đi từ hàng trên cùng xuống hàng dưới cùng.

Độ phức tạp thời gian là $O(m \times n \times \alpha(m \times n))$, còn độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số hàng và số cột của ma trận, còn $\alpha$ là hàm Ackermann nghịch đảo.

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
    def latestDayToCross(self, row: int, col: int, cells: List[List[int]]) -> int:
        mn = len(cells)
        uf = UnionFind(mn + 2)
        s, t = mn, mn + 1
        dirs = (-1, 0, 1, 0, -1)
        g = [[1] * col for _ in range(row)]
        for i in range(mn - 1, -1, -1):
            x, y = cells[i][0] - 1, cells[i][1] - 1
            g[x][y] = 0
            for a, b in pairwise(dirs):
                nx, ny = x + a, y + b
                if 0 <= nx < row and 0 <= ny < col and g[nx][ny] == 0:
                    uf.union(x * col + y, nx * col + ny)
            if x == 0:
                uf.union(y, s)
            if x == row - 1:
                uf.union(x * col + y, t)
            if uf.find(s) == uf.find(t):
                return i
```

#### Java

```java
class UnionFind {
    private final int[] p;
    private final int[] size;

    public UnionFind(int n) {
        p = new int[n];
        size = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            size[i] = 1;
        }
    }

    public int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    public boolean union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
    }
}

class Solution {
    public int latestDayToCross(int row, int col, int[][] cells) {
        int mn = cells.length;
        UnionFind uf = new UnionFind(mn + 2);
        int s = mn, t = mn + 1;
        int[][] g = new int[row][col];
        for (var e : g) {
            Arrays.fill(e, 1);
        }
        final int[] dirs = {-1, 0, 1, 0, -1};
        for (int i = mn - 1;; --i) {
            int x = cells[i][0] - 1, y = cells[i][1] - 1;
            g[x][y] = 0;
            for (int j = 0; j < 4; ++j) {
                int nx = x + dirs[j], ny = y + dirs[j + 1];
                if (nx >= 0 && nx < row && ny >= 0 && ny < col && g[nx][ny] == 0) {
                    uf.union(x * col + y, nx * col + ny);
                }
            }
            if (x == 0) {
                uf.union(s, x * col + y);
            }
            if (x == row - 1) {
                uf.union(t, x * col + y);
            }
            if (uf.find(s) == uf.find(t)) {
                return i;
            }
        }
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    UnionFind(int n) {
        p = vector<int>(n);
        size = vector<int>(n, 1);
        iota(p.begin(), p.end(), 0);
    }

    bool unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
    }

    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

private:
    vector<int> p, size;
};

class Solution {
public:
    int latestDayToCross(int row, int col, vector<vector<int>>& cells) {
        int mn = cells.size();
        UnionFind uf(mn + 2);
        int s = mn, t = mn + 1;
        vector<vector<int>> g(row, vector<int>(col, 1));
        const int dirs[5] = {0, 1, 0, -1, 0};
        for (int i = mn - 1;; --i) {
            int x = cells[i][0] - 1, y = cells[i][1] - 1;
            g[x][y] = 0;
            for (int j = 0; j < 4; ++j) {
                int nx = x + dirs[j], ny = y + dirs[j + 1];
                if (nx >= 0 && nx < row && ny >= 0 && ny < col && g[nx][ny] == 0) {
                    uf.unite(x * col + y, nx * col + ny);
                }
            }
            if (x == 0) {
                uf.unite(s, x * col + y);
            }
            if (x == row - 1) {
                uf.unite(t, x * col + y);
            }
            if (uf.find(s) == uf.find(t)) {
                return i;
            }
        }
    }
};
```

#### Go

```go
type unionFind struct {
	p, size []int
}

func newUnionFind(n int) *unionFind {
	p := make([]int, n)
	size := make([]int, n)
	for i := range p {
		p[i] = i
		size[i] = 1
	}
	return &unionFind{p, size}
}

func (uf *unionFind) find(x int) int {
	if uf.p[x] != x {
		uf.p[x] = uf.find(uf.p[x])
	}
	return uf.p[x]
}

func (uf *unionFind) union(a, b int) bool {
	pa, pb := uf.find(a), uf.find(b)
	if pa == pb {
		return false
	}
	if uf.size[pa] > uf.size[pb] {
		uf.p[pb] = pa
		uf.size[pa] += uf.size[pb]
	} else {
		uf.p[pa] = pb
		uf.size[pb] += uf.size[pa]
	}
	return true
}

func latestDayToCross(row int, col int, cells [][]int) int {
	mn := len(cells)
	uf := newUnionFind(mn + 2)
	s, t := mn, mn+1
	g := make([][]int, row)
	for i := range g {
		g[i] = make([]int, col)
		for j := range g[i] {
			g[i][j] = 1
		}
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	for i := mn - 1; ; i-- {
		x, y := cells[i][0]-1, cells[i][1]-1
		g[x][y] = 0
		for j := 0; j < 4; j++ {
			nx, ny := x+dirs[j], y+dirs[j+1]
			if nx >= 0 && nx < row && ny >= 0 && ny < col && g[nx][ny] == 0 {
				uf.union(x*col+y, nx*col+ny)
			}
		}
		if x == 0 {
			uf.union(s, x*col+y)
		}
		if x == row-1 {
			uf.union(t, x*col+y)
		}
		if uf.find(s) == uf.find(t) {
			return i
		}
	}
}
```

#### TypeScript

```ts
class UnionFind {
    p: number[];
    size: number[];
    constructor(n: number) {
        this.p = Array(n)
            .fill(0)
            .map((_, i) => i);
        this.size = Array(n).fill(1);
    }

    find(x: number): number {
        if (this.p[x] !== x) {
            this.p[x] = this.find(this.p[x]);
        }
        return this.p[x];
    }

    union(a: number, b: number): boolean {
        const [pa, pb] = [this.find(a), this.find(b)];
        if (pa === pb) {
            return false;
        }
        if (this.size[pa] > this.size[pb]) {
            this.p[pb] = pa;
            this.size[pa] += this.size[pb];
        } else {
            this.p[pa] = pb;
            this.size[pb] += this.size[pa];
        }
        return true;
    }
}

function latestDayToCross(row: number, col: number, cells: number[][]): number {
    const mn = cells.length;
    const uf = new UnionFind(row * col + 2);
    const [s, t] = [mn, mn + 1];
    const g: number[][] = Array.from({ length: row }, () => Array(col).fill(1));
    const dirs: number[] = [-1, 0, 1, 0, -1];
    for (let i = mn - 1; ; --i) {
        const [x, y] = [cells[i][0] - 1, cells[i][1] - 1];
        g[x][y] = 0;
        for (let j = 0; j < 4; ++j) {
            const [nx, ny] = [x + dirs[j], y + dirs[j + 1]];
            if (nx >= 0 && nx < row && ny >= 0 && ny < col && g[nx][ny] === 0) {
                uf.union(x * col + y, nx * col + ny);
            }
        }
        if (x === 0) {
            uf.union(s, y);
        }
        if (x === row - 1) {
            uf.union(t, x * col + y);
        }
        if (uf.find(s) === uf.find(t)) {
            return i;
        }
    }
}
```

#### Rust

```rust
struct UnionFind {
    p: Vec<usize>,
    size: Vec<usize>,
}

impl UnionFind {
    fn new(n: usize) -> Self {
        let mut p = vec![0; n];
        let mut size = vec![1; n];
        for i in 0..n {
            p[i] = i;
        }
        Self { p, size }
    }

    fn find(&mut self, x: usize) -> usize {
        if self.p[x] != x {
            let px = self.p[x];
            self.p[x] = self.find(px);
        }
        self.p[x]
    }

    fn union(&mut self, a: usize, b: usize) -> bool {
        let pa = self.find(a);
        let pb = self.find(b);
        if pa == pb {
            return false;
        }
        if self.size[pa] > self.size[pb] {
            self.p[pb] = pa;
            self.size[pa] += self.size[pb];
        } else {
            self.p[pa] = pb;
            self.size[pb] += self.size[pa];
        }
        true
    }
}

impl Solution {
    pub fn latest_day_to_cross(row: i32, col: i32, cells: Vec<Vec<i32>>) -> i32 {
        let mn = cells.len();
        let mut uf = UnionFind::new(mn + 2);
        let s = mn;
        let t = mn + 1;
        let row = row as usize;
        let col = col as usize;

        let mut g = vec![vec![1i32; col]; row];
        let dirs = [-1, 0, 1, 0, -1];

        let mut i = mn as i32 - 1;
        loop {
            let x = (cells[i as usize][0] - 1) as usize;
            let y = (cells[i as usize][1] - 1) as usize;
            g[x][y] = 0;

            for j in 0..4 {
                let nx = x as i32 + dirs[j];
                let ny = y as i32 + dirs[j + 1];
                if nx >= 0
                    && nx < row as i32
                    && ny >= 0
                    && ny < col as i32
                    && g[nx as usize][ny as usize] == 0
                {
                    uf.union(x * col + y, nx as usize * col + ny as usize);
                }
            }

            if x == 0 {
                uf.union(s, x * col + y);
            }
            if x == row - 1 {
                uf.union(t, x * col + y);
            }
            if uf.find(s) == uf.find(t) {
                return i;
            }
            i -= 1;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
