---
comments: true
difficulty: Medium
tags:
    - Tree
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [3787. Find Diameter Endpoints of a Tree 🔒](https://leetcode.com/problems/find-diameter-endpoints-of-a-tree)

[中文文档](/solution/3700-3799/3787.Find%20Diameter%20Endpoints%20of%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <strong>cây vô hướng</strong> gồm <code>n</code> node, được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có một cạnh nối node <code>a<sub>i</sub></code> với node <code>b<sub>i</sub></code> trong cây.</p>

<p>Một node được gọi là <strong>đặc biệt</strong> nếu nó là một <strong>endpoint</strong> của bất kỳ <strong>đường kính</strong> nào của cây.</p>

<p>Trả về một chuỗi nhị phân <code>s</code> có độ dài <code>n</code>, trong đó <code>s[i] = &#39;1&#39;</code> nếu node <code>i</code> là node đặc biệt, và <code>s[i] = &#39;0&#39;</code> nếu ngược lại.</p>

<p><strong>Đường kính</strong> của một cây là <strong>đường đi đơn dài nhất</strong> giữa hai node bất kỳ. Một cây có thể có nhiều đường kính.</p>

<p><strong>Endpoint</strong> của một đường đi là node <strong>đầu tiên</strong> hoặc <strong>cuối cùng</strong> trên đường đi đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3700-3799/3787.Find%20Diameter%20Endpoints%20of%20a%20Tree/images/pic1.png" style="width: 291px; height: 51px;" /></strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;101&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đường kính của cây này gồm 2 cạnh.</li>
	<li>Đường kính duy nhất là đường đi từ node 0 đến node 2.</li>
	<li>Endpoint của đường đi này là node 0 và node 2, nên chúng là các node đặc biệt.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3700-3799/3787.Find%20Diameter%20Endpoints%20of%20a%20Tree/images/pic2.png" /></strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7, edges = [[0,1],[1,2],[2,3],[3,4],[3,5],[1,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;1000111&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường kính của cây này gồm 4 cạnh. Có 4 đường kính:</p>

<ul>
	<li>Đường đi từ node 0 đến node 4.</li>
	<li>Đường đi từ node 0 đến node 5.</li>
	<li>Đường đi từ node 6 đến node 4.</li>
	<li>Đường đi từ node 6 đến node 5.</li>
</ul>

<p>Các node đặc biệt là node <code>0, 4, 5, 6</code>, vì chúng là endpoint của ít nhất một đường kính.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3700-3799/3787.Find%20Diameter%20Endpoints%20of%20a%20Tree/images/pic3.png" />​​​​​​​</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;11&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đường kính của cây này gồm 1 cạnh.</li>
	<li>Đường kính duy nhất là đường đi từ node 0 đến node 1.</li>
	<li>Endpoint của đường đi này là node 0 và node 1, nên chúng là các node đặc biệt.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Endpoint của đường kính cây có thể được tìm bằng hai lượt BFS: tìm node xa nhất $a$ từ một node bắt đầu bất kỳ, sau đó tìm node xa nhất $b$ từ $a$. Node $u$ là endpoint của một đường kính nào đó khi và chỉ khi khoảng cách từ node này đến $a$ hoặc đến $b$ bằng độ dài đường kính.

<!-- thinking:end -->

Trước tiên, ta chuyển đổi mảng $\text{edges}$ thành dạng danh sách kề của một đồ thị vô hướng, trong đó $g[u]$ biểu diễn tất cả các node kề với node $u$.

Tiếp theo, ta có thể sử dụng Breadth-First Search (BFS) để tìm các endpoint của đường kính cây. Các bước cụ thể như sau:

1. Bắt đầu từ một node bất kỳ (chẳng hạn node $0$), sử dụng BFS để tìm node xa nhất $a$ từ node đó.
2. Bắt đầu từ node $a$, sử dụng BFS lần nữa để tìm node xa nhất $b$ từ node $a$, đồng thời lấy mảng khoảng cách $\text{dist1}$ từ node $a$ đến tất cả các node khác.
3. Bắt đầu từ node $b$, sử dụng BFS để lấy mảng khoảng cách $\text{dist2}$ từ node $b$ đến tất cả các node khác.
4. Độ dài đường kính của cây là $\text{dist1}[b]$. Với mỗi node $i$, nếu $\text{dist1}[i]$ hoặc $\text{dist2}[i]$ bằng độ dài đường kính thì node $i$ là node đặc biệt.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSpecialNodes(self, n: int, edges: List[List[int]]) -> str:
        g = [[] for _ in range(n)]
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)

        def bfs(start: int):
            dist = [-1] * n
            dist[start] = 0
            q = deque([start])
            far = start
            while q:
                u = q.popleft()
                if dist[u] > dist[far]:
                    far = u
                for v in g[u]:
                    if dist[v] == -1:
                        dist[v] = dist[u] + 1
                        q.append(v)
            return far, dist

        a, _ = bfs(0)
        b, dist1 = bfs(a)
        _, dist2 = bfs(b)
        d = dist1[b]
        ans = ["0"] * n
        for i in range(n):
            if dist1[i] == d or dist2[i] == d:
                ans[i] = "1"
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String findSpecialNodes(int n, int[][] edges) {
        List<Integer>[] g = new ArrayList[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }

        record BFSResult(int far, int[] dist) {
        }

        IntFunction<BFSResult> bfs = (int start) -> {
            int[] dist = new int[n];
            Arrays.fill(dist, -1);
            dist[start] = 0;
            ArrayDeque<Integer> q = new ArrayDeque<>();
            q.add(start);
            int far = start;
            while (!q.isEmpty()) {
                int u = q.poll();
                if (dist[u] > dist[far]) {
                    far = u;
                }
                for (int v : g[u]) {
                    if (dist[v] == -1) {
                        dist[v] = dist[u] + 1;
                        q.add(v);
                    }
                }
            }
            return new BFSResult(far, dist);
        };

        int a = bfs.apply(0).far();
        BFSResult r1 = bfs.apply(a);
        int b = r1.far();
        int[] dist1 = r1.dist();
        int[] dist2 = bfs.apply(b).dist();
        int d = dist1[b];

        char[] ans = new char[n];
        Arrays.fill(ans, '0');
        for (int i = 0; i < n; i++) {
            if (dist1[i] == d || dist2[i] == d) {
                ans[i] = '1';
            }
        }
        return new String(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findSpecialNodes(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }

        auto bfs = [&](int start) -> pair<int, vector<int>> {
            vector<int> dist(n, -1);
            dist[start] = 0;
            deque<int> q;
            q.push_back(start);
            int far = start;
            while (!q.empty()) {
                int u = q.front();
                q.pop_front();
                if (dist[u] > dist[far]) {
                    far = u;
                }
                for (int v : g[u]) {
                    if (dist[v] == -1) {
                        dist[v] = dist[u] + 1;
                        q.push_back(v);
                    }
                }
            }
            return {far, dist};
        };

        auto [a, _0] = bfs(0);
        auto [b, dist1] = bfs(a);
        auto [_1, dist2] = bfs(b);
        int d = dist1[b];

        string ans(n, '0');
        for (int i = 0; i < n; i++) {
            if (dist1[i] == d || dist2[i] == d) {
                ans[i] = '1';
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findSpecialNodes(n int, edges [][]int) string {
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}

	bfs := func(start int) (int, []int) {
		dist := make([]int, n)
		for i := range dist {
			dist[i] = -1
		}
		dist[start] = 0
		q := make([]int, 0, n)
		q = append(q, start)
		far := start
		for head := 0; head < len(q); head++ {
			u := q[head]
			if dist[u] > dist[far] {
				far = u
			}
			for _, v := range g[u] {
				if dist[v] == -1 {
					dist[v] = dist[u] + 1
					q = append(q, v)
				}
			}
		}
		return far, dist
	}

	a, _ := bfs(0)
	b, dist1 := bfs(a)
	_, dist2 := bfs(b)
	d := dist1[b]

	ans := make([]byte, n)
	for i := range ans {
		ans[i] = '0'
	}
	for i := 0; i < n; i++ {
		if dist1[i] == d || dist2[i] == d {
			ans[i] = '1'
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function findSpecialNodes(n: number, edges: number[][]): string {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }

    const bfs = (start: number): [number, number[]] => {
        const dist = new Array<number>(n).fill(-1);
        dist[start] = 0;
        const q: number[] = [start];
        let far = start;

        for (const u of q) {
            if (dist[u] > dist[far]) {
                far = u;
            }
            for (const v of g[u]) {
                if (dist[v] === -1) {
                    dist[v] = dist[u] + 1;
                    q.push(v);
                }
            }
        }
        return [far, dist];
    };

    const [a] = bfs(0);
    const [b, dist1] = bfs(a);
    const [, dist2] = bfs(b);
    const d = dist1[b];

    const ans: string[] = new Array(n).fill('0');
    for (let i = 0; i < n; i++) {
        if (dist1[i] === d || dist2[i] === d) {
            ans[i] = '1';
        }
    }
    return ans.join('');
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn find_special_nodes(n: i32, edges: Vec<Vec<i32>>) -> String {
        let n = n as usize;
        let mut g: Vec<Vec<usize>> = vec![vec![]; n];
        for e in edges {
            let a = e[0] as usize;
            let b = e[1] as usize;
            g[a].push(b);
            g[b].push(a);
        }

        fn bfs(start: usize, g: &Vec<Vec<usize>>) -> (usize, Vec<i32>) {
            let n = g.len();
            let mut dist = vec![-1i32; n];
            let mut q: VecDeque<usize> = VecDeque::new();
            dist[start] = 0;
            q.push_back(start);

            let mut far = start;
            while let Some(u) = q.pop_front() {
                if dist[u] > dist[far] {
                    far = u;
                }
                for &v in &g[u] {
                    if dist[v] == -1 {
                        dist[v] = dist[u] + 1;
                        q.push_back(v);
                    }
                }
            }
            (far, dist)
        }

        let (a, _) = bfs(0, &g);
        let (b, dist1) = bfs(a, &g);
        let (_, dist2) = bfs(b, &g);
        let d = dist1[b];

        let mut ans = vec![b'0'; n];
        for i in 0..n {
            if dist1[i] == d || dist2[i] == d {
                ans[i] = b'1';
            }
        }
        String::from_utf8(ans).unwrap()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
