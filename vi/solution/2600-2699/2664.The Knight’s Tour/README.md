---
comments: true
difficulty: Medium
tags:
    - Array
    - Backtracking
    - Matrix
---

<!-- problem:start -->

# [2664. The Knight’s Tour 🔒](https://leetcode.com/problems/the-knights-tour)

[中文文档](/solution/2600-2699/2664.The%20Knight%E2%80%99s%20Tour/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>m</code> và <code>n</code> lần lượt là chiều cao và chiều rộng của mảng 2D <code>board</code> được đánh chỉ số từ <strong>0</strong>, cùng một cặp số nguyên dương <code>(r, c)</code> là vị trí bắt đầu của quân mã trên bàn cờ.</p>

<p>Nhiệm vụ của bạn là tìm thứ tự di chuyển của quân mã sao cho mọi ô trên <code>board</code> đều được thăm <strong>đúng</strong> một lần (ô bắt đầu được xem là đã thăm và <strong>không được</strong> thăm lại).</p>

<p>Trả về <em>mảng</em> <code>board</code> <em>trong đó giá trị của các ô cho biết thứ tự thăm ô, bắt đầu từ 0 (vị trí ban đầu của quân mã).</em></p>

<p>Lưu ý rằng một <strong>quân mã</strong> có thể <strong>di chuyển</strong> từ ô <code>(r1, c1)</code> đến ô <code>(r2, c2)</code> nếu <code>0 &lt;= r2 &lt;= m - 1</code>, <code>0 &lt;= c2 &lt;= n - 1</code>, <code>min(abs(r1 - r2), abs(c1 - c2)) = 1</code> và <code>max(abs(r1 - r2), abs(c1 - c2)) = 2</code>.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 1, n = 1, r = 0, c = 0
<strong>Đầu ra:</strong> [[0]]
<strong>Giải thích:</strong> Chỉ có 1 ô và quân mã ban đầu ở đó, nên lưới 1x1 chỉ chứa một số 0.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 3, n = 4, r = 0, c = 0
<strong>Đầu ra:</strong> [[0,3,6,9],[11,8,1,4],[2,5,10,7]]
<strong>Giải thích:</strong> Với thứ tự di chuyển sau, ta có thể đi qua toàn bộ bàn cờ.
(0,0)-&gt;(1,2)-&gt;(2,0)-&gt;(0,1)-&gt;(1,3)-&gt;(2,1)-&gt;(0,2)-&gt;(2,3)-&gt;(1,1)-&gt;(0,3)-&gt;(2,2)-&gt;(1,0)</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 5</code></li>
	<li><code>0 &lt;= r &lt;= m - 1</code></li>
	<li><code>0 &lt;= c &lt;= n - 1</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <strong>ít nhất</strong> một thứ tự di chuyển thỏa mãn điều kiện đã cho luôn tồn tại.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quay lui

<!-- thinking:start -->

> **Tư duy**
>
> Quân mã phải đi qua mọi ô đúng một lần. Bàn cờ có kích thước tối đa $5 \times 5$, nên ta có thể dùng quay lui. Thử tám ô lân cận chưa được thăm và dừng khi số bước đạt $mn-1$.
>
> Với các nhánh thất bại, ta hoàn tác giá trị đã ghi. Chỉ cần tìm được đường đi Hamilton đầu tiên là đủ.

<!-- thinking:end -->

Ta tạo một mảng hai chiều $g$ để ghi lại thứ tự di chuyển của quân mã, ban đầu $g[r][c] = -1$ và mọi vị trí khác cũng được đặt là $-1$. Ngoài ra, ta cần một biến $ok$ để ghi nhận liệu đã tìm được lời giải hay chưa.

Tiếp theo, ta bắt đầu tìm kiếm theo chiều sâu từ $(r, c)$. Mỗi khi tìm kiếm tại vị trí $(i, j)$, trước tiên ta kiểm tra xem $g[i][j]$ có bằng $m \times n - 1$ hay không. Nếu có, nghĩa là ta đã tìm được lời giải, khi đó đặt $ok$ thành `true` rồi trả về. Nếu không, ta liệt kê tám hướng di chuyển có thể có của quân mã đến vị trí $(x, y)$. Nếu $0 \leq x < m$, $0 \leq y < n$ và $g[x][y]=-1$, ta cập nhật $g[x][y]$ thành $g[i][j]+1$ rồi đệ quy tìm kiếm tại vị trí $(x, y)$. Nếu sau lần tìm kiếm đó biến $ok$ là `true`, ta trả về ngay. Ngược lại, đặt lại $g[x][y]$ thành $-1$ và tiếp tục tìm kiếm theo các hướng khác.

Cuối cùng, trả về mảng hai chiều $g$.

Độ phức tạp thời gian là $O(8^{m \times n})$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ là các số nguyên được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tourOfKnight(self, m: int, n: int, r: int, c: int) -> List[List[int]]:
        def dfs(i: int, j: int):
            nonlocal ok
            if g[i][j] == m * n - 1:
                ok = True
                return
            for a, b in pairwise((-2, -1, 2, 1, -2, 1, 2, -1, -2)):
                x, y = i + a, j + b
                if 0 <= x < m and 0 <= y < n and g[x][y] == -1:
                    g[x][y] = g[i][j] + 1
                    dfs(x, y)
                    if ok:
                        return
                    g[x][y] = -1

        g = [[-1] * n for _ in range(m)]
        g[r][c] = 0
        ok = False
        dfs(r, c)
        return g
```

#### Java

```java
class Solution {
    private int[][] g;
    private int m;
    private int n;
    private boolean ok;

    public int[][] tourOfKnight(int m, int n, int r, int c) {
        this.m = m;
        this.n = n;
        this.g = new int[m][n];
        for (var row : g) {
            Arrays.fill(row, -1);
        }
        g[r][c] = 0;
        dfs(r, c);
        return g;
    }

    private void dfs(int i, int j) {
        if (g[i][j] == m * n - 1) {
            ok = true;
            return;
        }
        int[] dirs = {-2, -1, 2, 1, -2, 1, 2, -1, -2};
        for (int k = 0; k < 8; ++k) {
            int x = i + dirs[k], y = j + dirs[k + 1];
            if (x >= 0 && x < m && y >= 0 && y < n && g[x][y] == -1) {
                g[x][y] = g[i][j] + 1;
                dfs(x, y);
                if (ok) {
                    return;
                }
                g[x][y] = -1;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> tourOfKnight(int m, int n, int r, int c) {
        vector<vector<int>> g(m, vector<int>(n, -1));
        g[r][c] = 0;
        int dirs[9] = {-2, -1, 2, 1, -2, 1, 2, -1, -2};
        bool ok = false;
        function<void(int, int)> dfs = [&](int i, int j) {
            if (g[i][j] == m * n - 1) {
                ok = true;
                return;
            }
            for (int k = 0; k < 8; ++k) {
                int x = i + dirs[k], y = j + dirs[k + 1];
                if (x >= 0 && x < m && y >= 0 && y < n && g[x][y] == -1) {
                    g[x][y] = g[i][j] + 1;
                    dfs(x, y);
                    if (ok) {
                        return;
                    }
                    g[x][y] = -1;
                }
            }
        };
        dfs(r, c);
        return g;
    }
};
```

#### Go

```go
func tourOfKnight(m int, n int, r int, c int) [][]int {
	g := make([][]int, m)
	for i := range g {
		g[i] = make([]int, n)
		for j := range g[i] {
			g[i][j] = -1
		}
	}
	g[r][c] = 0
	ok := false
	var dfs func(i, j int)
	dfs = func(i, j int) {
		if g[i][j] == m*n-1 {
			ok = true
			return
		}
		dirs := []int{-2, -1, 2, 1, -2, 1, 2, -1, -2}
		for k := 0; k < 8; k++ {
			x, y := i+dirs[k], j+dirs[k+1]
			if x >= 0 && x < m && y >= 0 && y < n && g[x][y] == -1 {
				g[x][y] = g[i][j] + 1
				dfs(x, y)
				if ok {
					return
				}
				g[x][y] = -1
			}
		}
	}
	dfs(r, c)
	return g
}
```

#### TypeScript

```ts
function tourOfKnight(m: number, n: number, r: number, c: number): number[][] {
    const g: number[][] = Array.from({ length: m }, () => Array(n).fill(-1));
    const dirs = [-2, -1, 2, 1, -2, 1, 2, -1, -2];
    let ok = false;
    const dfs = (i: number, j: number) => {
        if (g[i][j] === m * n - 1) {
            ok = true;
            return;
        }
        for (let k = 0; k < 8; ++k) {
            const [x, y] = [i + dirs[k], j + dirs[k + 1]];
            if (x >= 0 && x < m && y >= 0 && y < n && g[x][y] === -1) {
                g[x][y] = g[i][j] + 1;
                dfs(x, y);
                if (ok) {
                    return;
                }
                g[x][y] = -1;
            }
        }
    };
    g[r][c] = 0;
    dfs(r, c);
    return g;
}
```

#### Rust

```rust
impl Solution {
    pub fn tour_of_knight(m: i32, n: i32, r: i32, c: i32) -> Vec<Vec<i32>> {
        let mut g: Vec<Vec<i32>> = vec![vec![-1; n as usize]; m as usize];
        g[r as usize][c as usize] = 0;
        let dirs: [i32; 9] = [-2, -1, 2, 1, -2, 1, 2, -1, -2];
        let mut ok = false;

        fn dfs(
            i: usize,
            j: usize,
            g: &mut Vec<Vec<i32>>,
            m: i32,
            n: i32,
            dirs: &[i32; 9],
            ok: &mut bool,
        ) {
            if g[i][j] == m * n - 1 {
                *ok = true;
                return;
            }
            for k in 0..8 {
                let x = ((i as i32) + dirs[k]) as usize;
                let y = ((j as i32) + dirs[k + 1]) as usize;
                if x < (m as usize) && y < (n as usize) && g[x][y] == -1 {
                    g[x][y] = g[i][j] + 1;
                    dfs(x, y, g, m, n, dirs, ok);
                    if *ok {
                        return;
                    }
                    g[x][y] = -1;
                }
            }
        }

        dfs(r as usize, c as usize, &mut g, m, n, &dirs, &mut ok);
        g
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
