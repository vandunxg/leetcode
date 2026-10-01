---
comments: true
difficulty: Hard
tags:
    - Union Find
    - Array
    - Hash Table
---

<!-- problem:start -->

# [305. Number of Islands II 🔒](https://leetcode.com/problems/number-of-islands-ii)

[中文文档](/solution/0300-0399/0305.Number%20of%20Islands%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho lưới nhị phân 2D rỗng <code>grid</code> kích thước <code>m x n</code>, biểu diễn một bản đồ trong đó <code>0</code> là nước và <code>1</code> là đất liền. Ban đầu, tất cả ô trong <code>grid</code> đều là nước (tức đều có giá trị <code>0</code>).</p>

<p>Ta có thể thực hiện thao tác thêm đất liền để biến ô nước tại một vị trí thành đất liền. Cho mảng <code>positions</code>, trong đó <code>positions[i] = [r<sub>i</sub>, c<sub>i</sub>]</code> là vị trí <code>(r<sub>i</sub>, c<sub>i</sub>)</code> mà thao tác thứ <code>i<sup>th</sup></code> sẽ được thực hiện.</p>

<p>Trả về <em>mảng số nguyên</em> <code>answer</code>, trong đó <code>answer[i]</code> <em>là số lượng đảo sau khi biến ô</em> <code>(r<sub>i</sub>, c<sub>i</sub>)</code> <em>thành đất liền</em>.</p>

<p><strong>Đảo</strong> là vùng đất liền được bao quanh bởi nước, gồm các ô đất liền kề nhau theo chiều ngang hoặc chiều dọc. Có thể giả sử cả bốn cạnh của lưới đều giáp với nước.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0305.Number%20of%20Islands%20II/images/tmp-grid.jpg" style="width: 500px; height: 294px;" />
<pre>
<strong>Đầu vào:</strong> m = 3, n = 3, positions = [[0,0],[0,1],[1,2],[2,1]]
<strong>Đầu ra:</strong> [1,1,2,3]
<strong>Giải thích:</strong>
Ban đầu, toàn bộ lưới 2D là nước.
- Thao tác #1: addLand(0, 0) biến ô nước tại grid[0][0] thành đất liền. Ta có 1 đảo.
- Thao tác #2: addLand(0, 1) biến ô nước tại grid[0][1] thành đất liền. Ta vẫn có 1 đảo.
- Thao tác #3: addLand(1, 2) biến ô nước tại grid[1][2] thành đất liền. Ta có 2 đảo.
- Thao tác #4: addLand(2, 1) biến ô nước tại grid[2][1] thành đất liền. Ta có 3 đảo.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 1, n = 1, positions = [[0,0]]
<strong>Đầu ra:</strong> [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n, positions.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= m * n &lt;= 10<sup>4</sup></code></li>
	<li><code>positions[i].length == 2</code></li>
	<li><code>0 &lt;= r<sub>i</sub> &lt; m</code></li>
	<li><code>0 &lt;= c<sub>i</sub> &lt; n</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này với độ phức tạp thời gian <code>O(k log(mn))</code>, trong đó <code>k == positions.length</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Các ô đất liền được thêm dần theo thời gian và ta cần trả về số đảo sau mỗi lần thêm. Nếu chạy lại DFS/BFS trên toàn lưới sau mỗi thao tác, khối lượng xử lý sẽ tăng theo kích thước lưới nhân với số lần cập nhật.
>
> Ô đất liền mới ban đầu tạo thành một đảo riêng, sau đó được gộp với đất liền ở bốn ô lân cận. Union-Find theo dõi các thành phần liên thông: chỉ gộp khi ô lân cận đã là đất liền và thuộc tập khác, rồi giảm số đảo đi $1$. Thêm lại cùng một ô không làm thay đổi gì. Mỗi thao tác gần như có độ phức tạp hằng số, nên tổng thời gian tăng tuyến tính theo $k$.

<!-- thinking:end -->

Ta dùng mảng hai chiều $grid$ để biểu diễn bản đồ, trong đó $0$ là nước và $1$ là đất liền. Ban đầu, mọi ô trong $grid$ đều là nước (tức có giá trị $0$); biến $cnt$ lưu số đảo. Ta dùng union-find $uf$ để theo dõi tính liên thông giữa các ô.

Tiếp theo, ta duyệt từng vị trí $(i, j)$ trong mảng $positions$. Nếu $grid[i][j]$ bằng $1$, ô này đã là đất liền nên ta thêm $cnt$ vào đáp án. Nếu chưa, ta đổi $grid[i][j]$ thành $1$ và tăng $cnt$ lên $1$. Sau đó, ta kiểm tra bốn ô lân cận theo hướng lên, xuống, trái và phải. Nếu ô lân cận là đất liền và không thuộc cùng thành phần liên thông với $(i, j)$, ta gộp hai ô vào cùng một thành phần rồi giảm $cnt$ đi $1$. Sau khi kiểm tra cả bốn hướng, ta thêm $cnt$ vào đáp án.

Độ phức tạp thời gian là $O(k \times \alpha(m \times n))$ hoặc $O(k \times \log(m \times n))$, trong đó $k$ là độ dài của $positions$, còn $\alpha$ là hàm ngược của hàm Ackermann. Trong bài này, có thể xem $\alpha(m \times n)$ là một hằng số rất nhỏ.

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n: int):
        self.p = list(range(n))
        self.size = [1] * n

    def find(self, x: int):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a: int, b: int) -> bool:
        pa, pb = self.find(a - 1), self.find(b - 1)
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
    def numIslands2(self, m: int, n: int, positions: List[List[int]]) -> List[int]:
        uf = UnionFind(m * n)
        grid = [[0] * n for _ in range(m)]
        ans = []
        dirs = (-1, 0, 1, 0, -1)
        cnt = 0
        for i, j in positions:
            if grid[i][j]:
                ans.append(cnt)
                continue
            grid[i][j] = 1
            cnt += 1
            for a, b in pairwise(dirs):
                x, y = i + a, j + b
                if (
                    0 <= x < m
                    and 0 <= y < n
                    and grid[x][y]
                    and uf.union(i * n + j, x * n + y)
                ):
                    cnt -= 1
            ans.append(cnt)
        return ans
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
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        int[][] grid = new int[m][n];
        UnionFind uf = new UnionFind(m * n);
        int[] dirs = {-1, 0, 1, 0, -1};
        int cnt = 0;
        List<Integer> ans = new ArrayList<>();
        for (var p : positions) {
            int i = p[0], j = p[1];
            if (grid[i][j] == 1) {
                ans.add(cnt);
                continue;
            }
            grid[i][j] = 1;
            ++cnt;
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1
                    && uf.union(i * n + j, x * n + y)) {
                    --cnt;
                }
            }
            ans.add(cnt);
        }
        return ans;
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
    vector<int> numIslands2(int m, int n, vector<vector<int>>& positions) {
        int grid[m][n];
        memset(grid, 0, sizeof(grid));
        UnionFind uf(m * n);
        int dirs[5] = {-1, 0, 1, 0, -1};
        int cnt = 0;
        vector<int> ans;
        for (auto& p : positions) {
            int i = p[0], j = p[1];
            if (grid[i][j]) {
                ans.push_back(cnt);
                continue;
            }
            grid[i][j] = 1;
            ++cnt;
            for (int k = 0; k < 4; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] && uf.unite(i * n + j, x * n + y)) {
                    --cnt;
                }
            }
            ans.push_back(cnt);
        }
        return ans;
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

