---
comments: true
difficulty: Medium
rating: 1476
source: Weekly Contest 305 Q2
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2368. Reachable Nodes With Restrictions](https://leetcode.com/problems/reachable-nodes-with-restrictions)

[中文文档](/solution/2300-2399/2368.Reachable%20Nodes%20With%20Restrictions/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code> và <code>n - 1</code> cạnh.</p>

<p>Cho một mảng số nguyên 2D <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một cạnh nối node <code>a<sub>i</sub></code> và node <code>b<sub>i</sub></code> trong cây. Ngoài ra, cho một mảng số nguyên <code>restricted</code> biểu diễn các node bị <strong>hạn chế</strong>.</p>

<p>Trả về <em>số lượng <strong>lớn nhất</strong> các node có thể đi tới từ node </em><code>0</code><em> mà không đi qua node bị hạn chế.</em></p>

<p>Lưu ý rằng node <code>0</code> sẽ <strong>không</strong> phải là node bị hạn chế.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2368.Reachable%20Nodes%20With%20Restrictions/images/ex1drawio.png" style="width: 402px; height: 322px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[0,1],[1,2],[3,1],[4,0],[0,5],[5,6]], restricted = [4,5]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Hình trên biểu diễn cây.
Chỉ có các node [0,1,2,3] có thể đi tới từ node 0 mà không đi qua node bị hạn chế.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2368.Reachable%20Nodes%20With%20Restrictions/images/ex2drawio.png" style="width: 412px; height: 312px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[0,1],[0,2],[0,5],[0,4],[3,2],[6,5]], restricted = [4,2,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Hình trên biểu diễn cây.
Chỉ có các node [0,5,6] có thể đi tới từ node 0 mà không đi qua node bị hạn chế.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>edges</code> biểu diễn một cây hợp lệ.</li>
	<li><code>1 &lt;= restricted.length &lt; n</code></li>
	<li><code>1 &lt;= restricted[i] &lt; n</code></li>
	<li>Tất cả các giá trị trong <code>restricted</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Xuất phát từ node $0$ trên một cây, ta không thể đi vào các node bị hạn chế. Vì $n \le 10^5$, chỉ cần một lần duyệt. Ta coi các node bị hạn chế như đã được thăm.
>
> Xây dựng danh sách kề rồi dùng DFS: đánh dấu node hiện tại và đệ quy trên các node kề chưa thăm, đồng thời trả về số node có thể đi tới.

<!-- thinking:end -->

Trước hết, ta xây dựng danh sách kề $g$ dựa trên các cạnh đã cho, trong đó $g[i]$ biểu diễn danh sách các node kề với node $i$. Sau đó, ta định nghĩa một hash table $vis$ để ghi nhận các node bị hạn chế hoặc đã được thăm, rồi ban đầu thêm các node bị hạn chế vào $vis$.

Tiếp theo, ta định nghĩa hàm tìm kiếm theo chiều sâu $dfs(i)$, biểu diễn số node có thể đi tới khi bắt đầu từ node $i$. Trong hàm $dfs(i)$, trước tiên ta thêm node $i$ vào $vis$, sau đó duyệt các node $j$ kề với node $i$. Nếu $j$ không có trong $vis$, ta gọi đệ quy $dfs(j)$ và cộng giá trị trả về vào kết quả.

Cuối cùng, ta trả về $dfs(0)$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reachableNodes(
        self, n: int, edges: List[List[int]], restricted: List[int]
    ) -> int:
        def dfs(i: int) -> int:
            vis.add(i)
            return 1 + sum(j not in vis and dfs(j) for j in g[i])

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        vis = set(restricted)
        return dfs(0)
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private boolean[] vis;

    public int reachableNodes(int n, int[][] edges, int[] restricted) {
        g = new List[n];
        vis = new boolean[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        for (int i : restricted) {
            vis[i] = true;
        }
        return dfs(0);
    }

    private int dfs(int i) {
        vis[i] = true;
        int ans = 1;
        for (int j : g[i]) {
            if (!vis[j]) {
                ans += dfs(j);
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
    int reachableNodes(int n, vector<vector<int>>& edges, vector<int>& restricted) {
        vector<int> g[n];
        vector<int> vis(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b);
            g[b].emplace_back(a);
        }
        for (int i : restricted) {
            vis[i] = true;
        }
        function<int(int)> dfs = [&](int i) {
            vis[i] = true;
            int ans = 1;
            for (int j : g[i]) {
                if (!vis[j]) {
                    ans += dfs(j);
                }
            }
            return ans;
        };
        return dfs(0);
    }
};
```

#### Go

```go
func reachableNodes(n int, edges [][]int, restricted []int) int {
	g := make([][]int, n)
	vis := make([]bool, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	for _, i := range restricted {
		vis[i] = true
	}
	var dfs func(int) int
	dfs = func(i int) (ans int) {
		vis[i] = true
		ans = 1
		for _, j := range g[i] {
			if !vis[j] {
				ans += dfs(j)
			}
		}
		return
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function reachableNodes(n: number, edges: number[][], restricted: number[]): number {
    const vis: boolean[] = Array(n).fill(false);
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    for (const i of restricted) {
        vis[i] = true;
    }
    const dfs = (i: number): number => {
        vis[i] = true;
        let ans = 1;
        for (const j of g[i]) {
            if (!vis[j]) {
                ans += dfs(j);
            }
        }
        return ans;
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: BFS

<!-- thinking:start -->

> **Tư duy**
>
> DFS có thể gây tràn stack trên một cây có độ sâu lớn. Ta có thể dùng cùng một tập node đã thăm với BFS dùng queue để tránh độ sâu đệ quy.

<!-- thinking:end -->

Tương tự Lời giải 1, trước tiên ta xây dựng danh sách kề $g$ dựa trên các cạnh đã cho, sau đó định nghĩa một hash table $vis$ để ghi nhận các node bị hạn chế hoặc đã được thăm, rồi ban đầu thêm các node bị hạn chế vào $vis$.

Tiếp theo, ta dùng tìm kiếm theo chiều rộng để duyệt toàn bộ đồ thị và đếm số node có thể đi tới. Ta định nghĩa một queue $q$, ban đầu thêm node $0$ vào $q$ và thêm node $0$ vào $vis$. Sau đó, ta liên tục lấy node $i$ ra khỏi $q$, tăng kết quả, rồi thêm các node kề với node $i$ chưa được thăm vào $q$ và $vis$.

Sau khi duyệt xong, ta trả về kết quả.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reachableNodes(
        self, n: int, edges: List[List[int]], restricted: List[int]
    ) -> int:
        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        vis = set(restricted + [0])
        q = deque([0])
        ans = 0
        while q:
            i = q.popleft()
            ans += 1
            for j in g[i]:
                if j not in vis:
                    q.append(j)
                    vis.add(j)
        return ans
```

#### Java

```java
class Solution {
    public int reachableNodes(int n, int[][] edges, int[] restricted) {
        List<Integer>[] g = new List[n];
        boolean[] vis = new boolean[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        for (int v : restricted) {
            vis[v] = true;
        }
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        int ans = 0;
        for (vis[0] = true; !q.isEmpty(); ++ans) {
            int i = q.pollFirst();
            for (int j : g[i]) {
                if (!vis[j]) {
                    q.offer(j);
                    vis[j] = true;
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
    int reachableNodes(int n, vector<vector<int>>& edges, vector<int>& restricted) {
        vector<int> g[n];
        vector<int> vis(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b);
            g[b].emplace_back(a);
        }
        for (int i : restricted) {
            vis[i] = true;
        }
        queue<int> q{{0}};
        int ans = 0;
        for (vis[0] = true; !q.empty(); ++ans) {
            int i = q.front();
            q.pop();
            for (int j : g[i]) {
                if (!vis[j]) {
                    vis[j] = true;
                    q.push(j);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func reachableNodes(n int, edges [][]int, restricted []int) (ans int) {
	g := make([][]int, n)
	vis := make([]bool, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	for _, i := range restricted {
		vis[i] = true
	}
	q := []int{0}
	for vis[0] = true; len(q) > 0; ans++ {
		i := q[0]
		q = q[1:]
		for _, j := range g[i] {
			if !vis[j] {
				vis[j] = true
				q = append(q, j)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function reachableNodes(n: number, edges: number[][], restricted: number[]): number {
    const vis: boolean[] = Array(n).fill(false);
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    for (const i of restricted) {
        vis[i] = true;
    }
    const q: number[] = [0];
    let ans = 0;
    for (vis[0] = true; q.length; ++ans) {
        const i = q.pop()!;
        for (const j of g[i]) {
            if (!vis[j]) {
                vis[j] = true;
                q.push(j);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
