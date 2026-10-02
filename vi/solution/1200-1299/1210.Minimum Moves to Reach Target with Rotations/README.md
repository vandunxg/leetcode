---
comments: true
difficulty: Hard
rating: 2022
source: Weekly Contest 156 Q4
tags:
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [1210. Minimum Moves to Reach Target with Rotations](https://leetcode.com/problems/minimum-moves-to-reach-target-with-rotations)

[中文文档](/solution/1200-1299/1210.Minimum%20Moves%20to%20Reach%20Target%20with%20Rotations/README.md)

## Mô tả

<!-- description:start -->

<p>Trong lưới <code>n*n</code>, có một con rắn dài 2 ô bắt đầu ở góc trên bên trái tại <code>(0, 0)</code> và <code>(0, 1)</code>. Ô trống được biểu diễn bằng số 0, ô bị chặn bằng số 1. Con rắn cần đến góc dưới bên phải tại <code>(n-1, n-2)</code> và <code>(n-1, n-1)</code>.</p>

<p>Trong một bước, con rắn có thể:</p>

<ul>
	<li>Di chuyển sang phải một ô nếu ô đó không bị chặn. Hướng ngang/dọc của con rắn không thay đổi.</li>
	<li>Di chuyển xuống một ô nếu ô đó không bị chặn. Hướng ngang/dọc của con rắn không thay đổi.</li>
	<li>Xoay theo chiều kim đồng hồ nếu con rắn đang nằm ngang và hai ô bên dưới đều trống. Khi đó, con rắn chuyển từ&nbsp;<code>(r, c)</code>&nbsp;và&nbsp;<code>(r, c+1)</code>&nbsp;sang&nbsp;<code>(r, c)</code>&nbsp;và&nbsp;<code>(r+1, c)</code>.<br />
	<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1210.Minimum%20Moves%20to%20Reach%20Target%20with%20Rotations/images/image-2.png" style="width: 300px; height: 134px;" /></li>
	<li>Xoay ngược chiều kim đồng hồ nếu con rắn đang nằm dọc và hai ô bên phải đều trống. Khi đó, con rắn chuyển từ&nbsp;<code>(r, c)</code>&nbsp;và&nbsp;<code>(r+1, c)</code>&nbsp;sang&nbsp;<code>(r, c)</code>&nbsp;và&nbsp;<code>(r, c+1)</code>.<br />
	<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1210.Minimum%20Moves%20to%20Reach%20Target%20with%20Rotations/images/image-1.png" style="width: 300px; height: 121px;" /></li>
</ul>

<p>Trả về số bước ít nhất để đến đích.</p>

<p>Nếu không thể đến đích, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1210.Minimum%20Moves%20to%20Reach%20Target%20with%20Rotations/images/image.png" style="width: 400px; height: 439px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,0,0,0,0,1],
               [1,1,0,0,1,0],
&nbsp;              [0,0,0,0,1,1],
&nbsp;              [0,0,1,0,1,0],
&nbsp;              [0,1,1,0,0,0],
&nbsp;              [0,1,1,0,0,0]]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:
</strong>Một cách đi khả thi là [right, right, rotate clockwise, right, down, down, down, down, rotate counterclockwise, right, down].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grid = [[0,0,1,1,1,1],
&nbsp;              [0,0,0,0,1,1],
&nbsp;              [1,1,0,0,0,1],
&nbsp;              [1,1,1,0,0,1],
&nbsp;              [1,1,1,0,0,1],
&nbsp;              [1,1,1,0,0,0]]
<strong>Đầu ra:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 1</code></li>
	<li>Đảm bảo con rắn bắt đầu trên các ô trống.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Con rắn có thể tịnh tiến và xoay; trạng thái được xác định bởi ô đuôi và hướng của nó. Vì $n \le 100$, có $O(n^2)$ trạng thái, nên ta dùng BFS để tìm đường đi ngắn nhất.
>
> Mỗi bước có thể dịch chuyển sang phải hoặc xuống dưới (cả hai đầu cùng di chuyển, vẫn nằm trong lưới và không đi vào ô bị chặn). Rắn nằm ngang có thể xoay dựng lên theo chiều kim đồng hồ; rắn nằm dọc có thể xoay nằm xuống ngược chiều kim đồng hồ nếu ô quét qua đang trống. Sau khi chuyển tọa độ thành chỉ số một chiều, ta đánh dấu trạng thái $(tail, orientation)$ đã thăm.
>
> Queue lưu $(tail, head)$ và được mở rộng theo từng lớp. Lớp đầu tiên chạm tới $(n^2-2, n^2-1)$ chính là đáp án. BFS đảm bảo tìm được đường đi ngắn nhất.

<!-- thinking:end -->

