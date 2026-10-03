---
comments: true
difficulty: Medium
rating: 1638
source: Biweekly Contest 56 Q2
tags:
    - Breadth-First Search
    - Array
    - Matrix
---

<!-- problem:start -->

# [1926. Nearest Exit from Entrance in Maze](https://leetcode.com/problems/nearest-exit-from-entrance-in-maze)

[中文文档](/solution/1900-1999/1926.Nearest%20Exit%20from%20Entrance%20in%20Maze/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận <code>m x n</code> <code>maze</code> (<strong>đánh chỉ số từ 0</strong>) gồm các ô trống (ký hiệu là <code>&#39;.&#39;</code>) và các bức tường (ký hiệu là <code>&#39;+&#39;</code>). Ngoài ra, bạn được cho biết vị trí <code>entrance</code> của mê cung, trong đó <code>entrance = [entrance<sub>row</sub>, entrance<sub>col</sub>]</code> biểu thị hàng và cột của ô mà bạn đang đứng lúc đầu.</p>

<p>Trong một bước, bạn có thể di chuyển một ô theo hướng <strong>lên</strong>, <strong>xuống</strong>, <strong>sang trái</strong> hoặc <strong>sang phải</strong>. Bạn không thể đi vào ô có tường và không thể đi ra ngoài mê cung. Mục tiêu là tìm <strong>lối ra gần nhất</strong> từ <code>entrance</code>. <strong>Lối ra</strong> được định nghĩa là một <strong>ô trống</strong> nằm ở <strong>biên</strong> của <code>maze</code>. Ô <code>entrance</code> <strong>không được tính</strong> là lối ra.</p>

<p>Trả về <em><strong>số bước</strong> trên đường đi ngắn nhất từ </em><code>entrance</code><em> đến lối ra gần nhất, hoặc </em><code>-1</code><em> nếu không tồn tại đường đi như vậy</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1926.Nearest%20Exit%20from%20Entrance%20in%20Maze/images/nearest1-grid.jpg" style="width: 333px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> maze = [[&quot;+&quot;,&quot;+&quot;,&quot;.&quot;,&quot;+&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;,&quot;+&quot;],[&quot;+&quot;,&quot;+&quot;,&quot;+&quot;,&quot;.&quot;]], entrance = [1,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 3 lối ra trong mê cung này tại [1,0], [0,2] và [2,3].
Ban đầu, bạn đang ở ô entrance [1,2].
- Bạn có thể đến [1,0] bằng cách di chuyển sang trái 2 bước.
- Bạn có thể đến [0,2] bằng cách di chuyển lên 1 bước.
Không thể đến [2,3] từ entrance.
Vì vậy, lối ra gần nhất là [0,2], cách 1 bước.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1926.Nearest%20Exit%20from%20Entrance%20in%20Maze/images/nearesr2-grid.jpg" style="width: 253px; height: 253px;" />
<pre>
<strong>Đầu vào:</strong> maze = [[&quot;+&quot;,&quot;+&quot;,&quot;+&quot;],[&quot;.&quot;,&quot;.&quot;,&quot;.&quot;],[&quot;+&quot;,&quot;+&quot;,&quot;+&quot;]], entrance = [1,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 1 lối ra trong mê cung này tại [1,2].
[1,0] không được tính là lối ra vì đó là ô entrance.
Ban đầu, bạn đang ở ô entrance [1,0].
- Bạn có thể đến [1,2] bằng cách di chuyển sang phải 2 bước.
Vì vậy, lối ra gần nhất là [1,2], cách 2 bước.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1926.Nearest%20Exit%20from%20Entrance%20in%20Maze/images/nearest3-grid.jpg" style="width: 173px; height: 93px;" />
<pre>
<strong>Đầu vào:</strong> maze = [[&quot;.&quot;,&quot;+&quot;]], entrance = [0,0]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có lối ra nào trong mê cung.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>maze.length == m</code></li>
	<li><code>maze[i].length == n</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>maze[i][j]</code> là <code>&#39;.&#39;</code> hoặc <code>&#39;+&#39;</code>.</li>
	<li><code>entrance.length == 2</code></li>
	<li><code>0 &lt;= entrance<sub>row</sub> &lt; m</code></li>
	<li><code>0 &lt;= entrance<sub>col</sub> &lt; n</code></li>
	<li><code>entrance</code> luôn là một ô trống.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Lối ra gần nhất chính là đường đi ngắn nhất trên đồ thị không trọng số. DFS không đảm bảo tìm được đường đi ngắn nhất. Ô entrance không phải là lối ra, ngay cả khi nó nằm trên biên.
>
> Thực hiện BFS từ entrance, đánh dấu các ô trống đã thăm bằng cách biến chúng thành tường. Lần đầu tiên đến một ô trống trên biên thì khoảng cách hiện tại là đáp án; nếu queue rỗng thì không thể đến được lối ra.
>
> Mở rộng các ô kề theo bốn hướng qua từng tầng giúp số bước luôn bằng khoảng cách.

<!-- thinking:end -->

Bắt đầu từ entrance và thực hiện tìm kiếm theo chiều rộng (BFS). Mỗi khi đến một ô trống mới, ta đánh dấu ô đó đã được thăm và thêm vào queue cho đến khi tìm thấy một ô trống trên biên, khi đó trả về số bước.

Cụ thể, ta định nghĩa một queue $q$, ban đầu thêm $\textit{entrance}$ vào queue. Ta định nghĩa biến $\textit{ans}$ để ghi nhận số bước, ban đầu đặt bằng $1$. Sau đó bắt đầu BFS. Ở mỗi lượt, ta lấy tất cả phần tử trong queue ra và duyệt qua chúng. Với mỗi phần tử, ta thử di chuyển theo bốn hướng. Nếu vị trí mới là một ô trống, ta thêm nó vào queue và đánh dấu đã thăm. Nếu vị trí mới là một ô trống trên biên, ta trả về $\textit{ans}$. Nếu queue rỗng, ta trả về $-1$. Sau lượt tìm kiếm này, ta tăng $\textit{ans}$ lên một và tiếp tục lượt tìm kiếm tiếp theo.

Nếu duyệt hết mà không tìm thấy ô trống nào trên biên, ta trả về $-1$.

Độ phức tạp thời gian là $O(m \times n)$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của mê cung.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nearestExit(self, maze: List[List[str]], entrance: List[int]) -> int:
        m, n = len(maze), len(maze[0])
        i, j = entrance
        q = deque([(i, j)])
        maze[i][j] = "+"
        ans = 0
        while q:
            ans += 1
            for _ in range(len(q)):
                i, j = q.popleft()
                for a, b in [[0, -1], [0, 1], [-1, 0], [1, 0]]:
                    x, y = i + a, j + b
                    if 0 <= x < m and 0 <= y < n and maze[x][y] == ".":
                        if x == 0 or x == m - 1 or y == 0 or y == n - 1:
                            return ans
                        q.append((x, y))
                        maze[x][y] = "+"
        return -1
```

#### Java

```java
class Solution {
    public int nearestExit(char[][] maze, int[] entrance) {
        int m = maze.length, n = maze[0].length;
        final int[] dirs = {-1, 0, 1, 0, -1};
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(entrance);
        maze[entrance[0]][entrance[1]] = '+';
        for (int ans = 1; !q.isEmpty(); ++ans) {
            for (int k = q.size(); k > 0; --k) {
                var p = q.poll();
                for (int d = 0; d < 4; ++d) {
                    int x = p[0] + dirs[d], y = p[1] + dirs[d + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && maze[x][y] == '.') {
                        if (x == 0 || x == m - 1 || y == 0 || y == n - 1) {
                            return ans;
                        }
                        maze[x][y] = '+';
                        q.offer(new int[] {x, y});
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
    int nearestExit(vector<vector<char>>& maze, vector<int>& entrance) {
        int m = maze.size(), n = maze[0].size();
        int dirs[5] = {-1, 0, 1, 0, -1};
        queue<pair<int, int>> q;
        q.emplace(entrance[0], entrance[1]);
        maze[entrance[0]][entrance[1]] = '+';
        for (int ans = 1; !q.empty(); ++ans) {
            for (int k = q.size(); k; --k) {
                auto [i, j] = q.front();
                q.pop();
                for (int d = 0; d < 4; ++d) {
                    int x = i + dirs[d], y = j + dirs[d + 1];
                    if (x >= 0 && x < m && y >= 0 && y < n && maze[x][y] == '.') {
                        if (x == 0 || x == m - 1 || y == 0 || y == n - 1) {
                            return ans;
                        }
                        maze[x][y] = '+';
                        q.emplace(x, y);
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
func nearestExit(maze [][]byte, entrance []int) int {
	m, n := len(maze), len(maze[0])
	q := [][2]int{{entrance[0], entrance[1]}}
	maze[entrance[0]][entrance[1]] = '+'
	dirs := []int{-1, 0, 1, 0, -1}
	for ans := 1; len(q) > 0; ans++ {
		for k := len(q); k > 0; k-- {
			p := q[0]
			q = q[1:]
			for l := 0; l < 4; l++ {
				x, y := p[0]+dirs[l], p[1]+dirs[l+1]
				if x >= 0 && x < m && y >= 0 && y < n && maze[x][y] == '.' {
					if x == 0 || x == m-1 || y == 0 || y == n-1 {
						return ans
					}
					q = append(q, [2]int{x, y})
					maze[x][y] = '+'
				}
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function nearestExit(maze: string[][], entrance: number[]): number {
    const dir = [0, 1, 0, -1, 0];
    const q = [[...entrance, 0]];
    maze[entrance[0]][entrance[1]] = '+';
    for (const [i, j, ans] of q) {
        for (let d = 0; d < 4; d++) {
            const [x, y] = [i + dir[d], j + dir[d + 1]];
            const v = maze[x]?.[y];
            if (!v && ans) {
                return ans;
            }
            if (v === '.') {
                q.push([x, y, ans + 1]);
                maze[x][y] = '+';
            }
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
