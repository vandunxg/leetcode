---
comments: true
difficulty: Medium
rating: 1779
source: Weekly Contest 146 Q2
tags:
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [1129. Shortest Path with Alternating Colors](https://leetcode.com/problems/shortest-path-with-alternating-colors)

[中文文档](/solution/1100-1199/1129.Shortest%20Path%20with%20Alternating%20Colors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, là số node trong đồ thị có hướng với các node được đánh nhãn từ <code>0</code> đến <code>n - 1</code>. Mỗi cạnh trong đồ thị có màu đỏ hoặc xanh dương; đồ thị có thể có cạnh tự nối và cạnh song song.</p>

<p>Cho hai mảng <code>redEdges</code> và <code>blueEdges</code>, trong đó:</p>

<ul>
	<li><code>redEdges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị cạnh đỏ có hướng từ node <code>a<sub>i</sub></code> đến node <code>b<sub>i</sub></code> trong đồ thị, và</li>
	<li><code>blueEdges[j] = [u<sub>j</sub>, v<sub>j</sub>]</code> biểu thị cạnh xanh dương có hướng từ node <code>u<sub>j</sub></code> đến node <code>v<sub>j</sub></code> trong đồ thị.</li>
</ul>

<p>Hãy trả về mảng <code>answer</code> có độ dài <code>n</code>, trong đó mỗi <code>answer[x]</code> là độ dài đường đi ngắn nhất từ node <code>0</code> đến node <code>x</code> sao cho màu các cạnh trên đường đi luân phiên nhau; nếu không có đường đi như vậy thì <code>answer[x]</code> là <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, redEdges = [[0,1],[1,2]], blueEdges = []
<strong>Đầu ra:</strong> [0,1,-1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, redEdges = [[0,1]], blueEdges = [[2,1]]
<strong>Đầu ra:</strong> [0,1,-1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= redEdges.length,&nbsp;blueEdges.length &lt;= 400</code></li>
	<li><code>redEdges[i].length == blueEdges[j].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub>, u<sub>j</sub>, v<sub>j</sub> &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Các cạnh có màu đỏ hoặc xanh dương và hai cạnh liên tiếp phải khác màu, nên có thể cần thăm cùng một node hai lần với màu cạnh đi vào khác nhau. Trạng thái là $(\textit{node},\textit{last color})$. Chạy BFS từ $0$ với cả hai màu giả định ban đầu; lần đầu đến một node sẽ cho đường đi ngắn nhất.
>
> Danh sách kề được lưu riêng theo màu; sau mỗi bước, ta đổi màu cần đi để không bao giờ đi liên tiếp hai cạnh cùng màu.

<!-- thinking:end -->

Về bản chất, đây là bài toán tìm đường đi ngắn nhất, có thể giải bằng BFS.

Trước tiên, ta tiền xử lý các cạnh, phân loại theo màu và lưu vào mảng nhiều chiều $g$. Trong đó, $g[0]$ lưu các cạnh đỏ, còn $g[1]$ lưu các cạnh xanh dương.

Tiếp theo, ta định nghĩa các cấu trúc dữ liệu hoặc biến sau:

- Queue $q$: lưu node đang xét và màu của cạnh hiện tại;
- Set $vis$: lưu các node đã xét cùng với màu của cạnh hiện tại;
- Biến $d$: biểu thị level hiện tại của quá trình tìm kiếm, tức khoảng cách từ node đang xét đến điểm bắt đầu;
- Mảng $ans$: lưu khoảng cách ngắn nhất từ mỗi node đến điểm bắt đầu. Ban đầu, ta gán tất cả phần tử trong mảng $ans$ bằng $-1$, nghĩa là chưa biết khoảng cách từ các node đến điểm bắt đầu.

Đầu tiên, ta đưa điểm bắt đầu $0$ vào queue cùng với màu cạnh bắt đầu là $0$ hoặc $1$, tương ứng với việc cạnh hiện tại là đỏ hoặc xanh dương.

Tiếp theo, ta bắt đầu BFS. Mỗi lần lấy node $(i, c)$ khỏi queue, nếu chưa cập nhật đáp án cho node hiện tại thì đặt $ans[i] = d$, với $d$ là level hiện tại. Sau đó, đổi màu cạnh hiện tại $c$: đỏ thành xanh dương và ngược lại. Ta xét tất cả cạnh có màu tương ứng; nếu node đầu kia $j$ của cạnh chưa được thăm với màu này thì đưa nó vào queue.

Khi tìm kiếm kết thúc, trả về mảng đáp án.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestAlternatingPaths(
        self, n: int, redEdges: List[List[int]], blueEdges: List[List[int]]
    ) -> List[int]:
        g = [defaultdict(list), defaultdict(list)]
        for i, j in redEdges:
            g[0][i].append(j)
        for i, j in blueEdges:
            g[1][i].append(j)
        ans = [-1] * n
        vis = set()
        q = deque([(0, 0), (0, 1)])
        d = 0
        while q:
            for _ in range(len(q)):
                i, c = q.popleft()
                if ans[i] == -1:
                    ans[i] = d
                vis.add((i, c))
                c ^= 1
                for j in g[c][i]:
                    if (j, c) not in vis:
                        q.append((j, c))
            d += 1
        return ans
```

#### Java

```java
class Solution {
    public int[] shortestAlternatingPaths(int n, int[][] redEdges, int[][] blueEdges) {
        List<Integer>[][] g = new List[2][n];
        for (var f : g) {
            Arrays.setAll(f, k -> new ArrayList<>());
        }
        for (var e : redEdges) {
            g[0][e[0]].add(e[1]);
        }
        for (var e : blueEdges) {
            g[1][e[0]].add(e[1]);
        }
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {0, 0});
        q.offer(new int[] {0, 1});
        boolean[][] vis = new boolean[n][2];
        int[] ans = new int[n];
        Arrays.fill(ans, -1);
        int d = 0;
        while (!q.isEmpty()) {
            for (int k = q.size(); k > 0; --k) {
                var p = q.poll();
                int i = p[0], c = p[1];
                if (ans[i] == -1) {
                    ans[i] = d;
                }
                vis[i][c] = true;
                c ^= 1;
                for (int j : g[c][i]) {
                    if (!vis[j][c]) {
                        q.offer(new int[] {j, c});
                    }
                }
            }
            ++d;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> shortestAlternatingPaths(int n, vector<vector<int>>& redEdges, vector<vector<int>>& blueEdges) {
        vector<vector<vector<int>>> g(2, vector<vector<int>>(n));
        for (auto& e : redEdges) {
            g[0][e[0]].push_back(e[1]);
        }
        for (auto& e : blueEdges) {
            g[1][e[0]].push_back(e[1]);
        }
        queue<pair<int, int>> q;
        q.emplace(0, 0);
        q.emplace(0, 1);
        bool vis[n][2];
        memset(vis, false, sizeof vis);
        vector<int> ans(n, -1);
        int d = 0;
        while (!q.empty()) {
            for (int k = q.size(); k; --k) {
                auto [i, c] = q.front();
                q.pop();
                if (ans[i] == -1) {
                    ans[i] = d;
                }
                vis[i][c] = true;
                c ^= 1;
                for (int& j : g[c][i]) {
                    if (!vis[j][c]) {
                        q.emplace(j, c);
                    }
                }
            }
            ++d;
        }
        return ans;
    }
};
```

#### Go

```go
func shortestAlternatingPaths(n int, redEdges [][]int, blueEdges [][]int) []int {
	g := [2][][]int{}
	for i := range g {
		g[i] = make([][]int, n)
	}
	for _, e := range redEdges {
		g[0][e[0]] = append(g[0][e[0]], e[1])
	}
	for _, e := range blueEdges {
		g[1][e[0]] = append(g[1][e[0]], e[1])
	}
	type pair struct{ i, c int }
	q := []pair{pair{0, 0}, pair{0, 1}}
	ans := make([]int, n)
	vis := make([][2]bool, n)
	for i := range ans {
		ans[i] = -1
	}
	d := 0
	for len(q) > 0 {
		for k := len(q); k > 0; k-- {
			p := q[0]
			q = q[1:]
			i, c := p.i, p.c
			if ans[i] == -1 {
				ans[i] = d
			}
			vis[i][c] = true
			c ^= 1
			for _, j := range g[c][i] {
				if !vis[j][c] {
					q = append(q, pair{j, c})
				}
			}
		}
		d++
	}
	return ans
}
```

#### TypeScript

```ts
function shortestAlternatingPaths(
    n: number,
    redEdges: number[][],
    blueEdges: number[][],
): number[] {
    const g: [Graph, Graph] = [{}, {}];
    const ans = Array(n).fill(-1);
    const vis = Array.from({ length: n }, () => Array.from({ length: 2 }, () => false));
    let q: Vertex[] = [
        [0, 0],
        [0, 1],
    ];
    vis[0][0] = vis[0][1] = true;
    let d = 0;
    for (const [i, j] of redEdges) {
        (g[0][i] ??= []).push(j);
    }
    for (const [i, j] of blueEdges) {
        (g[1][i] ??= []).push(j);
    }
    while (q.length) {
        const qNext: Vertex[] = [];
        for (let [i, color] of q) {
            if (ans[i] === -1) {
                ans[i] = d;
            }
            color ^= 1;
            for (const j of g[color][i] ?? []) {
                if (!vis[j][color]) {
                    vis[j][color] = true;
                    qNext.push([j, color as Color]);
                }
            }
        }
        q = qNext;
        d++;
    }
    return ans;
}

type Graph = Record<number, number[]>;
type Color = 0 | 1;
type Vertex = [number, Color];
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