Bài toán yêu cầu tìm số bước ít nhất để con rắn đi từ vị trí bắt đầu đến đích. Ta dùng Breadth-First Search (BFS) để giải.

Ta định nghĩa các cấu trúc dữ liệu hoặc biến sau:

- Queue $q$: Lưu vị trí hiện tại của con rắn. Mỗi vị trí là tuple $(a, b)$, trong đó $a$ là vị trí đuôi và $b$ là vị trí đầu. Ban đầu, ta thêm vị trí $(0, 1)$ vào queue $q$. Nếu chuyển lưới 2D thành mảng 1D, vị trí $(0, 1)$ tương ứng với hai ô có chỉ số $0$ và $1$ trong mảng 1D.
- Vị trí đích $target$: Có giá trị cố định là $(n^2 - 2, n^2 - 1)$, tương ứng với hai ô cuối cùng của hàng cuối trong lưới 2D.
- Mảng hoặc set $vis$: Lưu trạng thái vị trí của con rắn đã được thăm hay chưa. Mỗi trạng thái là tuple $(a, status)$, trong đó $a$ là vị trí đuôi và $status$ biểu thị hướng hiện tại của con rắn: $0$ là nằm ngang, $1$ là nằm dọc. Ban đầu, thêm trạng thái vị trí bắt đầu $(0, 1)$ vào set $vis$.
- Biến đáp án $ans$: Lưu số bước con rắn đã đi từ vị trí bắt đầu đến vị trí hiện tại. Ban đầu, giá trị là $0$.

Ta dùng BFS để giải. Mỗi lần lấy một vị trí khỏi queue $q$, ta kiểm tra xem đó có phải vị trí đích $target$ không. Nếu đúng, trả về ngay biến đáp án $ans$. Nếu chưa tới đích, ta thêm các vị trí kế tiếp có thể đến vào queue $q$ và đánh dấu chúng trong $vis$. Lưu ý rằng trạng thái tiếp theo có thể là rắn nằm ngang hoặc nằm dọc, cần xét riêng từng trường hợp (xem các code comment bên dưới). Sau mỗi lượt tìm kiếm, tăng $ans$ thêm $1$.

Cuối cùng, nếu queue $q$ rỗng, nghĩa là không thể đi từ vị trí bắt đầu đến đích, nên trả về $-1$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số hàng hoặc số cột của lưới 2D.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumMoves(self, grid: List[List[int]]) -> int:
        def move(i1, j1, i2, j2):
            if 0 <= i1 < n and 0 <= j1 < n and 0 <= i2 < n and 0 <= j2 < n:
                a, b = i1 * n + j1, i2 * n + j2
                status = 0 if i1 == i2 else 1
                if (a, status) not in vis and grid[i1][j1] == 0 and grid[i2][j2] == 0:
                    q.append((a, b))
                    vis.add((a, status))

        n = len(grid)
        target = (n * n - 2, n * n - 1)
        q = deque([(0, 1)])
        vis = {(0, 0)}
        ans = 0
        while q:
            for _ in range(len(q)):
                a, b = q.popleft()
                if (a, b) == target:
                    return ans
                i1, j1 = a // n, a % n
                i2, j2 = b // n, b % n
                # 尝试向右平移（保持身体水平/垂直状态）
                move(i1, j1 + 1, i2, j2 + 1)
                # 尝试向下平移（保持身体水平/垂直状态）
                move(i1 + 1, j1, i2 + 1, j2)
                # 当前处于水平状态，且 grid[i1 + 1][j2] 无障碍，尝试顺时针旋转90°
                if i1 == i2 and i1 + 1 < n and grid[i1 + 1][j2] == 0:
                    move(i1, j1, i1 + 1, j1)
                # 当前处于垂直状态，且 grid[i2][j1 + 1] 无障碍，尝试逆时针旋转90°
                if j1 == j2 and j1 + 1 < n and grid[i2][j1 + 1] == 0:
                    move(i1, j1, i1, j1 + 1)
            ans += 1
        return -1
```

#### Java

```java
class Solution {
    private int n;
    private int[][] grid;
    private boolean[][] vis;
    private Deque<int[]> q = new ArrayDeque<>();

