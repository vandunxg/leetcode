---
comments: true
difficulty: Medium
rating: 1708
source: Biweekly Contest 77 Q3
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [2257. Count Unguarded Cells in the Grid](https://leetcode.com/problems/count-unguarded-cells-in-the-grid)

[中文文档](/solution/2200-2299/2257.Count%20Unguarded%20Cells%20in%20the%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>m</code> và <code>n</code>, lần lượt biểu diễn một lưới <code>m x n</code> được đánh chỉ số từ <strong>0</strong>. Bạn cũng được cho hai mảng số nguyên hai chiều <code>guards</code> và <code>walls</code>, trong đó <code>guards[i] = [row<sub>i</sub>, col<sub>i</sub>]</code> và <code>walls[j] = [row<sub>j</sub>, col<sub>j</sub>]</code> lần lượt biểu diễn vị trí của người bảo vệ thứ <code>i<sup>th</sup></code> và bức tường thứ <code>j<sup>th</sup></code>.</p>

<p>Một người bảo vệ có thể nhìn thấy <b>mọi</b> ô theo bốn hướng chính (bắc, đông, nam hoặc tây) bắt đầu từ vị trí của mình, trừ khi bị <strong>cản</strong> bởi một bức tường hoặc người bảo vệ khác. Một ô được gọi là <strong>được bảo vệ</strong> nếu có <strong>ít nhất</strong> một người bảo vệ có thể nhìn thấy ô đó.</p>

<p>Trả về <em>số ô trống <strong>không</strong> <strong>được bảo vệ</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2257.Count%20Unguarded%20Cells%20in%20the%20Grid/images/example1drawio2.png" style="width: 300px; height: 204px;" />
<pre>
<strong>Đầu vào:</strong> m = 4, n = 6, guards = [[0,0],[1,1],[2,3]], walls = [[0,1],[2,2],[1,4]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các ô được bảo vệ và không được bảo vệ lần lượt được hiển thị bằng màu đỏ và xanh lá trong hình trên.
Có tổng cộng 7 ô không được bảo vệ, vì vậy ta trả về 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2257.Count%20Unguarded%20Cells%20in%20the%20Grid/images/example2drawio.png" style="width: 200px; height: 201px;" />
<pre>
<strong>Đầu vào:</strong> m = 3, n = 3, guards = [[1,1]], walls = [[0,1],[1,0],[2,1],[1,2]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các ô không được bảo vệ được hiển thị bằng màu xanh lá trong hình trên.
Có tổng cộng 4 ô không được bảo vệ, vì vậy ta trả về 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= m * n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= guards.length, walls.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>2 &lt;= guards.length + walls.length &lt;= m * n</code></li>
	<li><code>guards[i].length == walls[j].length == 2</code></li>
	<li><code>0 &lt;= row<sub>i</sub>, row<sub>j</sub> &lt; m</code></li>
	<li><code>0 &lt;= col<sub>i</sub>, col<sub>j</sub> &lt; n</code></li>
	<li>Tất cả vị trí trong <code>guards</code> và <code>walls</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Người bảo vệ nhìn dọc theo một hàng hoặc cột cho đến khi gặp tường. Vì $mn \le 10^5$, ta không nên quét lại toàn bộ một dòng từ đầu cho mỗi người bảo vệ. Việc đi theo bốn tia từ mỗi người bảo vệ cho đến khi gặp tường sẽ đi qua mỗi ô một số lần không đổi.
>
> Đánh dấu tường và người bảo vệ là $2$, các ô nhìn thấy là $1$, đồng thời dừng một tia khi gặp $2$. Đáp án là số ô còn lại có giá trị bằng 0.

<!-- thinking:end -->

Ta tạo một mảng hai chiều $g$ có kích thước $m \times n$, trong đó $g[i][j]$ biểu diễn ô ở hàng $i$ và cột $j$. Ban đầu, giá trị của $g[i][j]$ là $0$, cho biết ô chưa được bảo vệ.

Sau đó, ta duyệt qua tất cả người bảo vệ và tường, rồi đặt giá trị của $g[i][j]$ thành $2$, cho biết các vị trí này không thể đi qua.

Tiếp theo, ta duyệt qua các vị trí của người bảo vệ, mô phỏng theo bốn hướng từ vị trí đó cho đến khi gặp tường hoặc người bảo vệ, hoặc đi ra ngoài biên. Trong quá trình mô phỏng, ta đặt giá trị của ô đi qua thành $1$, cho biết ô đó được bảo vệ.

Cuối cùng, ta duyệt qua $g$ và đếm số ô có giá trị bằng $0$, đó chính là đáp án.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của lưới.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countUnguarded(
        self, m: int, n: int, guards: List[List[int]], walls: List[List[int]]
    ) -> int:
        g = [[0] * n for _ in range(m)]
        for i, j in guards:
            g[i][j] = 2
        for i, j in walls:
            g[i][j] = 2
        dirs = (-1, 0, 1, 0, -1)
        for i, j in guards:
            for a, b in pairwise(dirs):
                x, y = i, j
                while 0 <= x + a < m and 0 <= y + b < n and g[x + a][y + b] < 2:
                    x, y = x + a, y + b
                    g[x][y] = 1
        return sum(v == 0 for row in g for v in row)
```

#### Java

```java
class Solution {
    public int countUnguarded(int m, int n, int[][] guards, int[][] walls) {
        int[][] g = new int[m][n];
        for (var e : guards) {
            g[e[0]][e[1]] = 2;
        }
        for (var e : walls) {
            g[e[0]][e[1]] = 2;
        }
        int[] dirs = {-1, 0, 1, 0, -1};
        for (var e : guards) {
            for (int k = 0; k < 4; ++k) {
                int x = e[0], y = e[1];
                int a = dirs[k], b = dirs[k + 1];
                while (x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && g[x + a][y + b] < 2) {
                    x += a;
                    y += b;
                    g[x][y] = 1;
                }
            }
        }
        int ans = 0;
        for (var row : g) {
            for (int v : row) {
                if (v == 0) {
                    ++ans;
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
    int countUnguarded(int m, int n, vector<vector<int>>& guards, vector<vector<int>>& walls) {
        int g[m][n];
        memset(g, 0, sizeof(g));
        for (auto& e : guards) {
            g[e[0]][e[1]] = 2;
        }
        for (auto& e : walls) {
            g[e[0]][e[1]] = 2;
        }
        int dirs[5] = {-1, 0, 1, 0, -1};
        for (auto& e : guards) {
            for (int k = 0; k < 4; ++k) {
                int x = e[0], y = e[1];
                int a = dirs[k], b = dirs[k + 1];
                while (x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && g[x + a][y + b] < 2) {
                    x += a;
                    y += b;
                    g[x][y] = 1;
                }
            }
        }
        int ans = 0;
        for (auto& row : g) {
            ans += count(row, row + n, 0);
        }
        return ans;
    }
};
```

#### Go

```go
func countUnguarded(m int, n int, guards [][]int, walls [][]int) (ans int) {
	g := make([][]int, m)
	for i := range g {
		g[i] = make([]int, n)
	}
	for _, e := range guards {
		g[e[0]][e[1]] = 2
	}
	for _, e := range walls {
		g[e[0]][e[1]] = 2
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	for _, e := range guards {
		for k := 0; k < 4; k++ {
			x, y := e[0], e[1]
			a, b := dirs[k], dirs[k+1]
			for x+a >= 0 && x+a < m && y+b >= 0 && y+b < n && g[x+a][y+b] < 2 {
				x, y = x+a, y+b
				g[x][y] = 1
			}
		}
	}
	for _, row := range g {
		for _, v := range row {
			if v == 0 {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countUnguarded(m: number, n: number, guards: number[][], walls: number[][]): number {
    const g: number[][] = Array.from({ length: m }, () => Array.from({ length: n }, () => 0));
    for (const [i, j] of guards) {
        g[i][j] = 2;
    }
    for (const [i, j] of walls) {
        g[i][j] = 2;
    }
    const dirs: number[] = [-1, 0, 1, 0, -1];
    for (const [i, j] of guards) {
        for (let k = 0; k < 4; ++k) {
            let [x, y] = [i, j];
            let [a, b] = [dirs[k], dirs[k + 1]];
            while (x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && g[x + a][y + b] < 2) {
                x += a;
                y += b;
                g[x][y] = 1;
            }
        }
    }
    let ans = 0;
    for (const row of g) {
        for (const v of row) {
            ans += v === 0 ? 1 : 0;
        }
    }
    return ans;
}
```

#### JavaScript

```js
function countUnguarded(m, n, guards, walls) {
    const g = Array.from({ length: m }, () => Array.from({ length: n }, () => 0));
    for (const [i, j] of guards) {
        g[i][j] = 2;
    }
    for (const [i, j] of walls) {
        g[i][j] = 2;
    }
    const dirs = [-1, 0, 1, 0, -1];
    for (const [i, j] of guards) {
        for (let k = 0; k < 4; ++k) {
            let [x, y] = [i, j];
            let [a, b] = [dirs[k], dirs[k + 1]];
            while (x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && g[x + a][y + b] < 2) {
                x += a;
                y += b;
                g[x][y] = 1;
            }
        }
    }
    let ans = 0;
    for (const row of g) {
        for (const v of row) {
            ans += v === 0 ? 1 : 0;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_unguarded(m: i32, n: i32, guards: Vec<Vec<i32>>, walls: Vec<Vec<i32>>) -> i32 {
        let m = m as usize;
        let n = n as usize;
        let mut g = vec![vec![0; n]; m];
        for e in &guards {
            g[e[0] as usize][e[1] as usize] = 2;
        }
        for e in &walls {
            g[e[0] as usize][e[1] as usize] = 2;
        }
        let dirs = [-1, 0, 1, 0, -1];
        for e in &guards {
            let (x0, y0) = (e[0] as i32, e[1] as i32);
            for k in 0..4 {
                let (mut x, mut y) = (x0, y0);
                let (a, b) = (dirs[k], dirs[k + 1]);
                while x + a >= 0
                    && x + a < m as i32
                    && y + b >= 0
                    && y + b < n as i32
                    && g[(x + a) as usize][(y + b) as usize] < 2
                {
                    x += a;
                    y += b;
                    g[x as usize][y as usize] = 1;
                }
            }
        }
        let mut ans = 0;
        for row in g {
            for v in row {
                if v == 0 {
                    ans += 1;
                }
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int CountUnguarded(int m, int n, int[][] guards, int[][] walls) {
        int[,] g = new int[m, n];
        foreach (var e in guards) {
            g[e[0], e[1]] = 2;
        }
        foreach (var e in walls) {
            g[e[0], e[1]] = 2;
        }
        int[] dirs = { -1, 0, 1, 0, -1 };
        foreach (var e in guards) {
            int x0 = e[0], y0 = e[1];
            for (int k = 0; k < 4; ++k) {
                int x = x0, y = y0;
                int a = dirs[k], b = dirs[k + 1];
                while (x + a >= 0 && x + a < m && y + b >= 0 && y + b < n && g[x + a, y + b] < 2) {
                    x += a;
                    y += b;
                    g[x, y] = 1;
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (g[i, j] == 0) {
                    ++ans;
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
