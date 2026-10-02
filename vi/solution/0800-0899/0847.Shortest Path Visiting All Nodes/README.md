---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Breadth-First Search
    - Graph
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [847. Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes)

[中文文档](/solution/0800-0899/0847.Shortest%20Path%20Visiting%20All%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho đồ thị vô hướng, liên thông gồm <code>n</code> node được đánh nhãn từ <code>0</code> đến <code>n - 1</code>. Mảng <code>graph</code> được cho sao cho <code>graph[i]</code> liệt kê tất cả node có cạnh nối với node <code>i</code>.</p>

<p>Trả về <em>độ dài của đường đi ngắn nhất đi qua mọi node</em>. Bạn có thể bắt đầu và kết thúc ở bất kỳ node nào, ghé lại một node nhiều lần và đi qua lại các cạnh.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0847.Shortest%20Path%20Visiting%20All%20Nodes/images/shortest1-graph.jpg" style="width: 222px; height: 183px;" />
<pre>
<strong>Đầu vào:</strong> graph = [[1,2,3],[0],[0],[0]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một đường đi có thể là [1,0,2,0,3]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0847.Shortest%20Path%20Visiting%20All%20Nodes/images/shortest2-graph.jpg" style="width: 382px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> graph = [[1],[0,2,4],[1,3,4],[2],[1,2]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một đường đi có thể là [0,1,4,2,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == graph.length</code></li>
	<li><code>1 &lt;= n &lt;= 12</code></li>
	<li><code>0 &lt;= graph[i].length &lt;&nbsp;n</code></li>
	<li><code>graph[i]</code> không chứa <code>i</code>.</li>
	<li>Nếu <code>graph[a]</code> chứa <code>b</code> thì <code>graph[b]</code> cũng chứa <code>a</code>.</li>
	<li>Đồ thị đầu vào luôn liên thông.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm đường đi ngắn nhất qua mọi node và có thể dùng lại cạnh. Vì $n\le 12$, không cần lưu toàn bộ đường đi làm trạng thái; chỉ cần cặp (node hiện tại, mask các node đã thăm), tổng cộng có $n\cdot 2^n$ trạng thái.
>
> Chạy multi-source BFS trên không gian trạng thái với mọi node làm điểm bắt đầu. Lần đầu mask chứa đủ node thì chỉ số layer chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestPathLength(self, graph: List[List[int]]) -> int:
        n = len(graph)
        q = deque()
        vis = set()
        for i in range(n):
            q.append((i, 1 << i))
            vis.add((i, 1 << i))
        ans = 0
        while 1:
            for _ in range(len(q)):
                i, st = q.popleft()
                if st == (1 << n) - 1:
                    return ans
                for j in graph[i]:
                    nst = st | 1 << j
                    if (j, nst) not in vis:
                        vis.add((j, nst))
                        q.append((j, nst))
            ans += 1
```

#### Java

```java
class Solution {
    public int shortestPathLength(int[][] graph) {
        int n = graph.length;
        Deque<int[]> q = new ArrayDeque<>();
        boolean[][] vis = new boolean[n][1 << n];
        for (int i = 0; i < n; ++i) {
            q.offer(new int[] {i, 1 << i});
            vis[i][1 << i] = true;
        }
        for (int ans = 0;; ++ans) {
            for (int k = q.size(); k > 0; --k) {
                var p = q.poll();
                int i = p[0], st = p[1];
                if (st == (1 << n) - 1) {
                    return ans;
                }
                for (int j : graph[i]) {
                    int nst = st | 1 << j;
                    if (!vis[j][nst]) {
                        vis[j][nst] = true;
                        q.offer(new int[] {j, nst});
                    }
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
    int shortestPathLength(vector<vector<int>>& graph) {
        int n = graph.size();
        queue<pair<int, int>> q;
        bool vis[n][1 << n];
        memset(vis, false, sizeof(vis));
        for (int i = 0; i < n; ++i) {
            q.emplace(i, 1 << i);
            vis[i][1 << i] = true;
        }
        for (int ans = 0;; ++ans) {
            for (int k = q.size(); k; --k) {
                auto [i, st] = q.front();
                q.pop();
                if (st == (1 << n) - 1) {
                    return ans;
                }
                for (int j : graph[i]) {
                    int nst = st | 1 << j;
                    if (!vis[j][nst]) {
                        vis[j][nst] = true;
                        q.emplace(j, nst);
                    }
                }
            }
        }
    }
};
```

#### Go

```go
func shortestPathLength(graph [][]int) int {
	n := len(graph)
	q := [][2]int{}
	vis := make([][]bool, n)
	for i := range vis {
		vis[i] = make([]bool, 1<<n)
		vis[i][1<<i] = true
		q = append(q, [2]int{i, 1 << i})
	}
	for ans := 0; ; ans++ {
		for k := len(q); k > 0; k-- {
			p := q[0]
			q = q[1:]
			i, st := p[0], p[1]
			if st == (1<<n)-1 {
				return ans
			}
			for _, j := range graph[i] {
				nst := st | 1<<j
				if !vis[j][nst] {
					vis[j][nst] = true
					q = append(q, [2]int{j, nst})
				}
			}
		}
	}
}
```

#### TypeScript

```ts
function shortestPathLength(graph: number[][]): number {
    const n = graph.length;
    const q: number[][] = [];
    const vis: boolean[][] = new Array(n).fill(false).map(() => new Array(1 << n).fill(false));
    for (let i = 0; i < n; ++i) {
        q.push([i, 1 << i]);
        vis[i][1 << i] = true;
    }
    for (let ans = 0; ; ++ans) {
        for (let k = q.length; k; --k) {
            const [i, st] = q.shift()!;
            if (st === (1 << n) - 1) {
                return ans;
            }
            for (const j of graph[i]) {
                const nst = st | (1 << j);
                if (!vis[j][nst]) {
                    vis[j][nst] = true;
                    q.push([j, nst]);
                }
            }
        }
    }
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    #[allow(dead_code)]
    pub fn shortest_path_length(graph: Vec<Vec<i32>>) -> i32 {
        let n = graph.len();
        let mut vis = vec![vec![false; 1 << n]; n];
        let mut q = VecDeque::new();

        // Initialize the queue
        for i in 0..n {
            q.push_back(((i, 1 << i), 0));
            vis[i][1 << i] = true;
        }

        // Begin BFS
        while !q.is_empty() {
            let ((i, st), count) = q.pop_front().unwrap();
            if st == (1 << n) - 1 {
                return count;
            }
            // If the path has not been visited
            for j in &graph[i] {
                let nst = st | (1 << *j);
                if !vis[*j as usize][nst] {
                    q.push_back(((*j as usize, nst), count + 1));
                    vis[*j as usize][nst] = true;
                }
            }
        }

        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> BFS thông thường mở rộng theo từng layer. Heuristic admissible — số node chưa được thăm — cho phép priority queue ưu tiên lấy trạng thái có $dist+h$ nhỏ nhất.
>
> Việc relax vẫn tương đương tìm đường đi ngắn nhất với cạnh trọng số 1 nên vẫn bảo đảm tối ưu; các mask đầy đủ thường được tìm thấy sớm hơn khi phần lớn node đã được thăm.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestPathLength(self, graph: List[List[int]]) -> int:
        n = len(graph)

        def f(state):
            return sum(((state >> i) & 1) == 0 for i in range(n))

        q = []
        dist = [[inf] * (1 << n) for _ in range(n)]
        for i in range(n):
            heappush(q, (f(1 << i), i, 1 << i))
            dist[i][1 << i] = 0
        while q:
            _, u, state = heappop(q)
            if state == (1 << n) - 1:
                return dist[u][state]
            for v in graph[u]:
                nxt = state | (1 << v)
                if dist[v][nxt] > dist[u][state] + 1:
                    dist[v][nxt] = dist[u][state] + 1
                    heappush(q, (dist[v][nxt] + f(nxt), v, nxt))
        return 0
```

#### Java

```java
class Solution {
    private int n;

    public int shortestPathLength(int[][] graph) {
        n = graph.length;
        int[][] dist = new int[n][1 << n];
        for (int i = 0; i < n; ++i) {
            Arrays.fill(dist[i], Integer.MAX_VALUE);
        }
        PriorityQueue<int[]> q = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        for (int i = 0; i < n; ++i) {
            q.offer(new int[] {f(1 << i), i, 1 << i});
            dist[i][1 << i] = 0;
        }
        while (!q.isEmpty()) {
            int[] p = q.poll();
            int u = p[1], state = p[2];
            if (state == (1 << n) - 1) {
                return dist[u][state];
            }
            for (int v : graph[u]) {
                int nxt = state | (1 << v);
                if (dist[v][nxt] > dist[u][state] + 1) {
                    dist[v][nxt] = dist[u][state] + 1;
                    q.offer(new int[] {dist[v][nxt] + f(nxt), v, nxt});
                }
            }
        }
        return 0;
    }

    private int f(int state) {
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (((state >> i) & 1) == 0) {
                ++ans;
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
    int n;

    int shortestPathLength(vector<vector<int>>& graph) {
        n = graph.size();
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> q;
        vector<vector<int>> dist(n, vector<int>(1 << n, INT_MAX));
        for (int i = 0; i < n; ++i) {
            q.push({f(1 << i), i, 1 << i});
            dist[i][1 << i] = 0;
        }
        while (!q.empty()) {
            auto [_, u, state] = q.top();
            q.pop();
            if (state == (1 << n) - 1) return dist[u][state];
            for (int v : graph[u]) {
                int nxt = state | (1 << v);
                if (dist[v][nxt] > dist[u][state] + 1) {
                    dist[v][nxt] = dist[u][state] + 1;
                    q.push({dist[v][nxt] + f(nxt), v, nxt});
                }
            }
        }
        return 0;
    }

    int f(int state) {
        int ans = 0;
        for (int i = 0; i < n; ++i)
            if (((state >> i) & 1) == 0)
                ++ans;
        return ans;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
