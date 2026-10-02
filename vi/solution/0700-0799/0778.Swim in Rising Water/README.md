---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Minimax
    - Array
    - Binary Search
    - Matrix
    - Dijkstra
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [778. Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water)

[中文文档](/solution/0700-0799/0778.Swim%20in%20Rising%20Water/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận số nguyên <code>grid</code> kích thước <code>n x n</code>, trong đó mỗi giá trị <code>grid[i][j]</code> biểu diễn độ cao tại điểm <code>(i, j)</code>.</p>

<p>Trời bắt đầu mưa và mực nước dâng dần theo thời gian. Tại thời điểm <code>t</code>, mực nước là <code>t</code>; khi đó, mọi ô có độ cao nhỏ hơn hoặc bằng <code>t</code> đều bị ngập hoặc có thể đi tới.</p>

<p>Bạn có thể bơi từ một ô sang ô kề theo bốn hướng khi và chỉ khi độ cao của cả hai ô đều không vượt quá <code>t</code>. Bạn có thể bơi quãng đường bất kỳ trong thời gian bằng 0. Tất nhiên, khi bơi bạn phải ở trong phạm vi của ma trận.</p>

<p>Trả về <em>thời điểm sớm nhất bạn có thể đến ô dưới cùng bên phải </em><code>(n - 1, n - 1)</code><em> khi bắt đầu ở ô trên cùng bên trái </em><code>(0, 0)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0778.Swim%20in%20Rising%20Water/images/swim1-grid.jpg" style="width: 164px; height: 165px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,2],[1,3]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Tại thời điểm 0, bạn ở vị trí (0, 0) trong grid.
Bạn không thể đi sang ô nào khác vì các ô kề theo bốn hướng đều có độ cao lớn hơn t = 0.
Bạn chưa thể đến điểm (1, 1) cho tới thời điểm 3.
Khi độ sâu của nước là 3, ta có thể bơi đến mọi vị trí trong grid.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0778.Swim%20in%20Rising%20Water/images/swim2-grid-1.jpg" style="width: 404px; height: 405px;" />
<pre>
<strong>Đầu vào:</strong> grid = [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Đường đi cuối cùng được minh họa trong hình.
Ta cần đợi đến thời điểm 16 để có thể đi từ (0, 0) đến (4, 4).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;&nbsp;n<sup>2</sup></code></li>
	<li>Mỗi giá trị <code>grid[i][j]</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union Find

<!-- thinking:start -->

> **Tư duy**
>
> Ở thời điểm $t$, ta có thể đi qua các ô có độ cao $\le t$. Độ cao là một hoán vị của $0..n^2-1$, vì vậy thêm các ô theo thứ tự độ cao.
>
> Khi ô có độ cao $t$ được union với các ô kề đã được thêm trước đó, thời điểm đầu tiên điểm bắt đầu kết nối với điểm đích chính là đáp án.
>
> Ánh xạ độ cao sang id, rồi thực hiện union khi $t$ tăng dần. $O(n^2\alpha)$.

<!-- thinking:end -->

Ta có thể ánh xạ mỗi vị trí $(i, j)$ thành ID $id = i \times n + j$, rồi dùng cấu trúc dữ liệu union-find để quản lý các thành phần liên thông.

Đầu tiên, dùng mảng một chiều $hi$ để lưu ID vị trí tương ứng với từng độ cao; cụ thể, $hi[h]$ là ID của vị trí có độ cao $h$.

Sau đó, duyệt độ cao từ $0$ đến $n^2 - 1$. Với mỗi độ cao $t$, gộp vị trí $hi[t]$ với các vị trí kề theo bốn hướng có độ cao không vượt quá $t$. Nếu sau khi gộp, vị trí $0$ và vị trí $n^2 - 1$ đã kết nối với nhau thì ta tìm được thời điểm nhỏ nhất $t$ và trả về $t$.

Độ phức tạp thời gian là $O(n^2 \times \log n)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài cạnh của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        n = len(grid)
        m = n * n
        p = list(range(m))
        hi = [0] * m
        for i, row in enumerate(grid):
            for j, h in enumerate(row):
                hi[h] = i * n + j
        dirs = (-1, 0, 1, 0, -1)
        for t in range(m):
            x, y = divmod(hi[t], n)
            for dx, dy in pairwise(dirs):
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < n and grid[nx][ny] <= t:
                    p[find(x * n + y)] = find(nx * n + ny)
            if find(0) == find(m - 1):
                return t
        return 0
```

#### Java

```java
class Solution {
    public int swimInWater(int[][] grid) {
        int n = grid.length;
        int m = n * n;
        int[] p = new int[m];
        Arrays.setAll(p, i -> i);
        IntUnaryOperator find = new IntUnaryOperator() {
            @Override
            public int applyAsInt(int x) {
                if (p[x] != x) p[x] = applyAsInt(p[x]);
                return p[x];
            }
        };

        int[] hi = new int[m];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                hi[grid[i][j]] = i * n + j;
            }
        }

        int[] dirs = {-1, 0, 1, 0, -1};

        for (int t = 0; t < m; t++) {
            int id = hi[t];
            int x = id / n, y = id % n;
            for (int k = 0; k < 4; k++) {
                int nx = x + dirs[k], ny = y + dirs[k + 1];
                if (nx >= 0 && nx < n && ny >= 0 && ny < n && grid[nx][ny] <= t) {
                    int a = find.applyAsInt(x * n + y);
                    int b = find.applyAsInt(nx * n + ny);
                    p[a] = b;
                }
            }
            if (find.applyAsInt(0) == find.applyAsInt(m - 1)) {
                return t;
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
    int swimInWater(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = n * n;
        vector<int> p(m);
        iota(p.begin(), p.end(), 0);

        auto find = [&](this auto&& find, int x) -> int {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };

        vector<int> hi(m);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                hi[grid[i][j]] = i * n + j;
            }
        }

        array<int, 5> dirs{-1, 0, 1, 0, -1};

        for (int t = 0; t < m; ++t) {
            int id = hi[t];
            int x = id / n, y = id % n;
            for (int k = 0; k < 4; ++k) {
                int nx = x + dirs[k], ny = y + dirs[k + 1];
                if (nx >= 0 && nx < n && ny >= 0 && ny < n && grid[nx][ny] <= t) {
                    int a = find(x * n + y);
                    int b = find(nx * n + ny);
                    p[a] = b;
                }
            }
            if (find(0) == find(m - 1)) {
                return t;
            }
        }
        return 0;
    }
};
```

#### Go

```go
func swimInWater(grid [][]int) int {
	n := len(grid)
	m := n * n
	p := make([]int, m)
	for i := range p {
		p[i] = i
	}
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	hi := make([]int, m)
	for i := range grid {
		for j, h := range grid[i] {
			hi[h] = i*n + j
		}
	}
	dirs := []int{-1, 0, 1, 0, -1}
	for t := 0; t < m; t++ {
		id := hi[t]
		x, y := id/n, id%n
		for k := 0; k < 4; k++ {
			nx, ny := x+dirs[k], y+dirs[k+1]
			if nx >= 0 && nx < n && ny >= 0 && ny < n && grid[nx][ny] <= t {
				a := find(x*n + y)
				b := find(nx*n + ny)
				p[a] = b
			}
		}
		if find(0) == find(m-1) {
			return t
		}
	}
	return 0
}
```

#### TypeScript

```ts
function swimInWater(grid: number[][]): number {
    const n = grid.length;
    const m = n * n;
    const p = Array.from({ length: m }, (_, i) => i);
    const hi = new Array<number>(m);
    const find = (x: number): number => (p[x] === x ? x : (p[x] = find(p[x])));

    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            hi[grid[i][j]] = i * n + j;
        }
    }

    const dirs = [-1, 0, 1, 0, -1];

    for (let t = 0; t < m; ++t) {
        const id = hi[t];
        const x = Math.floor(id / n);
        const y = id % n;

        for (let k = 0; k < 4; ++k) {
            const nx = x + dirs[k];
            const ny = y + dirs[k + 1];
            if (nx >= 0 && nx < n && ny >= 0 && ny < n && grid[nx][ny] <= t) {
                p[find(x * n + y)] = find(nx * n + ny);
            }
        }
        if (find(0) === find(m - 1)) {
            return t;
        }
    }

    return 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn swim_in_water(grid: Vec<Vec<i32>>) -> i32 {
        let n = grid.len();
        let m = n * n;
        let mut p: Vec<usize> = (0..m).collect();
        let mut hi = vec![0usize; m];

        for i in 0..n {
            for j in 0..n {
                hi[grid[i][j] as usize] = i * n + j;
            }
        }

        fn find(x: usize, p: &mut Vec<usize>) -> usize {
            if p[x] != x {
                p[x] = find(p[x], p);
            }
            p[x]
        }

        let dirs = [-1isize, 0, 1, 0, -1];

        for t in 0..m {
            let id = hi[t];
            let x = id / n;
            let y = id % n;

            for k in 0..4 {
                let nx = x as isize + dirs[k];
                let ny = y as isize + dirs[k + 1];
                if nx >= 0 && nx < n as isize && ny >= 0 && ny < n as isize {
                    let nx = nx as usize;
                    let ny = ny as usize;
                    if grid[nx][ny] as usize <= t {
                        let a = find(x * n + y, &mut p);
                        let b = find(nx * n + ny, &mut p);
                        p[a] = b;
                    }
                }
            }

            if find(0, &mut p) == find(m - 1, &mut p) {
                return t as i32;
            }
        }

        0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