    public int minimumMoves(int[][] grid) {
        this.grid = grid;
        n = grid.length;
        vis = new boolean[n * n][2];
        int[] target = {n * n - 2, n * n - 1};
        q.offer(new int[] {0, 1});
        vis[0][0] = true;
        int ans = 0;
        while (!q.isEmpty()) {
            for (int k = q.size(); k > 0; --k) {
                var p = q.poll();
                if (p[0] == target[0] && p[1] == target[1]) {
                    return ans;
                }
                int i1 = p[0] / n, j1 = p[0] % n;
                int i2 = p[1] / n, j2 = p[1] % n;
                // 尝试向右平移（保持身体水平/垂直状态）
                move(i1, j1 + 1, i2, j2 + 1);
                // 尝试向下平移（保持身体水平/垂直状态）
                move(i1 + 1, j1, i2 + 1, j2);
                // 当前处于水平状态，且 grid[i1 + 1][j2] 无障碍，尝试顺时针旋转90°
                if (i1 == i2 && i1 + 1 < n && grid[i1 + 1][j2] == 0) {
                    move(i1, j1, i1 + 1, j1);
                }
                // 当前处于垂直状态，且 grid[i2][j1 + 1] 无障碍，尝试逆时针旋转90°
                if (j1 == j2 && j1 + 1 < n && grid[i2][j1 + 1] == 0) {
                    move(i1, j1, i1, j1 + 1);
                }
            }
            ++ans;
        }
        return -1;
    }

