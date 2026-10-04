---
comments: true
difficulty: Medium
rating: 1567
source: Weekly Contest 409 Q2
tags:
    - Breadth-First Search
    - Graph
    - Array
---

<!-- problem:start -->

# [3243. Shortest Distance After Road Addition Queries I](https://leetcode.com/problems/shortest-distance-after-road-addition-queries-i)

[中文文档](/solution/3200-3299/3243.Shortest%20Distance%20After%20Road%20Addition%20Queries%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> và một mảng số nguyên hai chiều <code>queries</code>.</p>

<p>Có <code>n</code> thành phố được đánh số từ <code>0</code> đến <code>n - 1</code>. Ban đầu, có một con đường <strong>một chiều</strong> từ thành phố <code>i</code> đến thành phố <code>i + 1</code> với mọi <code>0 &lt;= i &lt; n - 1</code>.</p>

<p><code>queries[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu diễn việc thêm một con đường <strong>một chiều</strong> từ thành phố <code>u<sub>i</sub></code> đến thành phố <code>v<sub>i</sub></code>. Sau mỗi truy vấn, bạn cần tìm <strong>độ dài</strong> của <strong>đường đi ngắn nhất</strong> từ thành phố <code>0</code> đến thành phố <code>n - 1</code>.</p>

<p>Trả về một mảng <code>answer</code>, trong đó với mỗi <code>i</code> thuộc đoạn <code>[0, queries.length - 1]</code>, <code>answer[i]</code> là <em>độ dài của đường đi ngắn nhất</em> từ thành phố <code>0</code> đến thành phố <code>n - 1</code> sau khi xử lý <strong>tổng cộng </strong><code>i + 1</code> truy vấn đầu tiên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, queries = [[2,4],[0,2],[0,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,2,1]</span></p>

<p><strong>Giải thích: </strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3243.Shortest%20Distance%20After%20Road%20Addition%20Queries%20I/images/image8.jpg" style="width: 350px; height: 60px;" /></p>

<p>Sau khi thêm con đường từ 2 đến 4, độ dài của đường đi ngắn nhất từ 0 đến 4 là 3.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3243.Shortest%20Distance%20After%20Road%20Addition%20Queries%20I/images/image9.jpg" style="width: 350px; height: 60px;" /></p>

<p>Sau khi thêm con đường từ 0 đến 2, độ dài của đường đi ngắn nhất từ 0 đến 4 là 2.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3243.Shortest%20Distance%20After%20Road%20Addition%20Queries%20I/images/image10.jpg" style="width: 350px; height: 96px;" /></p>

<p>Sau khi thêm con đường từ 0 đến 4, độ dài của đường đi ngắn nhất từ 0 đến 4 là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, queries = [[0,3],[0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3243.Shortest%20Distance%20After%20Road%20Addition%20Queries%20I/images/image11.jpg" style="width: 300px; height: 70px;" /></p>

<p>Sau khi thêm con đường từ 0 đến 3, độ dài của đường đi ngắn nhất từ 0 đến 3 là 1.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3243.Shortest%20Distance%20After%20Road%20Addition%20Queries%20I/images/image12.jpg" style="width: 300px; height: 70px;" /></p>

<p>Sau khi thêm con đường từ 0 đến 2, độ dài của đường đi ngắn nhất vẫn là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>3 &lt;= n &lt;= 500</code></li>
    <li><code>1 &lt;= queries.length &lt;= 500</code></li>
    <li><code>queries[i].length == 2</code></li>
    <li><code>0 &lt;= queries[i][0] &lt; queries[i][1] &lt; n</code></li>
    <li><code>1 &lt; queries[i][1] - queries[i][0]</code></li>
    <li>Không có con đường nào bị lặp lại trong các truy vấn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Đồ thị ban đầu là đường đi $0\to 1\to\cdots\to n-1$; sau mỗi cạnh hướng về phía trước, ta cần tìm khoảng cách từ $0$ đến $n-1$. Vì $n,q\le 500$, việc tìm đường đi ngắn nhất mới sau mỗi truy vấn là hoàn toàn phù hợp.
>
> Các cạnh đều có trọng số $1$, vì vậy ta chạy BFS từ $0$ sau mỗi lần thêm cạnh để ghi nhận khoảng cách. Tổng thời gian là $O(q(n+q))$.

<!-- thinking:end -->

Trước hết, ta xây dựng một đồ thị có hướng $\textit{g}$, trong đó $\textit{g}[i]$ biểu diễn danh sách các thành phố có thể đi tới từ thành phố $i$. Ban đầu, mỗi thành phố $i$ có một con đường một chiều đến thành phố $i + 1$.

Sau đó, với mỗi truy vấn $[u, v]$, ta thêm $v$ vào danh sách các thành phố có thể đi tới từ $u$, rồi sử dụng BFS để tìm độ dài đường đi ngắn nhất từ thành phố $0$ đến thành phố $n - 1$ và thêm kết quả vào mảng answer.

Cuối cùng, ta trả về mảng answer.

Độ phức tạp thời gian là $O(q \times (n + q))$, còn độ phức tạp không gian là $O(n + q)$. Trong đó, $n$ và $q$ lần lượt là số thành phố và số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestDistanceAfterQueries(
        self, n: int, queries: List[List[int]]
    ) -> List[int]:
        def bfs(i: int) -> int:
            q = deque([i])
            vis = [False] * n
            vis[i] = True
            d = 0
            while 1:
                for _ in range(len(q)):
                    u = q.popleft()
                    if u == n - 1:
                        return d
                    for v in g[u]:
                        if not vis[v]:
                            vis[v] = True
                            q.append(v)
                d += 1

        g = [[i + 1] for i in range(n - 1)]
        ans = []
        for u, v in queries:
            g[u].append(v)
            ans.append(bfs(0))
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int n;

    public int[] shortestDistanceAfterQueries(int n, int[][] queries) {
        this.n = n;
        g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int i = 0; i < n - 1; ++i) {
            g[i].add(i + 1);
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int u = queries[i][0], v = queries[i][1];
            g[u].add(v);
            ans[i] = bfs(0);
        }
        return ans;
    }

    private int bfs(int i) {
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(i);
        boolean[] vis = new boolean[n];
        vis[i] = true;
        for (int d = 0;; ++d) {
            for (int k = q.size(); k > 0; --k) {
                int u = q.poll();
                if (u == n - 1) {
                    return d;
                }
                for (int v : g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        q.offer(v);
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
    vector<int> shortestDistanceAfterQueries(int n, vector<vector<int>>& queries) {
        vector<int> g[n];
        for (int i = 0; i < n - 1; ++i) {
            g[i].push_back(i + 1);
        }
        auto bfs = [&](int i) -> int {
            queue<int> q{{i}};
            vector<bool> vis(n);
            vis[i] = true;
            for (int d = 0;; ++d) {
                for (int k = q.size(); k; --k) {
                    int u = q.front();
                    q.pop();
                    if (u == n - 1) {
                        return d;
                    }
                    for (int v : g[u]) {
                        if (!vis[v]) {
                            vis[v] = true;
                            q.push(v);
                        }
                    }
                }
            }
        };
        vector<int> ans;
        for (const auto& q : queries) {
            g[q[0]].push_back(q[1]);
            ans.push_back(bfs(0));
        }
        return ans;
    }
};
```

#### Go

```go
func shortestDistanceAfterQueries(n int, queries [][]int) []int {
    g := make([][]int, n)
    for i := range g {
        g[i] = append(g[i], i+1)
    }
    bfs := func(i int) int {
        q := []int{i}
        vis := make([]bool, n)
        vis[i] = true
        for d := 0; ; d++ {
            for k := len(q); k > 0; k-- {
                u := q[0]
                if u == n-1 {
                    return d
                }
                q = q[1:]
                for _, v := range g[u] {
                    if !vis[v] {
                        vis[v] = true
                        q = append(q, v)
                    }
                }
            }
        }
    }
    ans := make([]int, len(queries))
    for i, q := range queries {
        g[q[0]] = append(g[q[0]], q[1])
        ans[i] = bfs(0)
    }
    return ans
}
```

#### TypeScript

```ts
function shortestDistanceAfterQueries(n: number, queries: number[][]): number[] {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (let i = 0; i < n - 1; ++i) {
        g[i].push(i + 1);
    }
    const bfs = (i: number): number => {
        const q: number[] = [i];
        const vis: boolean[] = Array(n).fill(false);
        vis[i] = true;
        for (let d = 0; ; ++d) {
            const nq: number[] = [];
            for (const u of q) {
                if (u === n - 1) {
                    return d;
                }
                for (const v of g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        nq.push(v);
                    }
                }
            }
            q.splice(0, q.length, ...nq);
        }
    };
    const ans: number[] = [];
    for (const [u, v] of queries) {
        g[u].push(v);
        ans.push(bfs(0));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
