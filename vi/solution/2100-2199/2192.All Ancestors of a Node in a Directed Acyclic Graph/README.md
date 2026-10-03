---
comments: true
difficulty: Medium
rating: 1787
source: Biweekly Contest 73 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [2192. All Ancestors of a Node in a Directed Acyclic Graph](https://leetcode.com/problems/all-ancestors-of-a-node-in-a-directed-acyclic-graph)

[Tài liệu tiếng Trung](/solution/2100-2199/2192.All%20Ancestors%20of%20a%20Node%20in%20a%20Directed%20Acyclic%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code> biểu diễn số node của một <strong>Đồ thị có hướng không chu trình</strong> (DAG). Các node được đánh số từ <code>0</code> đến <code>n - 1</code> (<strong>bao gồm cả hai đầu</strong>).</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>edges</code>, trong đó <code>edges[i] = [from<sub>i</sub>, to<sub>i</sub>]</code> biểu thị rằng có một cạnh <strong>một chiều</strong> từ <code>from<sub>i</sub></code> đến <code>to<sub>i</sub></code> trong đồ thị.</p>

<p>Trả về <em>một danh sách</em> <code>answer</code><em>, trong đó </em><code>answer[i]</code><em> là <strong>danh sách ancestor</strong> của node thứ </em><code>i<sup>th</sup></code><em>, được sắp xếp theo <strong>thứ tự tăng dần</strong></em>.</p>

<p>Một node <code>u</code> là <strong>ancestor</strong> của node <code>v</code> khác nếu <code>u</code> có thể đi đến <code>v</code> thông qua một chuỗi các cạnh.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2192.All%20Ancestors%20of%20a%20Node%20in%20a%20Directed%20Acyclic%20Graph/images/e1.png" style="width: 322px; height: 265px;" />
<pre>
<strong>Đầu vào:</strong> n = 8, edgeList = [[0,3],[0,4],[1,3],[2,4],[2,7],[3,5],[3,6],[3,7],[4,6]]
<strong>Đầu ra:</strong> [[],[],[],[0,1],[0,2],[0,1,3],[0,1,2,3,4],[0,1,2,3]]
<strong>Giải thích:</strong>
Sơ đồ trên biểu diễn đồ thị đầu vào.
- Các node 0, 1 và 2 không có ancestor nào.
- Node 3 có hai ancestor là 0 và 1.
- Node 4 có hai ancestor là 0 và 2.
- Node 5 có ba ancestor là 0, 1 và 3.
- Node 6 có năm ancestor là 0, 1, 2, 3 và 4.
- Node 7 có bốn ancestor là 0, 1, 2 và 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2192.All%20Ancestors%20of%20a%20Node%20in%20a%20Directed%20Acyclic%20Graph/images/e2.png" style="width: 343px; height: 299px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, edgeList = [[0,1],[0,2],[0,3],[0,4],[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
<strong>Đầu ra:</strong> [[],[0],[0,1],[0,1,2],[0,1,2,3]]
<strong>Giải thích:</strong>
Sơ đồ trên biểu diễn đồ thị đầu vào.
- Node 0 không có ancestor nào.
- Node 1 có một ancestor là 0.
- Node 2 có hai ancestor là 0 và 1.
- Node 3 có ba ancestor là 0, 1 và 2.
- Node 4 có bốn ancestor là 0, 1, 2 và 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= edges.length &lt;= min(2000, n * (n - 1) / 2)</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= from<sub>i</sub>, to<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>from<sub>i</sub> != to<sub>i</sub></code></li>
	<li>Không có cạnh trùng lặp.</li>
	<li>Đồ thị là <strong>có hướng</strong> và <strong>không chu trình</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần liệt kê mọi ancestor của mỗi node trong một DAG theo thứ tự tăng dần của id. DFS ngược từ mỗi node sẽ lặp lại các cạnh đi vào dùng chung và vẫn cần sắp xếp. Với $n\le 1000$, ta có thể duyệt các node hậu duệ của mỗi $i$ rồi thêm $i$ vào các danh sách tương ứng, vốn đã được sắp xếp theo $i$.
>
> Ta xây dựng danh sách kề, chạy BFS từ mỗi $i$ và ghi nhận $i$ vào mọi node có thể đi tới. Một tập visited giúp tránh đưa cùng một node vào hàng đợi nhiều lần.
>
> Mỗi dòng của answer tăng dần vì các node nguồn được duyệt theo thứ tự.

<!-- thinking:end -->

Trước tiên, ta xây dựng danh sách kề $g$ dựa trên mảng hai chiều $edges$, trong đó $g[i]$ biểu diễn tất cả node kế tiếp của node $i$.

Sau đó, ta duyệt node $i$ theo thứ tự tăng dần, coi nó là node ancestor, dùng BFS để tìm tất cả node kế tiếp của $i$ và thêm node $i$ vào danh sách ancestor của các node đó.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getAncestors(self, n: int, edges: List[List[int]]) -> List[List[int]]:
        def bfs(s: int):
            q = deque([s])
            vis = {s}
            while q:
                i = q.popleft()
                for j in g[i]:
                    if j not in vis:
                        vis.add(j)
                        q.append(j)
                        ans[j].append(s)

        g = defaultdict(list)
        for u, v in edges:
            g[u].append(v)
        ans = [[] for _ in range(n)]
        for i in range(n):
            bfs(i)
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private List<Integer>[] g;
    private List<List<Integer>> ans;

    public List<List<Integer>> getAncestors(int n, int[][] edges) {
        g = new List[n];
        this.n = n;
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : edges) {
            g[e[0]].add(e[1]);
        }
        ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            ans.add(new ArrayList<>());
        }
        for (int i = 0; i < n; ++i) {
            bfs(i);
        }
        return ans;
    }

    private void bfs(int s) {
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(s);
        boolean[] vis = new boolean[n];
        vis[s] = true;
        while (!q.isEmpty()) {
            int i = q.poll();
            for (int j : g[i]) {
                if (!vis[j]) {
                    vis[j] = true;
                    q.offer(j);
                    ans.get(j).add(s);
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> getAncestors(int n, vector<vector<int>>& edges) {
        vector<int> g[n];
        for (auto& e : edges) {
            g[e[0]].push_back(e[1]);
        }
        vector<vector<int>> ans(n);
        auto bfs = [&](int s) {
            queue<int> q;
            q.push(s);
            bool vis[n];
            memset(vis, 0, sizeof(vis));
            vis[s] = true;
            while (q.size()) {
                int i = q.front();
                q.pop();
                for (int j : g[i]) {
                    if (!vis[j]) {
                        vis[j] = true;
                        ans[j].push_back(s);
                        q.push(j);
                    }
                }
            }
        };
        for (int i = 0; i < n; ++i) {
            bfs(i);
        }
        return ans;
    }
};
```

#### Go

```go
func getAncestors(n int, edges [][]int) [][]int {
	g := make([][]int, n)
	for _, e := range edges {
		g[e[0]] = append(g[e[0]], e[1])
	}
	ans := make([][]int, n)
	bfs := func(s int) {
		q := []int{s}
		vis := make([]bool, n)
		vis[s] = true
		for len(q) > 0 {
			i := q[0]
			q = q[1:]
			for _, j := range g[i] {
				if !vis[j] {
					vis[j] = true
					q = append(q, j)
					ans[j] = append(ans[j], s)
				}
			}
		}
	}
	for i := 0; i < n; i++ {
		bfs(i)
	}
	return ans
}
```

#### TypeScript

```ts
function getAncestors(n: number, edges: number[][]): number[][] {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u].push(v);
    }
    const ans: number[][] = Array.from({ length: n }, () => []);
    const bfs = (s: number) => {
        const q: number[] = [s];
        const vis: boolean[] = Array.from({ length: n }, () => false);
        vis[s] = true;
        while (q.length) {
            const i = q.pop()!;
            for (const j of g[i]) {
                if (!vis[j]) {
                    vis[j] = true;
                    ans[j].push(s);
                    q.push(j);
                }
            }
        }
    };
    for (let i = 0; i < n; ++i) {
        bfs(i);
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    private int n;
    private List<int>[] g;
    private IList<IList<int>> ans;

    public IList<IList<int>> GetAncestors(int n, int[][] edges) {
        g = new List<int>[n];
        this.n = n;
        for (int i = 0; i < n; i++) {
            g[i] = new List<int>();
        }
        foreach (var e in edges) {
            g[e[0]].Add(e[1]);
        }
        ans = new List<IList<int>>();
        for (int i = 0; i < n; ++i) {
            ans.Add(new List<int>());
        }
        for (int i = 0; i < n; ++i) {
            BFS(i);
        }
        return ans;
    }

    private void BFS(int s) {
        Queue<int> q = new Queue<int>();
        q.Enqueue(s);
        bool[] vis = new bool[n];
        vis[s] = true;
        while (q.Count > 0) {
            int i = q.Dequeue();
            foreach (int j in g[i]) {
                if (!vis[j]) {
                    vis[j] = true;
                    q.Enqueue(j);
                    ans[j].Add(s);
                }
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
