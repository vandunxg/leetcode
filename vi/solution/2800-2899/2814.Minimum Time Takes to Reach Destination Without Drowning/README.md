---
comments: true
difficulty: Hard
tags:
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [2814. Minimum Time Takes to Reach Destination Without Drowning 🔒](https://leetcode.com/problems/minimum-time-takes-to-reach-destination-without-drowning)

[中文文档](/solution/2800-2899/2814.Minimum%20Time%20Takes%20to%20Reach%20Destination%20Without%20Drowning/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một lưới chuỗi <code>land</code> kích thước <code>n * m</code>, được <strong>đánh chỉ số từ 0</strong>. Hiện tại, bạn đang đứng tại ô chứa <code>&quot;S&quot;</code> và muốn đi đến ô chứa <code>&quot;D&quot;</code>. Trong vùng đất này còn có ba loại ô khác:</p>

<ul>
	<li><code>&quot;.&quot;</code>: Đây là các ô trống.</li>
	<li><code>&quot;X&quot;</code>: Đây là các ô đá.</li>
	<li><code>&quot;*&quot;</code>: Đây là các ô bị ngập.</li>
</ul>

<p>Mỗi giây, bạn có thể di chuyển đến một ô chung cạnh với ô hiện tại (nếu ô đó tồn tại). Đồng thời, mỗi giây, mọi <strong>ô trống</strong> chung cạnh với một ô bị ngập cũng sẽ bị ngập.<br />
Trên hành trình, bạn phải đối mặt với hai vấn đề:</p>

<ul>
	<li>Bạn không thể bước lên các ô đá.</li>
	<li>Bạn không thể bước lên các ô bị ngập vì sẽ bị chết đuối (đồng thời, bạn cũng không thể bước lên một ô sẽ bị ngập đúng lúc bạn bước vào đó).</li>
</ul>

<p>Hãy trả về <em>thời gian <strong>nhỏ nhất</strong> cần để bạn đến đích tính bằng giây, hoặc </em><code>-1</code><em> nếu không thể.</em></p>

<p><strong>Lưu ý</strong> rằng ô đích sẽ không bao giờ bị ngập.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> land = [[&quot;D&quot;,&quot;.&quot;,&quot;*&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;S&quot;,&quot;.&quot;]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Hình dưới đây mô phỏng trạng thái của vùng đất qua từng giây. Các ô màu xanh dương là ô bị ngập, còn các ô màu xám là ô đá.
Hình (0) biểu diễn trạng thái ban đầu và hình (3) biểu diễn trạng thái cuối cùng khi ta đến đích. Như bạn thấy, cần 3 giây để đến đích và đáp án là 3.
Có thể chứng minh rằng 3 là thời gian nhỏ nhất cần để đi từ S đến D.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2814.Minimum%20Time%20Takes%20to%20Reach%20Destination%20Without%20Drowning/images/ex1.png" style="padding: 5px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 600px; height: 111px;" /></p>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> land = [[&quot;D&quot;,&quot;X&quot;,&quot;*&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;S&quot;]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Hình dưới đây mô phỏng trạng thái của vùng đất qua từng giây. Các ô màu xanh dương là ô bị ngập, còn các ô màu xám là ô đá.
Hình (0) biểu diễn trạng thái ban đầu. Như bạn thấy, dù chọn đường đi nào, chúng ta cũng sẽ bị chết đuối ở giây thứ 3<sup>rd</sup>. Đồng thời, đường đi ngắn nhất cần 4 giây để đi từ S đến D.
Vì vậy, đáp án là -1.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2814.Minimum%20Time%20Takes%20to%20Reach%20Destination%20Without%20Drowning/images/ex2-2.png" style="padding: 7px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 600px; height: 107px;" /></p>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> land = [[&quot;D&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;*&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;X&quot;,&quot;.&quot;,&quot;X&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;S&quot;,&quot;.&quot;]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có thể chứng minh rằng ta có thể đến đích trong 6 giây. Đồng thời, có thể chứng minh rằng 6 là số giây nhỏ nhất cần để đi từ S đến D.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n, m &lt;= 100</code></li>
	<li><code>land</code>&nbsp;chỉ bao gồm&nbsp;<code>&quot;S&quot;</code>, <code>&quot;D&quot;</code>, <code>&quot;.&quot;</code>, <code>&quot;*&quot;</code> và <code>&quot;X&quot;</code>.</li>
	<li><strong>Chính xác</strong> một ô có giá trị bằng <code>&quot;S&quot;</code>.</li>
	<li><strong>Chính xác</strong> một ô có giá trị bằng <code>&quot;D&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lần duyệt BFS

<!-- thinking:start -->

> **Tư duy**
>
> Nước lan ra một ô mỗi giây và người đi không thể bước vào ô đã bị ngập. Lần BFS đa nguồn đầu tiên lưu thời điểm ngập $g[i][j]$ của mỗi ô. Lần BFS thứ hai bắt đầu từ điểm xuất phát và chỉ đi đến ô kề khi $g[x][y]>t+1$, biến ràng buộc ngập nước thành một phép so sánh tĩnh.

<!-- thinking:end -->

Đầu tiên, ta chạy BFS (Breadth-First Search) để tính khoảng cách ngắn nhất từ mỗi ô đến nước và lưu kết quả vào mảng $g$. Sau đó, ta chạy một BFS khác bắt đầu từ ô $(s_i, s_j)$ để tìm khoảng cách ngắn nhất đến ô đích $(d_i, d_j)$. Trong quá trình này, nếu ô kề $(x, y)$ của ô hiện tại $(i, j)$ thỏa mãn $g[x][y] > t + 1$, ta có thể di chuyển từ $(x, y)$ đến $(i, j)$.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của mảng $land$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSeconds(self, land: List[List[str]]) -> int:
        m, n = len(land), len(land[0])
        vis = [[False] * n for _ in range(m)]
        g = [[inf] * n for _ in range(m)]
        q = deque()
        si = sj = 0
        for i, row in enumerate(land):
            for j, c in enumerate(row):
                match c:
                    case "*":
                        q.append((i, j))
                    case "S":
                        si, sj = i, j
        dirs = (-1, 0, 1, 0, -1)
        t = 0
        while q:
            for _ in range(len(q)):
                i, j = q.popleft()
                g[i][j] = t
                for a, b in pairwise(dirs):
                    x, y = i + a, j + b
                    if (
                        0 <= x < m
                        and 0 <= y < n
                        and not vis[x][y]
                        and land[x][y] in ".S"
                    ):
                        vis[x][y] = True
                        q.append((x, y))
            t += 1
        t = 0
        q = deque([(si, sj)])
        vis = [[False] * n for _ in range(m)]
        vis[si][sj] = True
        while q:
            for _ in range(len(q)):
                i, j = q.popleft()
                if land[i][j] == "D":
                    return t
                for a, b in pairwise(dirs):
                    x, y = i + a, j + b
                    if (
                        0 <= x < m
                        and 0 <= y < n
                        and g[x][y] > t + 1
                        and not vis[x][y]
                        and land[x][y] in ".D"
                    ):
                        vis[x][y] = True
                        q.append((x, y))
            t += 1
        return -1
```

#### Java

```java
class Solution {
    public int minimumSeconds(List<List<String>> land) {
        int m = land.size(), n = land.get(0).size();
        boolean[][] vis = new boolean[m][n];
        int[][] g = new int[m][n];
        Deque<int[]> q = new ArrayDeque<>();
        int si = 0, sj = 0;
        for (int i = 0; i < m; ++i) {
            Arrays.fill(g[i], 1 << 30);
            for (int j = 0; j < n; ++j) {
                String c = land.get(i).get(j);
                if ("*".equals(c)) {
                    q.offer(new int[] {i, j});
                } else if ("S".equals(c)) {
                    si = i;
                    sj = j;
                }
            }
        }
        int[] dirs = {-1, 0, 1, 0, -1};
        for (int t = 0; !q.isEmpty(); ++t) {
            for (int k = q.size(); k > 0; --k) {
                int[] p = q.poll();
                int i = p[0], j = p[1];
                g[i][j] = t;
                for (int d = 0; d < 4; ++d) {
                    int x = i + dirs[d], y = j + dirs[d + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y]) {
                        boolean empty = ".".equals(land.get(x).get(y));
                        boolean start = "S".equals(land.get(x).get(y));
                        if (empty || start) {
                            vis[x][y] = true;
                            q.offer(new int[] {x, y});
                        }
                    }
                }
            }
        }
        q.offer(new int[] {si, sj});
        vis = new boolean[m][n];
        vis[si][sj] = true;
        for (int t = 0; !q.isEmpty(); ++t) {
            for (int k = q.size(); k > 0; --k) {
                int[] p = q.poll();
                int i = p[0], j = p[1];
                if ("D".equals(land.get(i).get(j))) {
                    return t;
                }
                for (int d = 0; d < 4; ++d) {
                    int x = i + dirs[d], y = j + dirs[d + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && g[x][y] > t + 1) {
                        boolean empty = ".".equals(land.get(x).get(y));
                        boolean dest = "D".equals(land.get(x).get(y));
                        if (empty || dest) {
                            vis[x][y] = true;
                            q.offer(new int[] {x, y});
                        }
                    }
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSeconds(vector<vector<string>>& land) {
        int m = land.size(), n = land[0].size();
        bool vis[m][n];
        int g[m][n];
        memset(vis, false, sizeof(vis));
        memset(g, 0x3f, sizeof(g));
        queue<pair<int, int>> q;
        int si = 0, sj = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                auto c = land[i][j];
                if (c == "*") {
                    q.emplace(i, j);
                } else if (c == "S") {
                    si = i;
                    sj = j;
                }
            }
        }
        int dirs[5] = {-1, 0, 1, 0, -1};
        for (int t = 0; !q.empty(); ++t) {
            for (int k = q.size(); k; --k) {
                auto [i, j] = q.front();
                q.pop();
                g[i][j] = t;
                for (int d = 0; d < 4; ++d) {
                    int x = i + dirs[d], y = j + dirs[d + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y]) {
                        bool empty = land[x][y] == ".";
                        bool start = land[x][y] == "S";
                        if (empty || start) {
                            vis[x][y] = true;
                            q.emplace(x, y);
                        }
                    }
                }
            }
        }
        q.emplace(si, sj);
        memset(vis, false, sizeof(vis));
        vis[si][sj] = true;
        for (int t = 0; !q.empty(); ++t) {
            for (int k = q.size(); k; --k) {
                auto [i, j] = q.front();
                q.pop();
                if (land[i][j] == "D") {
                    return t;
                }
                for (int d = 0; d < 4; ++d) {
                    int x = i + dirs[d], y = j + dirs[d + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && g[x][y] > t + 1) {
                        bool empty = land[x][y] == ".";
                        bool dest = land[x][y] == "D";
                        if (empty || dest) {
                            vis[x][y] = true;
                            q.emplace(x, y);
                        }
                    }
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minimumSeconds(land [][]string) int {
	m, n := len(land), len(land[0])
	vis := make([][]bool, m)
	g := make([][]int, m)
	q := [][2]int{}
	var si, sj int
	for i, row := range land {
		vis[i] = make([]bool, n)
		g[i] = make([]int, n)
		for j := range g[i] {
			g[i][j] = 1 << 30
		}
		for j, c := range row {
			if c == "*" {
				q = append(q, [2]int{i, j})
			} else if c == "S" {
				si, sj = i, j
			}
		}
	}
	dirs := [5]int{-1, 0, 1, 0, -1}
	for t := 0; len(q) > 0; t++ {
		for k := len(q); k > 0; k-- {
			p := q[0]
			q = q[1:]
			i, j := p[0], p[1]
			g[i][j] = t
			for d := 0; d < 4; d++ {
				x, y := i+dirs[d], j+dirs[d+1]
				if x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] {
					empty := land[x][y] == "."
					start := land[x][y] == "S"
					if empty || start {
						vis[x][y] = true
						q = append(q, [2]int{x, y})
					}
				}
			}
		}
	}
	q = append(q, [2]int{si, sj})
	vis = make([][]bool, m)
	for i := range vis {
		vis[i] = make([]bool, n)
	}
	vis[si][sj] = true
	for t := 0; len(q) > 0; t++ {
		for k := len(q); k > 0; k-- {
			p := q[0]
			q = q[1:]
			i, j := p[0], p[1]
			if land[i][j] == "D" {
				return t
			}
			for d := 0; d < 4; d++ {
				x, y := i+dirs[d], j+dirs[d+1]
				if x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && g[x][y] > t+1 {
					empty := land[x][y] == "."
					dest := land[x][y] == "D"
					if empty || dest {
						vis[x][y] = true
						q = append(q, [2]int{x, y})
					}
				}
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minimumSeconds(land: string[][]): number {
    const m = land.length;
    const n = land[0].length;
    const g: number[][] = Array(m)
        .fill(0)
        .map(() => Array(n).fill(1 << 30));
    const vis: boolean[][] = Array(m)
        .fill(0)
        .map(() => Array(n).fill(false));
    const q: number[][] = [];
    let [si, sj] = [0, 0];
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            const c = land[i][j];
            if (c === '*') {
                q.push([i, j]);
            } else if (c === 'S') {
                [si, sj] = [i, j];
            }
        }
    }
    const dirs = [-1, 0, 1, 0, -1];
    for (let t = 0; q.length; ++t) {
        for (let k = q.length; k; --k) {
            const [i, j] = q.shift()!;
            g[i][j] = t;
            for (let d = 0; d < 4; ++d) {
                const [x, y] = [i + dirs[d], j + dirs[d + 1]];
                if (x >= 0 && x < m && y >= 0 && y < n && !vis[x][y] && 'S.'.includes(land[x][y])) {
                    vis[x][y] = true;
                    q.push([x, y]);
                }
            }
        }
    }
    q.push([si, sj]);
    for (let i = 0; i < m; ++i) {
        vis[i].fill(false);
    }
    vis[si][sj] = true;
    for (let t = 0; q.length; ++t) {
        for (let k = q.length; k; --k) {
            const [i, j] = q.shift()!;
            if (land[i][j] === 'D') {
                return t;
            }
            for (let d = 0; d < 4; ++d) {
                const [x, y] = [i + dirs[d], j + dirs[d + 1]];
                if (
                    x >= 0 &&
                    x < m &&
                    y >= 0 &&
                    y < n &&
                    !vis[x][y] &&
                    g[x][y] > t + 1 &&
                    'D.'.includes(land[x][y]) &&
                    t + 1 < g[x][y]
                ) {
                    vis[x][y] = true;
                    q.push([x, y]);
                }
            }
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
