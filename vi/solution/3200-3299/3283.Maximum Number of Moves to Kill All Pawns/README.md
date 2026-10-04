---
comments: true
difficulty: Hard
rating: 2473
source: Weekly Contest 414 Q4
tags:
    - Bit Manipulation
    - Breadth-First Search
    - Array
    - Math
    - Bitmask
    - Game Theory
---

<!-- problem:start -->

# [3283. Maximum Number of Moves to Kill All Pawns](https://leetcode.com/problems/maximum-number-of-moves-to-kill-all-pawns)

[中文文档](/solution/3200-3299/3283.Maximum%20Number%20of%20Moves%20to%20Kill%20All%20Pawns/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bàn cờ <code>50 x 50</code> với <strong>một</strong> quân mã và một số quân tốt trên đó. Cho hai số nguyên <code>kx</code> và <code>ky</code>, trong đó <code>(kx, ky)</code> biểu thị vị trí của quân mã, và một mảng 2 chiều <code>positions</code>, trong đó <code>positions[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu thị vị trí của các quân tốt trên bàn cờ.</p>

<p>Alice và Bob chơi một trò chơi <em>theo lượt</em>, trong đó Alice đi trước. Trong lượt của mỗi người chơi:</p>

<ul>
	<li>Người chơi <em>chọn </em>một quân tốt vẫn còn trên bàn cờ và dùng quân mã để bắt nó trong số <strong>nước đi</strong> <strong>ít nhất</strong> có thể. <strong>Lưu ý</strong> rằng người chơi có thể chọn <strong>bất kỳ</strong> quân tốt nào, <strong>có thể không phải</strong> quân tốt có thể bị bắt trong <strong>ít nước nhất</strong>.</li>
	<li><span>Trong quá trình bắt quân tốt <em>được chọn</em>, quân mã <strong>có thể</strong> đi qua các quân tốt khác mà <strong>không</strong> bắt chúng</span>. <strong>Chỉ</strong> quân tốt <em>được chọn</em> mới có thể bị bắt trong <em>lượt này</em>.</li>
</ul>

<p>Alice muốn <strong>tối đa hóa</strong> <strong>tổng</strong> số nước đi do <em>cả hai người chơi</em> thực hiện cho đến khi không còn quân tốt nào trên bàn cờ, trong khi Bob muốn <strong>tối thiểu hóa</strong> tổng số nước đi đó.</p>

<p>Trả về <strong>tổng</strong> số nước đi <em>lớn nhất</em> được thực hiện trong trò chơi mà Alice có thể đạt được, giả sử cả hai người chơi đều chơi <strong>tối ưu</strong>.</p>

<p>Lưu ý rằng trong <strong>một nước đi</strong>, quân mã trên bàn cờ có thể di chuyển đến một trong tám vị trí, như minh họa bên dưới. Mỗi nước đi gồm hai ô theo một hướng chính, sau đó một ô theo hướng vuông góc.</p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3283.Maximum%20Number%20of%20Moves%20to%20Kill%20All%20Pawns/images/chess_knight.jpg" style="width: 275px; height: 273px;" /></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">kx = 1, ky = 1, positions = [[0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3283.Maximum%20Number%20of%20Moves%20to%20Kill%20All%20Pawns/images/gif3.gif" style="width: 275px; height: 275px;" /></p>

<p>Quân mã cần 4 nước đi để đến quân tốt tại <code>(0, 0)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">kx = 0, ky = 2, positions = [[1,1],[2,2],[3,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3283.Maximum%20Number%20of%20Moves%20to%20Kill%20All%20Pawns/images/gif4.gif" style="width: 320px; height: 320px;" /></strong></p>

<ul>
	<li>Alice chọn quân tốt tại <code>(2, 2)</code> và bắt nó trong hai nước đi: <code>(0, 2) -&gt; (1, 4) -&gt; (2, 2)</code>.</li>
	<li>Bob chọn quân tốt tại <code>(3, 3)</code> và bắt nó trong hai nước đi: <code>(2, 2) -&gt; (4, 1) -&gt; (3, 3)</code>.</li>
	<li>Alice chọn quân tốt tại <code>(1, 1)</code> và bắt nó trong bốn nước đi: <code>(3, 3) -&gt; (4, 1) -&gt; (2, 2) -&gt; (0, 3) -&gt; (1, 1)</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">kx = 0, ky = 0, positions = [[1,2],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Alice chọn quân tốt tại <code>(2, 4)</code> và bắt nó trong hai nước đi: <code>(0, 0) -&gt; (1, 2) -&gt; (2, 4)</code>. Lưu ý rằng quân tốt tại <code>(1, 2)</code> không bị bắt.</li>
	<li>Bob chọn quân tốt tại <code>(1, 2)</code> và bắt nó trong một nước đi: <code>(2, 4) -&gt; (1, 2)</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= kx, ky &lt;= 49</code></li>
	<li><code>1 &lt;= positions.length &lt;= 15</code></li>
	<li><code>positions[i].length == 2</code></li>
	<li><code>0 &lt;= positions[i][0], positions[i][1] &lt;= 49</code></li>
	<li>Tất cả <code>positions[i]</code> đều khác nhau.</li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>positions[i] != [kx, ky]</code> với mọi <code>0 &lt;= i &lt; positions.length</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS + Nén trạng thái + Ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Quân mã phải bắt nhiều nhất $15$ quân tốt, Alice muốn tối đa hóa tổng số nước đi còn Bob muốn tối thiểu hóa. Bàn cờ có kích thước $50\times 50$; chạy BFS từ mỗi quân tốt sẽ cho khoảng cách của quân mã. Có $15!$ thứ tự bắt nên ta cần một trò chơi trên tập con.
>
> $\textit{dfs}(last,state,k)$: $last$ là quân tốt bị bắt cuối cùng (ban đầu là quân mã), $state$ là tập các quân tốt còn lại, còn $k$ cho biết lượt của ai. Alice chọn giá trị lớn nhất, Bob chọn giá trị nhỏ nhất, sử dụng $dist[last][x][y]$. Các trạng thái được ghi nhớ có số lượng $O(n\cdot 2^n)$.

<!-- thinking:end -->

Trước hết, ta tiền xử lý khoảng cách ngắn nhất từ mỗi quân tốt đến mọi vị trí trên bàn cờ và lưu chúng trong mảng $\textit{dist}$, trong đó $\textit{dist}[i][x][y]$ biểu thị khoảng cách ngắn nhất từ quân tốt thứ $i$ đến vị trí $(x, y)$.

Tiếp theo, ta xây dựng hàm $\text{dfs}(\textit{last}, \textit{state}, \textit{k})$, trong đó $\textit{last}$ biểu thị chỉ số của quân tốt bị bắt cuối cùng, $\textit{state}$ biểu thị trạng thái hiện tại của các quân tốt còn lại, và $\textit{k}$ biểu thị đang là lượt của Alice hay Bob. Hàm trả về số nước đi lớn nhất cho lượt hiện tại. Đáp án là $\text{dfs}(n, 2^n-1, 1)$. Ban đầu, chỉ số của quân tốt bị bắt cuối cùng là $n$, cũng chính là vị trí của quân mã.

Cài đặt cụ thể của hàm $\text{dfs}$ như sau:

- Nếu $\textit{state} = 0$, nghĩa là không còn quân tốt nào, trả về $0$;
- Nếu $\textit{k} = 1$, nghĩa là đến lượt Alice. Ta cần tìm một quân tốt sao cho số nước đi sau khi bắt quân tốt đó là lớn nhất, tức là $\text{dfs}(i, \textit{state} \oplus 2^i, \textit{k} \oplus 1) + \textit{dist}[\textit{last}][x][y]$;
- Nếu $\textit{k} = 0$, nghĩa là đến lượt Bob. Ta cần tìm một quân tốt sao cho số nước đi sau khi bắt quân tốt đó là nhỏ nhất, tức là $\text{dfs}(i, \textit{state} \oplus 2^i, \textit{k} \oplus 1) + \textit{dist}[\textit{last}][x][y]$.

Để tránh tính toán lặp lại, ta sử dụng memoization, tức là dùng một hash table để lưu các trạng thái đã được tính.

Độ phức tạp thời gian là $O(n \times m^2 + 2^n \times n^2)$, và độ phức tạp không gian là $O(n \times m^2 + 2^n \times n)$. Ở đây, $n$ và $m$ lần lượt là số quân tốt và kích thước bàn cờ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxMoves(self, kx: int, ky: int, positions: List[List[int]]) -> int:
        @cache
        def dfs(last: int, state: int, k: int) -> int:
            if state == 0:
                return 0
            if k:
                res = 0
                for i, (x, y) in enumerate(positions):
                    if state >> i & 1:
                        t = dfs(i, state ^ (1 << i), k ^ 1) + dist[last][x][y]
                        if res < t:
                            res = t
                return res
            else:
                res = inf
                for i, (x, y) in enumerate(positions):
                    if state >> i & 1:
                        t = dfs(i, state ^ (1 << i), k ^ 1) + dist[last][x][y]
                        if res > t:
                            res = t
                return res

        n = len(positions)
        m = 50
        dist = [[[-1] * m for _ in range(m)] for _ in range(n + 1)]
        dx = [1, 1, 2, 2, -1, -1, -2, -2]
        dy = [2, -2, 1, -1, 2, -2, 1, -1]
        positions.append([kx, ky])
        for i, (x, y) in enumerate(positions):
            dist[i][x][y] = 0
            q = deque([(x, y)])
            step = 0
            while q:
                step += 1
                for _ in range(len(q)):
                    x1, y1 = q.popleft()
                    for j in range(8):
                        x2, y2 = x1 + dx[j], y1 + dy[j]
                        if 0 <= x2 < m and 0 <= y2 < m and dist[i][x2][y2] == -1:
                            dist[i][x2][y2] = step
                            q.append((x2, y2))

        ans = dfs(n, (1 << n) - 1, 1)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private Integer[][][] f;
    private Integer[][][] dist;
    private int[][] positions;
    private final int[] dx = {1, 1, 2, 2, -1, -1, -2, -2};
    private final int[] dy = {2, -2, 1, -1, 2, -2, 1, -1};

    public int maxMoves(int kx, int ky, int[][] positions) {
        int n = positions.length;
        final int m = 50;
        dist = new Integer[n + 1][m][m];
        this.positions = positions;
        for (int i = 0; i <= n; ++i) {
            int x = i < n ? positions[i][0] : kx;
            int y = i < n ? positions[i][1] : ky;
            Deque<int[]> q = new ArrayDeque<>();
            q.offer(new int[] {x, y});
            for (int step = 1; !q.isEmpty(); ++step) {
                for (int k = q.size(); k > 0; --k) {
                    var p = q.poll();
                    int x1 = p[0], y1 = p[1];
                    for (int j = 0; j < 8; ++j) {
                        int x2 = x1 + dx[j], y2 = y1 + dy[j];
                        if (x2 >= 0 && x2 < m && y2 >= 0 && y2 < m && dist[i][x2][y2] == null) {
                            dist[i][x2][y2] = step;
                            q.offer(new int[] {x2, y2});
                        }
                    }
                }
            }
        }
        f = new Integer[n + 1][1 << n][2];
        return dfs(n, (1 << n) - 1, 1);
    }

    private int dfs(int last, int state, int k) {
        if (state == 0) {
            return 0;
        }
        if (f[last][state][k] != null) {
            return f[last][state][k];
        }
        int res = k == 1 ? 0 : Integer.MAX_VALUE;
        for (int i = 0; i < positions.length; ++i) {
            int x = positions[i][0], y = positions[i][1];
            if ((state >> i & 1) == 1) {
                int t = dfs(i, state ^ (1 << i), k ^ 1) + dist[last][x][y];
                res = k == 1 ? Math.max(res, t) : Math.min(res, t);
            }
        }
        return f[last][state][k] = res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxMoves(int kx, int ky, vector<vector<int>>& positions) {
        int n = positions.size();
        const int m = 50;
        const int dx[8] = {1, 1, 2, 2, -1, -1, -2, -2};
        const int dy[8] = {2, -2, 1, -1, 2, -2, 1, -1};
        int dist[n + 1][m][m];
        memset(dist, -1, sizeof(dist));
        for (int i = 0; i <= n; ++i) {
            int x = (i < n) ? positions[i][0] : kx;
            int y = (i < n) ? positions[i][1] : ky;
            queue<pair<int, int>> q;
            q.push({x, y});
            dist[i][x][y] = 0;
            for (int step = 1; !q.empty(); ++step) {
                for (int k = q.size(); k > 0; --k) {
                    auto [x1, y1] = q.front();
                    q.pop();
                    for (int j = 0; j < 8; ++j) {
                        int x2 = x1 + dx[j], y2 = y1 + dy[j];
                        if (x2 >= 0 && x2 < m && y2 >= 0 && y2 < m && dist[i][x2][y2] == -1) {
                            dist[i][x2][y2] = step;
                            q.push({x2, y2});
                        }
                    }
                }
            }
        }

        int f[n + 1][1 << n][2];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int last, int state, int k) -> int {
            if (state == 0) {
                return 0;
            }
            if (f[last][state][k] != -1) {
                return f[last][state][k];
            }
            int res = (k == 1) ? 0 : INT_MAX;

            for (int i = 0; i < positions.size(); ++i) {
                int x = positions[i][0], y = positions[i][1];
                if ((state >> i) & 1) {
                    int t = dfs(i, state ^ (1 << i), k ^ 1) + dist[last][x][y];
                    if (k == 1) {
                        res = max(res, t);
                    } else {
                        res = min(res, t);
                    }
                }
            }
            return f[last][state][k] = res;
        };
        return dfs(n, (1 << n) - 1, 1);
    }
};
```

#### Go

```go
func maxMoves(kx int, ky int, positions [][]int) int {
	n := len(positions)
	const m = 50
	dx := []int{1, 1, 2, 2, -1, -1, -2, -2}
	dy := []int{2, -2, 1, -1, 2, -2, 1, -1}
	dist := make([][][]int, n+1)
	for i := range dist {
		dist[i] = make([][]int, m)
		for j := range dist[i] {
			dist[i][j] = make([]int, m)
			for k := range dist[i][j] {
				dist[i][j][k] = -1
			}
		}
	}

	for i := 0; i <= n; i++ {
		x := kx
		y := ky
		if i < n {
			x = positions[i][0]
			y = positions[i][1]
		}
		q := [][2]int{[2]int{x, y}}
		dist[i][x][y] = 0

		for step := 1; len(q) > 0; step++ {
			for k := len(q); k > 0; k-- {
				p := q[0]
				q = q[1:]
				x1, y1 := p[0], p[1]
				for j := 0; j < 8; j++ {
					x2 := x1 + dx[j]
					y2 := y1 + dy[j]
					if x2 >= 0 && x2 < m && y2 >= 0 && y2 < m && dist[i][x2][y2] == -1 {
						dist[i][x2][y2] = step
						q = append(q, [2]int{x2, y2})
					}
				}
			}
		}
	}
	f := make([][][]int, n+1)
	for i := range f {
		f[i] = make([][]int, 1<<n)
		for j := range f[i] {
			f[i][j] = make([]int, 2)
			for k := range f[i][j] {
				f[i][j][k] = -1
			}
		}
	}
	var dfs func(last, state, k int) int
	dfs = func(last, state, k int) int {
		if state == 0 {
			return 0
		}
		if f[last][state][k] != -1 {
			return f[last][state][k]
		}

		var res int
		if k == 0 {
			res = math.MaxInt32
		}

		for i, p := range positions {
			x, y := p[0], p[1]
			if (state>>i)&1 == 1 {
				t := dfs(i, state^(1<<i), k^1) + dist[last][x][y]
				if k == 1 {
					res = max(res, t)
				} else {
					res = min(res, t)
				}
			}
		}
		f[last][state][k] = res
		return res
	}

	return dfs(n, (1<<n)-1, 1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
