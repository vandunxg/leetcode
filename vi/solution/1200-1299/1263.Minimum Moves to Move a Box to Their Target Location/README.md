---
comments: true
difficulty: Hard
rating: 2297
source: Weekly Contest 163 Q4
tags:
    - Breadth-First Search
    - Array
    - Matrix
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1263. Minimum Moves to Move a Box to Their Target Location](https://leetcode.com/problems/minimum-moves-to-move-a-box-to-their-target-location)

[中文文档](/solution/1200-1299/1263.Minimum%20Moves%20to%20Move%20a%20Box%20to%20Their%20Target%20Location/README.md)

## Mô tả

<!-- description:start -->

<p>Storekeeper là trò chơi trong đó người chơi đẩy các hộp trong nhà kho để đưa chúng đến vị trí mục tiêu.</p>

<p>Trò chơi được biểu diễn bằng grid ký tự <code>m x n</code> tên <code>grid</code>, trong đó mỗi ô là tường, nền hoặc hộp.</p>

<p>Nhiệm vụ của bạn là đưa hộp <code>&#39;B&#39;</code> đến vị trí mục tiêu <code>&#39;T&#39;</code> theo các quy tắc sau:</p>

<ul>
	<li>Ký tự <code>&#39;S&#39;</code> biểu thị người chơi. Người chơi có thể di chuyển lên, xuống, trái hoặc phải trong <code>grid</code> nếu ô đó là nền (ô trống).</li>
	<li>Ký tự <code>&#39;.&#39;</code> biểu thị nền, tức ô có thể đi qua.</li>
	<li>Ký tự <font face="monospace">&nbsp;</font><code>&#39;#&#39;</code><font face="monospace">&nbsp;</font>biểu thị tường, tức chướng ngại vật không thể đi qua.</li>
	<li>Trong <code>grid</code> chỉ có một hộp <code>&#39;B&#39;</code> và một ô mục tiêu <code>&#39;T&#39;</code>.</li>
	<li>Có thể đẩy hộp sang ô trống liền kề bằng cách đứng cạnh hộp rồi di chuyển theo hướng đẩy. Thao tác này được gọi là <strong>push</strong>.</li>
	<li>Người chơi không thể đi xuyên qua hộp.</li>
</ul>

<p>Trả về <em>số lần <strong>push</strong> ít nhất để đưa hộp đến mục tiêu</em>. Nếu không thể đến mục tiêu, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1263.Minimum%20Moves%20to%20Move%20a%20Box%20to%20Their%20Target%20Location/images/sample_1_1620.png" style="width: 500px; height: 335px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;T&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;.&quot;,&quot;.&quot;,&quot;B&quot;,&quot;.&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;.&quot;,&quot;#&quot;,&quot;#&quot;,&quot;.&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;S&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta chỉ trả về số lần đẩy hộp.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;T&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;.&quot;,&quot;.&quot;,&quot;B&quot;,&quot;.&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;.&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;S&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;]]
<strong>Đầu ra:</strong> -1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;T&quot;,&quot;.&quot;,&quot;.&quot;,&quot;#&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;.&quot;,&quot;#&quot;,&quot;B&quot;,&quot;.&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;S&quot;,&quot;#&quot;],
               [&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;#&quot;]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> đẩy hộp xuống, sang trái, sang trái, lên rồi lên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 20</code></li>
	<li><code>grid</code> chỉ chứa các ký tự <code>&#39;.&#39;</code>, <code>&#39;#&#39;</code>, <code>&#39;S&#39;</code>, <code>&#39;T&#39;</code> hoặc <code>&#39;B&#39;</code>.</li>
	<li>Trong <code>grid</code> chỉ có duy nhất một ký tự <code>&#39;S&#39;</code>, <code>&#39;B&#39;</code> và <code>&#39;T&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Deque + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tối thiểu số lần đẩy hộp; người chơi di chuyển không tốn chi phí. Một state gồm cặp vị trí của người chơi và hộp. Vì $m,n \le 20$, không gian state vẫn có thể xử lý được. Đi bộ đến hộp có chi phí $0$, còn đẩy hộp có chi phí $1$, nên có thể dùng 0-1 BFS.
>
> Dùng deque: nếu chỉ người chơi di chuyển thì giữ nguyên $d$ và thêm state vào đầu deque; nếu người chơi bước đến vị trí hộp và đẩy hộp theo cùng hướng thì chi phí tăng thành $d+1$ và thêm state vào cuối deque. Giá trị $d$ khi hộp ở vị trí $T$ là đáp án. 0-1 BFS xử lý các cạnh miễn phí trước.

<!-- thinking:end -->

Ta biểu diễn vị trí người chơi và hộp bằng state $(s_i, s_j, b_i, b_j)$, trong đó $(s_i, s_j)$ là vị trí người chơi và $(b_i, b_j)$ là vị trí hộp. Trong code, hàm $f(i, j)$ ánh xạ tọa độ hai chiều $(i, j)$ thành số state một chiều: $f(i, j) = i \times n + j$, với $n$ là số cột của grid. Khi đó, state của người chơi và hộp là $(f(s_i, s_j), f(b_i, b_j))$.

Trước tiên, duyệt grid để tìm vị trí ban đầu của người chơi và hộp, lần lượt ký hiệu là $(s_i, s_j)$ và $(b_i, b_j)$.

Tiếp theo, định nghĩa deque $q$, mỗi phần tử là bộ ba $(f(s_i, s_j), f(b_i, b_j), d)$ biểu thị người chơi ở $(s_i, s_j)$, hộp ở $(b_i, b_j)$ và đã thực hiện $d$ lần đẩy. Ban đầu, thêm $(f(s_i, s_j), f(b_i, b_j), 0)$ vào $q$.

Ngoài ra, dùng mảng hai chiều $vis$ để ghi nhận state đã được thăm hay chưa. Ban đầu, đánh dấu $vis[f(s_i, s_j), f(b_i, b_j)]$ là đã thăm.

Sau đó, bắt đầu breadth-first search.

Ở mỗi bước, lấy phần tử đầu queue $(f(s_i, s_j), f(b_i, b_j), d)$ ra và kiểm tra $grid[b_i][b_j] = 'T'$. Nếu đúng, hộp đã được đẩy đến vị trí mục tiêu và có thể trả về $d$ làm đáp án.

Nếu chưa đến đích, xét các hướng di chuyển tiếp theo của người chơi. Gọi vị trí mới là $(s_x, s_y)$. Nếu $(s_x, s_y)$ là vị trí hợp lệ, kiểm tra xem nó có trùng vị trí hộp $(b_i, b_j)$ hay không:

- Nếu trùng, người chơi đã đến vị trí hộp và đẩy hộp tiến một ô. Vị trí mới của hộp là $(b_x, b_y)$. Nếu $(b_x, b_y)$ hợp lệ và state $(f(s_x, s_y), f(b_x, b_y))$ chưa được thăm, thêm $(f(s_x, s_y), f(b_x, b_y), d + 1)$ vào cuối $q$ và đánh dấu $vis[f(s_x, s_y), f(b_x, b_y)]$ đã thăm.
- Nếu không trùng, người chơi chưa đẩy hộp. Khi đó, chỉ cần kiểm tra state $(f(s_x, s_y), f(b_i, b_j))$ đã được thăm chưa. Nếu chưa, thêm $(f(s_x, s_y), f(b_i, b_j), d)$ vào đầu $q$ và đánh dấu $vis[f(s_x, s_y), f(b_i, b_j)]$ đã thăm.

Tiếp tục breadth-first search cho đến khi queue rỗng.

> Lưu ý, nếu đẩy hộp thì tăng số lần đẩy $d$ thêm $1$ và thêm state mới vào cuối $q$. Nếu không đẩy hộp, giữ nguyên $d$ và thêm state mới vào đầu $q$.

Cuối cùng, nếu không tìm được cách đẩy hợp lệ thì trả về $-1$.

Độ phức tạp thời gian là $O(m^2 \times n^2)$ và độ phức tạp không gian là $O(m^2 \times n^2)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của grid.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minPushBox(self, grid: List[List[str]]) -> int:
        def f(i: int, j: int) -> int:
            return i * n + j

        def check(i: int, j: int) -> bool:
            return 0 <= i < m and 0 <= j < n and grid[i][j] != "#"

        for i, row in enumerate(grid):
            for j, c in enumerate(row):
                if c == "S":
                    si, sj = i, j
                elif c == "B":
                    bi, bj = i, j
        m, n = len(grid), len(grid[0])
        dirs = (-1, 0, 1, 0, -1)
        q = deque([(f(si, sj), f(bi, bj), 0)])
        vis = [[False] * (m * n) for _ in range(m * n)]
        vis[f(si, sj)][f(bi, bj)] = True
        while q:
            s, b, d = q.popleft()
            bi, bj = b // n, b % n
            if grid[bi][bj] == "T":
                return d
            si, sj = s // n, s % n
            for a, b in pairwise(dirs):
                sx, sy = si + a, sj + b
                if not check(sx, sy):
                    continue
                if sx == bi and sy == bj:
                    bx, by = bi + a, bj + b
                    if not check(bx, by) or vis[f(sx, sy)][f(bx, by)]:
                        continue
                    vis[f(sx, sy)][f(bx, by)] = True
                    q.append((f(sx, sy), f(bx, by), d + 1))
                elif not vis[f(sx, sy)][f(bi, bj)]:
                    vis[f(sx, sy)][f(bi, bj)] = True
                    q.appendleft((f(sx, sy), f(bi, bj), d))
        return -1
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private char[][] grid;

    public int minPushBox(char[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        int si = 0, sj = 0, bi = 0, bj = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 'S') {
                    si = i;
                    sj = j;
                } else if (grid[i][j] == 'B') {
                    bi = i;
                    bj = j;
                }
            }
        }
        int[] dirs = {-1, 0, 1, 0, -1};
        Deque<int[]> q = new ArrayDeque<>();
        boolean[][] vis = new boolean[m * n][m * n];
        q.offer(new int[] {f(si, sj), f(bi, bj), 0});
        vis[f(si, sj)][f(bi, bj)] = true;
        while (!q.isEmpty()) {
            var p = q.poll();
            int d = p[2];
            bi = p[1] / n;
            bj = p[1] % n;
            if (grid[bi][bj] == 'T') {
                return d;
            }
            si = p[0] / n;
            sj = p[0] % n;
            for (int k = 0; k < 4; ++k) {
                int sx = si + dirs[k], sy = sj + dirs[k + 1];
                if (!check(sx, sy)) {
                    continue;
                }
                if (sx == bi && sy == bj) {
                    int bx = bi + dirs[k], by = bj + dirs[k + 1];
                    if (!check(bx, by) || vis[f(sx, sy)][f(bx, by)]) {
                        continue;
                    }
                    vis[f(sx, sy)][f(bx, by)] = true;
                    q.offer(new int[] {f(sx, sy), f(bx, by), d + 1});
                } else if (!vis[f(sx, sy)][f(bi, bj)]) {
                    vis[f(sx, sy)][f(bi, bj)] = true;
                    q.offerFirst(new int[] {f(sx, sy), f(bi, bj), d});
                }
            }
        }
        return -1;
    }

    private int f(int i, int j) {
        return i * n + j;
    }

    private boolean check(int i, int j) {
        return i >= 0 && i < m && j >= 0 && j < n && grid[i][j] != '#';
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minPushBox(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int si, sj, bi, bj;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 'S') {
                    si = i, sj = j;
                } else if (grid[i][j] == 'B') {
                    bi = i, bj = j;
                }
            }
        }
        auto f = [&](int i, int j) {
            return i * n + j;
        };
        auto check = [&](int i, int j) {
            return i >= 0 && i < m && j >= 0 && j < n && grid[i][j] != '#';
        };
        int dirs[5] = {-1, 0, 1, 0, -1};
        deque<tuple<int, int, int>> q;
        q.emplace_back(f(si, sj), f(bi, bj), 0);
        bool vis[m * n][m * n];
        memset(vis, false, sizeof(vis));
        vis[f(si, sj)][f(bi, bj)] = true;
        while (!q.empty()) {
            auto [s, b, d] = q.front();
            q.pop_front();
            si = s / n, sj = s % n;
            bi = b / n, bj = b % n;
            if (grid[bi][bj] == 'T') {
                return d;
            }
            for (int k = 0; k < 4; ++k) {
                int sx = si + dirs[k], sy = sj + dirs[k + 1];
                if (!check(sx, sy)) {
                    continue;
                }
                if (sx == bi && sy == bj) {
                    int bx = bi + dirs[k], by = bj + dirs[k + 1];
                    if (!check(bx, by) || vis[f(sx, sy)][f(bx, by)]) {
                        continue;
                    }
                    vis[f(sx, sy)][f(bx, by)] = true;
                    q.emplace_back(f(sx, sy), f(bx, by), d + 1);
                } else if (!vis[f(sx, sy)][f(bi, bj)]) {
                    vis[f(sx, sy)][f(bi, bj)] = true;
                    q.emplace_front(f(sx, sy), f(bi, bj), d);
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minPushBox(grid [][]byte) int {
	m, n := len(grid), len(grid[0])
	var si, sj, bi, bj int
	for i, row := range grid {
		for j, c := range row {
			if c == 'S' {
				si, sj = i, j
			} else if c == 'B' {
				bi, bj = i, j
			}
		}
	}
	f := func(i, j int) int {
		return i*n + j
	}
	check := func(i, j int) bool {
		return i >= 0 && i < m && j >= 0 && j < n && grid[i][j] != '#'
	}
	q := [][]int{[]int{f(si, sj), f(bi, bj), 0}}
	vis := make([][]bool, m*n)
	for i := range vis {
		vis[i] = make([]bool, m*n)
	}
	vis[f(si, sj)][f(bi, bj)] = true
	dirs := [5]int{-1, 0, 1, 0, -1}
	for len(q) > 0 {
		p := q[0]
		q = q[1:]
		si, sj, bi, bj = p[0]/n, p[0]%n, p[1]/n, p[1]%n
		d := p[2]
		if grid[bi][bj] == 'T' {
			return d
		}
		for k := 0; k < 4; k++ {
			sx, sy := si+dirs[k], sj+dirs[k+1]
			if !check(sx, sy) {
				continue
			}
			if sx == bi && sy == bj {
				bx, by := bi+dirs[k], bj+dirs[k+1]
				if !check(bx, by) || vis[f(sx, sy)][f(bx, by)] {
					continue
				}
				vis[f(sx, sy)][f(bx, by)] = true
				q = append(q, []int{f(sx, sy), f(bx, by), d + 1})
			} else if !vis[f(sx, sy)][f(bi, bj)] {
				vis[f(sx, sy)][f(bi, bj)] = true
				q = append([][]int{[]int{f(sx, sy), f(bi, bj), d}}, q...)
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minPushBox(grid: string[][]): number {
    const [m, n] = [grid.length, grid[0].length];
    let [si, sj, bi, bj] = [0, 0, 0, 0];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            if (grid[i][j] === 'S') {
                [si, sj] = [i, j];
            } else if (grid[i][j] === 'B') {
                [bi, bj] = [i, j];
            }
        }
    }
    const f = (i: number, j: number): number => i * n + j;
    const check = (i: number, j: number): boolean =>
        i >= 0 && i < m && j >= 0 && j < n && grid[i][j] !== '#';

    const q: Deque<[number, number, number]> = new Deque();
    const vis: boolean[][] = new Array(m * n).fill(0).map(() => new Array(m * n).fill(false));
    q.push([f(si, sj), f(bi, bj), 0]);
    vis[f(si, sj)][f(bi, bj)] = true;
    const dirs: number[] = [-1, 0, 1, 0, -1];
    while (q.size() > 0) {
        const [s, b, d] = q.shift()!;
        const [si, sj] = [Math.floor(s / n), s % n];
        const [bi, bj] = [Math.floor(b / n), b % n];
        if (grid[bi][bj] === 'T') {
            return d;
        }
        for (let k = 0; k < 4; ++k) {
            const [sx, sy] = [si + dirs[k], sj + dirs[k + 1]];
            if (!check(sx, sy)) {
                continue;
            }
            if (sx === bi && sy === bj) {
                const [bx, by] = [bi + dirs[k], bj + dirs[k + 1]];
                if (!check(bx, by) || vis[f(sx, sy)][f(bx, by)]) {
                    continue;
                }
                vis[f(sx, sy)][f(bx, by)] = true;
                q.push([f(sx, sy), f(bx, by), d + 1]);
            } else if (!vis[f(sx, sy)][f(bi, bj)]) {
                vis[f(sx, sy)][f(bi, bj)] = true;
                q.unshift([f(sx, sy), f(bi, bj), d]);
            }
        }
    }
    return -1;
}

/* 以下是双向列队模板类 */
class CircularDeque<T> {
    prev: CircularDeque<T> | null;
    next: CircularDeque<T> | null;
    begin: number;
    end: number;
    empty: boolean;
    data: T[];
    constructor(N: number) {
        this.prev = this.next = null;
        this.begin = this.end = 0;
        this.empty = true;
        this.data = Array(N);
    }

    isFull(): boolean {
        return this.end === this.begin && !this.empty;
    }

    isEmpty(): boolean {
        return this.empty;
    }

    push(val: T): boolean {
        if (this.isFull()) return false;
        this.empty = false;
        this.data[this.end] = val;
        this.end = (this.end + 1) % this.data.length;
        return true;
    }

    front(): T | undefined {
        return this.isEmpty() ? undefined : this.data[this.begin];
    }

    back(): T | undefined {
        return this.isEmpty() ? undefined : this.data[this.end - 1];
    }

    pop(): T | undefined {
        if (this.isEmpty()) return undefined;
        const value = this.data[this.end - 1];
        this.end = (this.end - 1) % this.data.length;
        if (this.end < 0) this.end += this.data.length;
        if (this.end === this.begin) this.empty = true;
        return value;
    }

    unshift(val: T): boolean {
        if (this.isFull()) return false;
        this.empty = false;
        this.begin = (this.begin - 1) % this.data.length;
        if (this.begin < 0) this.begin += this.data.length;
        this.data[this.begin] = val;
        return true;
    }

    shift(): T | undefined {
        if (this.isEmpty()) return undefined;
        const value = this.data[this.begin];
        this.begin = (this.begin + 1) % this.data.length;
        if (this.end === this.begin) this.empty = true;
        return value;
    }

    *values(): Generator<T, void, undefined> {
        if (this.isEmpty()) return undefined;
        let i = this.begin;
        do {
            yield this.data[i];
            i = (i + 1) % this.data.length;
        } while (i !== this.end);
    }
}

class Deque<T> {
    head: CircularDeque<T>;
    tail: CircularDeque<T>;
    _size: number;
    constructor(collection: T[] = []) {
        this.head = new CircularDeque<T>(128);
        this.tail = new CircularDeque<T>(128);
        this.tail.empty = this.head.empty = false;
        this.tail.prev = this.head;
        this.head.next = this.tail;
        this._size = 0;
        for (const item of collection) this.push(item);
    }

    size(): number {
        return this._size;
    }

    push(val: T): void {
        let last = this.tail.prev!;
        if (last.isFull()) {
            const inserted = new CircularDeque<T>(128);

            this.tail.prev = inserted;
            inserted.next = this.tail;

            last.next = inserted;
            inserted.prev = last;

            last = inserted;
        }
        last.push(val);
        this._size++;
    }

    back(): T | undefined {
        if (this._size === 0) return;
        return this.tail.prev!.back();
    }

    pop(): T | undefined {
        if (this.head.next === this.tail) return undefined;
        const last = this.tail.prev!;
        const value = last.pop();
        if (last.isEmpty()) {
            this.tail.prev = last.prev;
            last.prev!.next = this.tail;
        }
        this._size--;
        return value;
    }

    unshift(val: T): void {
        let first = this.head.next!;
        if (first.isFull()) {
            const inserted = new CircularDeque<T>(128);

            this.head.next = inserted;
            inserted.prev = this.head;

            inserted.next = first;
            first.prev = inserted;

            first = inserted;
        }
        first.unshift(val);
        this._size++;
    }

    shift(): T | undefined {
        if (this.head.next === this.tail) return undefined;
        const first = this.head.next!;
        const value = first.shift();
        if (first.isEmpty()) {
            this.head.next = first.next;
            first.next!.prev = this.head;
        }
        this._size--;
        return value;
    }

    front(): T | undefined {
        if (this._size === 0) return undefined;
        return this.head.next!.front();
    }

    *values(): Generator<T, void, undefined> {
        let node = this.head.next!;
        while (node !== this.tail) {
            for (const value of node.values()) yield value;
            node = node.next!;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