    private void move(int i1, int j1, int i2, int j2) {
        if (i1 >= 0 && i1 < n && j1 >= 0 && j1 < n && i2 >= 0 && i2 < n && j2 >= 0 && j2 < n) {
            int a = i1 * n + j1, b = i2 * n + j2;
            int status = i1 == i2 ? 0 : 1;
            if (!vis[a][status] && grid[i1][j1] == 0 && grid[i2][j2] == 0) {
                q.offer(new int[] {a, b});
                vis[a][status] = true;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumMoves(vector<vector<int>>& grid) {
        int n = grid.size();
        auto target = make_pair(n * n - 2, n * n - 1);
        queue<pair<int, int>> q;
        q.emplace(0, 1);
        bool vis[n * n][2];
        memset(vis, 0, sizeof vis);
        vis[0][0] = true;

        auto move = [&](int i1, int j1, int i2, int j2) {
            if (i1 >= 0 && i1 < n && j1 >= 0 && j1 < n && i2 >= 0 && i2 < n && j2 >= 0 && j2 < n) {
                int a = i1 * n + j1, b = i2 * n + j2;
                int status = i1 == i2 ? 0 : 1;
                if (!vis[a][status] && grid[i1][j1] == 0 && grid[i2][j2] == 0) {
                    q.emplace(a, b);
                    vis[a][status] = true;
                }
            }
        };

        int ans = 0;
        while (!q.empty()) {
            for (int k = q.size(); k; --k) {
                auto p = q.front();
                q.pop();
                if (p == target) {
                    return ans;
                }
                auto [a, b] = p;
                int i1 = a / n, j1 = a % n;
                int i2 = b / n, j2 = b % n;
                // 尝试向右平移（保持身体水平/垂直状态）
                move(i1, j1 + 1, i2, j2 + 1);
                // 尝试向下平移（保持身体水平/垂直状态）
                move(i1 + 1, j1, i2 + 1, j2);
                // 当前处于水平状态，且 grid[i1 + 1][j2] 无障碍，尝试顺时针旋转90°
                if (i1 == i2 && i1 + 1 < n && grid[i1 + 1][j2] == 0) {
                    move(i1, j1, i1 + 1, j1);
                }
                // 当前处于垂直状态，且 grid[i2][j1 + 1] 无障碍，尝试逆时针旋转90°
                if (j1 == j2 && j1 + 1 < n && grid[i2][j1 + 1] == 0) {
                    move(i1, j1, i1, j1 + 1);
                }
            }
            ++ans;
        }
        return -1;
    }
};
```

#### Go

```go
func minimumMoves(grid [][]int) int {
	n := len(grid)
	type pair struct{ a, b int }
	target := pair{n*n - 2, n*n - 1}
	q := []pair{pair{0, 1}}
	vis := make([][2]bool, n*n)
	vis[0][0] = true

	move := func(i1, j1, i2, j2 int) {
		if i1 >= 0 && i1 < n && j1 >= 0 && j1 < n && i2 >= 0 && i2 < n && j2 >= 0 && j2 < n {
			a, b := i1*n+j1, i2*n+j2
			status := 1
			if i1 == i2 {
				status = 0
			}
			if !vis[a][status] && grid[i1][j1] == 0 && grid[i2][j2] == 0 {
				q = append(q, pair{a, b})
				vis[a][status] = true
			}
		}
	}

	ans := 0
	for len(q) > 0 {
		for k := len(q); k > 0; k-- {
			p := q[0]
			q = q[1:]
			if p == target {
				return ans
			}
			a, b := p.a, p.b
			i1, j1 := a/n, a%n
			i2, j2 := b/n, b%n
			// 尝试向右平移（保持身体水平/垂直状态）
			move(i1, j1+1, i2, j2+1)
			// 尝试向下平移（保持身体水平/垂直状态）
			move(i1+1, j1, i2+1, j2)
			// 当前处于水平状态，且 grid[i1 + 1][j2] 无障碍，尝试顺时针旋转90°
			if i1 == i2 && i1+1 < n && grid[i1+1][j2] == 0 {
				move(i1, j1, i1+1, j1)
			}
			// 当前处于垂直状态，且 grid[i2][j1 + 1] 无障碍，尝试逆时针旋转90°
			if j1 == j2 && j1+1 < n && grid[i2][j1+1] == 0 {
				move(i1, j1, i1, j1+1)
			}
		}
		ans++
	}
	return -1
}
```

#### TypeScript

```ts
function minimumMoves(grid: number[][]): number {
    const n = grid.length;
    const target: number[] = [n * n - 2, n * n - 1];
    const q: number[][] = [[0, 1]];
    const vis = Array.from({ length: n * n }, () => Array(2).fill(false));
    vis[0][0] = true;

    const move = (i1: number, j1: number, i2: number, j2: number) => {
        if (i1 >= 0 && i1 < n && j1 >= 0 && j1 < n && i2 >= 0 && i2 < n && j2 >= 0 && j2 < n) {
            const a = i1 * n + j1;
            const b = i2 * n + j2;
            const status = i1 === i2 ? 0 : 1;
            if (!vis[a][status] && grid[i1][j1] == 0 && grid[i2][j2] == 0) {
                q.push([a, b]);
                vis[a][status] = true;
            }
        }
    };

    let ans = 0;
    while (q.length) {
        for (let k = q.length; k; --k) {
            const p: number[] = q.shift();
            if (p[0] === target[0] && p[1] === target[1]) {
                return ans;
            }
            const [i1, j1] = [~~(p[0] / n), p[0] % n];
            const [i2, j2] = [~~(p[1] / n), p[1] % n];
            // 尝试向右平移（保持身体水平/垂直状态）
            move(i1, j1 + 1, i2, j2 + 1);
            // 尝试向下平移（保持身体水平/垂直状态）
            move(i1 + 1, j1, i2 + 1, j2);
            // 当前处于水平状态，且 grid[i1 + 1][j2] 无障碍，尝试顺时针旋转90°
            if (i1 == i2 && i1 + 1 < n && grid[i1 + 1][j2] == 0) {
                move(i1, j1, i1 + 1, j1);
            }
            // 当前处于垂直状态，且 grid[i2][j1 + 1] 无障碍，尝试逆时针旋转90°
            if (j1 == j2 && j1 + 1 < n && grid[i2][j1 + 1] == 0) {
                move(i1, j1, i1, j1 + 1);
            }
        }
        ++ans;
    }
    return -1;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var minimumMoves = function (grid) {
    const n = grid.length;
    const target = [n * n - 2, n * n - 1];
    const q = [[0, 1]];
    const vis = Array.from({ length: n * n }, () => Array(2).fill(false));
    vis[0][0] = true;

    const move = (i1, j1, i2, j2) => {
        if (i1 >= 0 && i1 < n && j1 >= 0 && j1 < n && i2 >= 0 && i2 < n && j2 >= 0 && j2 < n) {
            const a = i1 * n + j1;
            const b = i2 * n + j2;
            const status = i1 === i2 ? 0 : 1;
            if (!vis[a][status] && grid[i1][j1] == 0 && grid[i2][j2] == 0) {
                q.push([a, b]);
                vis[a][status] = true;
            }
        }
    };

    let ans = 0;
    while (q.length) {
        for (let k = q.length; k; --k) {
            const p = q.shift();
            if (p[0] === target[0] && p[1] === target[1]) {
                return ans;
            }
            const [i1, j1] = [~~(p[0] / n), p[0] % n];
            const [i2, j2] = [~~(p[1] / n), p[1] % n];
            // 尝试向右平移（保持身体水平/垂直状态）
            move(i1, j1 + 1, i2, j2 + 1);
            // 尝试向下平移（保持身体水平/垂直状态）
            move(i1 + 1, j1, i2 + 1, j2);
            // 当前处于水平状态，且 grid[i1 + 1][j2] 无障碍，尝试顺时针旋转90°
            if (i1 == i2 && i1 + 1 < n && grid[i1 + 1][j2] == 0) {
                move(i1, j1, i1 + 1, j1);
            }
            // 当前处于垂直状态，且 grid[i2][j1 + 1] 无障碍，尝试逆时针旋转90°
            if (j1 == j2 && j1 + 1 < n && grid[i2][j1 + 1] == 0) {
                move(i1, j1, i1, j1 + 1);
            }
        }
        ++ans;
    }
    return -1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
