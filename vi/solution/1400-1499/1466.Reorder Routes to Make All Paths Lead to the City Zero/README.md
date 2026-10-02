---
comments: true
difficulty: Medium
rating: 1633
source: Weekly Contest 191 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [1466. Reorder Routes to Make All Paths Lead to the City Zero](https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero)

[中文文档](/solution/1400-1499/1466.Reorder%20Routes%20to%20Make%20All%20Paths%20Lead%20to%20the%20City%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> thành phố được đánh số từ <code>0</code> đến <code>n - 1</code> và <code>n - 1</code> con đường, sao cho chỉ có một cách di chuyển giữa hai thành phố khác nhau (mạng lưới này tạo thành một cây). Năm ngoái, Bộ Giao thông vận tải quyết định định hướng các con đường theo một chiều vì chúng quá hẹp.</p>

<p>Các con đường được biểu diễn bởi <code>connections</code>, trong đó <code>connections[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu diễn một con đường từ thành phố <code>a<sub>i</sub></code> đến thành phố <code>b<sub>i</sub></code>.</p>

<p>Năm nay, thủ đô (thành phố <code>0</code>) sẽ tổ chức một sự kiện lớn và nhiều người muốn đi đến thành phố này.</p>

<p>Nhiệm vụ của bạn là đổi hướng một số con đường để mọi thành phố đều có thể đi đến thành phố <code>0</code>. Hãy trả về số cạnh <strong>nhỏ nhất</strong> cần thay đổi.</p>

<p><strong>Đảm bảo</strong> rằng sau khi sắp xếp lại, mọi thành phố đều có thể đi đến thành phố <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1466.Reorder%20Routes%20to%20Make%20All%20Paths%20Lead%20to%20the%20City%20Zero/images/sample_1_1819.png" style="width: 311px; height: 189px;" />
<pre>
<strong>Input:</strong> n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
<strong>Output:</strong> 3
<strong>Giải thích: </strong>Đổi hướng các cạnh được đánh dấu màu đỏ để mọi node đều có thể đi đến node 0 (thủ đô).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1466.Reorder%20Routes%20to%20Make%20All%20Paths%20Lead%20to%20the%20City%20Zero/images/sample_2_1819.png" style="width: 509px; height: 79px;" />
<pre>
<strong>Input:</strong> n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
<strong>Output:</strong> 2
<strong>Giải thích: </strong>Đổi hướng các cạnh được đánh dấu màu đỏ để mọi node đều có thể đi đến node 0 (thủ đô).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 3, connections = [[1,0],[2,0]]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>connections.length == n - 1</code></li>
	<li><code>connections[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Bỏ qua hướng, đồ thị là một cây; mọi thành phố đều phải đi đến $0$. Duyệt từ $0$ ra ngoài, cạnh ban đầu hướng ra ngoài phải được đảo chiều. Gán chi phí $1$ cho hướng đã cho và $0$ cho hướng ngược lại, sau đó chạy DFS trên các chi phí này.

<!-- thinking:end -->

Bản đồ tuyến đường trong đề bài có $n$ node và $n-1$ cạnh. Nếu bỏ qua hướng của các cạnh, $n$ node này tạo thành một cây. Bài toán yêu cầu đổi hướng một số cạnh để mọi node đều có thể đi đến node $0$.

Ta có thể xem xét việc bắt đầu từ node $0$ và đi đến tất cả node khác. Hướng này ngược với mô tả của đề bài, nên khi xây dựng đồ thị, với cạnh có hướng $[a, b]$, ta xem nó là cạnh có hướng $[b, a]$. Nói cách khác, nếu cạnh đi từ $a$ đến $b$, ta cần đổi hướng một lần; nếu cạnh đi từ $b$ đến $a$, ta không cần đổi hướng.

Tiếp theo, ta chỉ cần bắt đầu từ node $0$, duyệt tất cả node khác, và trong quá trình đó, nếu gặp một cạnh cần đổi hướng thì cộng thêm một lần đổi hướng.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số node trong bài toán.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minReorder(self, n: int, connections: List[List[int]]) -> int:
        def dfs(a: int, fa: int) -> int:
            return sum(c + dfs(b, a) for b, c in g[a] if b != fa)

        g = [[] for _ in range(n)]
        for a, b in connections:
            g[a].append((b, 1))
            g[b].append((a, 0))
        return dfs(0, -1)
```

#### Java

```java
class Solution {
    private List<int[]>[] g;

    public int minReorder(int n, int[][] connections) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : connections) {
            int a = e[0], b = e[1];
            g[a].add(new int[] {b, 1});
            g[b].add(new int[] {a, 0});
        }
        return dfs(0, -1);
    }

    private int dfs(int a, int fa) {
        int ans = 0;
        for (var e : g[a]) {
            int b = e[0], c = e[1];
            if (b != fa) {
                ans += c + dfs(b, a);
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
    int minReorder(int n, vector<vector<int>>& connections) {
        vector<pair<int, int>> g[n];
        for (auto& e : connections) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b, 1);
            g[b].emplace_back(a, 0);
        }
        function<int(int, int)> dfs = [&](int a, int fa) {
            int ans = 0;
            for (auto& [b, c] : g[a]) {
                if (b != fa) {
                    ans += c + dfs(b, a);
                }
            }
            return ans;
        };
        return dfs(0, -1);
    }
};
```

#### Go

```go
func minReorder(n int, connections [][]int) int {
	g := make([][][2]int, n)
	for _, e := range connections {
		a, b := e[0], e[1]
		g[a] = append(g[a], [2]int{b, 1})
		g[b] = append(g[b], [2]int{a, 0})
	}
	var dfs func(int, int) int
	dfs = func(a, fa int) (ans int) {
		for _, e := range g[a] {
			if b, c := e[0], e[1]; b != fa {
				ans += c + dfs(b, a)
			}
		}
		return
	}
	return dfs(0, -1)
}
```

#### TypeScript

```ts
function minReorder(n: number, connections: number[][]): number {
    const g: [number, number][][] = Array.from({ length: n }, () => []);
    for (const [a, b] of connections) {
        g[a].push([b, 1]);
        g[b].push([a, 0]);
    }
    const dfs = (a: number, fa: number): number => {
        let ans = 0;
        for (const [b, c] of g[a]) {
            if (b !== fa) {
                ans += c + dfs(b, a);
            }
        }
        return ans;
    };
    return dfs(0, -1);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_reorder(n: i32, connections: Vec<Vec<i32>>) -> i32 {
        let mut g: Vec<Vec<(i32, i32)>> = vec![vec![]; n as usize];
        for e in connections.iter() {
            let a = e[0] as usize;
            let b = e[1] as usize;
            g[a].push((b as i32, 1));
            g[b].push((a as i32, 0));
        }
        fn dfs(a: usize, fa: i32, g: &Vec<Vec<(i32, i32)>>) -> i32 {
            let mut ans = 0;
            for &(b, c) in g[a].iter() {
                if b != fa {
                    ans += c + dfs(b as usize, a as i32, g);
                }
            }
            ans
        }
        dfs(0, -1, &g)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 sử dụng đệ quy. Ta có thể duyệt cùng adjacency list bằng BFS từ $0$, cộng chi phí của cạnh khi lần đầu gặp một node kề mới.

<!-- thinking:end -->

Ta có thể sử dụng phương pháp Breadth-First Search (BFS), bắt đầu từ node $0$, để duyệt tất cả node khác. Trong quá trình đó, nếu gặp một cạnh cần đổi hướng, ta tăng số lần đổi hướng.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số node trong bài toán.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minReorder(self, n: int, connections: List[List[int]]) -> int:
        g = [[] for _ in range(n)]
        for a, b in connections:
            g[a].append((b, 1))
            g[b].append((a, 0))
        q = deque([0])
        vis = {0}
        ans = 0
        while q:
            a = q.popleft()
            for b, c in g[a]:
                if b not in vis:
                    vis.add(b)
                    q.append(b)
                    ans += c
        return ans
```

```java
class Solution {
    public int minReorder(int n, int[][] connections) {
        List<int[]>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : connections) {
            int a = e[0], b = e[1];
            g[a].add(new int[] {b, 1});
            g[b].add(new int[] {a, 0});
        }
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        boolean[] vis = new boolean[n];
        vis[0] = true;
        int ans = 0;
        while (!q.isEmpty()) {
            int a = q.poll();
            for (var e : g[a]) {
                int b = e[0], c = e[1];
                if (!vis[b]) {
                    vis[b] = true;
                    q.offer(b);
                    ans += c;
                }
            }
        }
        return ans;
    }
}
```

```cpp
class Solution {
public:
    int minReorder(int n, vector<vector<int>>& connections) {
        vector<pair<int, int>> g[n];
        for (auto& e : connections) {
            int a = e[0], b = e[1];
            g[a].emplace_back(b, 1);
            g[b].emplace_back(a, 0);
        }
        queue<int> q{{0}};
        vector<bool> vis(n);
        vis[0] = true;
        int ans = 0;
        while (q.size()) {
            int a = q.front();
            q.pop();
            for (auto& [b, c] : g[a]) {
                if (!vis[b]) {
                    vis[b] = true;
                    q.push(b);
                    ans += c;
                }
            }
        }
        return ans;
    }
};
```

```go
func minReorder(n int, connections [][]int) (ans int) {
	g := make([][][2]int, n)
	for _, e := range connections {
		a, b := e[0], e[1]
		g[a] = append(g[a], [2]int{b, 1})
		g[b] = append(g[b], [2]int{a, 0})
	}
	q := []int{0}
	vis := make([]bool, n)
	vis[0] = true
	for len(q) > 0 {
		a := q[0]
		q = q[1:]
		for _, e := range g[a] {
			b, c := e[0], e[1]
			if !vis[b] {
				vis[b] = true
				q = append(q, b)
				ans += c
			}
		}
	}
	return
}
```

```ts
function minReorder(n: number, connections: number[][]): number {
    const g: [number, number][][] = Array.from({ length: n }, () => []);
    for (const [a, b] of connections) {
        g[a].push([b, 1]);
        g[b].push([a, 0]);
    }

    const q: number[] = [0];
    const vis = new Set<number>();
    vis.add(0);

    let ans = 0;
    while (q.length) {
        const a = q.pop()!;
        for (const [b, c] of g[a]) {
            if (!vis.has(b)) {
                vis.add(b);
                q.push(b);
                ans += c;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