func numIslands2(m int, n int, positions [][]int) (ans []int) {
	uf := newUnionFind(m * n)
	grid := make([][]int, m)
	for i := range grid {
		grid[i] = make([]int, n)
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	cnt := 0
	for _, p := range positions {
		i, j := p[0], p[1]
		if grid[i][j] == 1 {
			ans = append(ans, cnt)
			continue
		}
		grid[i][j] = 1
		cnt++
		for k := 0; k < 4; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1 && uf.union(i*n+j, x*n+y) {
				cnt--
			}
		}
		ans = append(ans, cnt)
	}
	return
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

function numIslands2(m: number, n: number, positions: number[][]): number[] {
    const grid: number[][] = Array.from({ length: m }, () => Array(n).fill(0));
    const uf = new UnionFind(m * n);
    const ans: number[] = [];
    const dirs: number[] = [-1, 0, 1, 0, -1];
    let cnt = 0;
    for (const [i, j] of positions) {
        if (grid[i][j]) {
            ans.push(cnt);
            continue;
        }
        grid[i][j] = 1;
        ++cnt;
        for (let k = 0; k < 4; ++k) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x < 0 || x >= m || y < 0 || y >= n || !grid[x][y]) {
                continue;
            }
            if (uf.union(i * n + j, x * n + y)) {
                --cnt;
            }
        }
        ans.push(cnt);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
