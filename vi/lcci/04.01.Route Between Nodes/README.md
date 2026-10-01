---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [04.01. Route Between Nodes](https://leetcode.cn/problems/route-between-nodes-lcci)

[中文文档](/lcci/04.01.Route%20Between%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đồ thị có hướng, hãy thiết kế một thuật toán để xác định xem có đường đi giữa hai node hay không.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong> Đầu vào</strong>: n = 3, graph = [[0, 1], [0, 2], [1, 2], [1, 2]], start = 0, target = 2

<strong> Đầu ra</strong>: true

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong> Đầu vào</strong>: n = 5, graph = [[0, 1], [0, 2], [0, 4], [0, 4], [0, 1], [1, 3], [1, 4], [1, 3], [2, 3], [3, 4]], start = 0, target = 4

<strong> Đầu ra</strong> true

</pre>

<p><strong>Lưu ý: </strong></p>

<ol>
	<li><code>0 &lt;= n &lt;= 100000</code></li>
	<li>Tất cả số node đều nằm trong khoảng [0, n].</li>
	<li>Có thể có self cycle và cạnh trùng lặp.</li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Bài toán hỏi liệu $start$ có thể đi tới $target$ trong một đồ thị có hướng hay không. Không cần liệt kê các đường đi đơn; chỉ cần một lần duyệt $O(n+m)$.
>
> Vì chỉ cần biết khả năng đi tới, DFS là phù hợp: đệ quy theo các cạnh đi ra và đánh dấu $vis$ để cắt các chu kỳ.
>
> Xây dựng danh sách kề $g$, sau đó chạy DFS từ $start$: trả về true khi gặp $target$, trả về false nếu node đã được duyệt, nếu không thì đánh dấu và dùng `any` trên các node kề. Đánh dấu trước khi mở rộng giúp tránh đệ quy vô hạn khi có chu kỳ.

<!-- thinking:end -->

Trước tiên, ta xây dựng danh sách kề $g$ dựa trên đồ thị đã cho, trong đó $g[i]$ biểu diễn tất cả node kề với node $i$. Ta dùng hash table hoặc mảng $vis$ để ghi lại các node đã duyệt, sau đó bắt đầu tìm kiếm theo chiều sâu từ node $start$. Nếu tìm thấy node $target$, ta trả về `true`, ngược lại trả về `false`.

Quá trình tìm kiếm theo chiều sâu như sau:

1. Nếu node hiện tại $i$ bằng node đích $target$, trả về `true`.
2. Nếu node hiện tại $i$ đã được duyệt, trả về `false`.
3. Nếu không, đánh dấu node hiện tại $i$ đã được duyệt, sau đó duyệt qua tất cả node kề $j$ của node $i$ và tìm kiếm đệ quy node $j$.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findWhetherExistsPath(
        self, n: int, graph: List[List[int]], start: int, target: int
    ) -> bool:
        def dfs(i: int):
            if i == target:
                return True
            if i in vis:
                return False
            vis.add(i)
            return any(dfs(j) for j in g[i])

        g = [[] for _ in range(n)]
        for a, b in graph:
            g[a].append(b)
        vis = set()
        return dfs(start)
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private boolean[] vis;
    private int target;

    public boolean findWhetherExistsPath(int n, int[][] graph, int start, int target) {
        vis = new boolean[n];
        g = new List[n];
        this.target = target;
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : graph) {
            g[e[0]].add(e[1]);
        }
        return dfs(start);
    }

    private boolean dfs(int i) {
        if (i == target) {
            return true;
        }
        if (vis[i]) {
            return false;
        }
        vis[i] = true;
        for (int j : g[i]) {
            if (dfs(j)) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool findWhetherExistsPath(int n, vector<vector<int>>& graph, int start, int target) {
        vector<int> g[n];
        vector<bool> vis(n);
        for (auto& e : graph) {
            g[e[0]].push_back(e[1]);
        }
        auto dfs = [&](this auto&& dfs, int i) -> bool {
            if (i == target) {
                return true;
            }
            if (vis[i]) {
                return false;
            }
            vis[i] = true;
            for (int j : g[i]) {
                if (dfs(j)) {
                    return true;
                }
            }
            return false;
        };
        return dfs(start);
    }
};
```

#### Go

```go
func findWhetherExistsPath(n int, graph [][]int, start int, target int) bool {
	g := make([][]int, n)
	vis := make([]bool, n)
	for _, e := range graph {
		g[e[0]] = append(g[e[0]], e[1])
	}
	var dfs func(int) bool
	dfs = func(i int) bool {
		if i == target {
			return true
		}
		if vis[i] {
			return false
		}
		vis[i] = true
		for _, j := range g[i] {
			if dfs(j) {
				return true
			}
		}
		return false
	}
	return dfs(start)
}
```

#### TypeScript

```ts
function findWhetherExistsPath(
    n: number,
    graph: number[][],
    start: number,
    target: number,
): boolean {
    const g: number[][] = Array.from({ length: n }, () => []);
    const vis: boolean[] = Array.from({ length: n }, () => false);
    for (const [a, b] of graph) {
        g[a].push(b);
    }
    const dfs = (i: number): boolean => {
        if (i === target) {
            return true;
        }
        if (vis[i]) {
            return false;
        }
        vis[i] = true;
        return g[i].some(dfs);
    };
    return dfs(start);
}
```

#### Swift

```swift
class Solution {
    private var g: [[Int]]!
    private var vis: [Bool]!
    private var target: Int!

    func findWhetherExistsPath(_ n: Int, _ graph: [[Int]], _ start: Int, _ target: Int) -> Bool {
        vis = [Bool](repeating: false, count: n)
        g = [[Int]](repeating: [], count: n)
        self.target = target
        for e in graph {
            g[e[0]].append(e[1])
        }
        return dfs(start)
    }

    private func dfs(_ i: Int) -> Bool {
        if i == target {
            return true
        }
        if vis[i] {
            return false
        }
        vis[i] = true
        for j in g[i] {
            if dfs(j) {
                return true
            }
        }
        return false
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
> DFS đã xác định được khả năng đi tới, nhưng call stack có thể sâu tới $n$ trên một đường đi dài.
>
> BFS mở rộng bằng queue và dùng cùng tập $vis$, chuyển phần bộ nhớ sử dụng từ call stack sang heap nhưng vẫn cho cùng câu trả lời đúng/sai.

<!-- thinking:end -->

Tương tự Lời giải 1, trước tiên ta xây dựng danh sách kề $g$ dựa trên đồ thị đã cho, trong đó $g[i]$ biểu diễn tất cả node kề với node $i$. Ta dùng hash table hoặc mảng $vis$ để ghi lại các node đã duyệt, sau đó bắt đầu tìm kiếm theo chiều rộng từ node $start$. Nếu tìm thấy node $target$, ta trả về `true`, ngược lại trả về `false`.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findWhetherExistsPath(
        self, n: int, graph: List[List[int]], start: int, target: int
    ) -> bool:
        g = [[] for _ in range(n)]
        for a, b in graph:
            g[a].append(b)
        vis = {start}
        q = deque([start])
        while q:
            i = q.popleft()
            if i == target:
                return True
            for j in g[i]:
                if j not in vis:
                    vis.add(j)
                    q.append(j)
        return False
```

#### Java

```java
class Solution {
    public boolean findWhetherExistsPath(int n, int[][] graph, int start, int target) {
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        boolean[] vis = new boolean[n];
        for (int[] e : graph) {
            g[e[0]].add(e[1]);
        }
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(start);
        vis[start] = true;
        while (!q.isEmpty()) {
            int i = q.poll();
            if (i == target) {
                return true;
            }
            for (int j : g[i]) {
                if (!vis[j]) {
                    q.offer(j);
                    vis[j] = true;
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool findWhetherExistsPath(int n, vector<vector<int>>& graph, int start, int target) {
        vector<int> g[n];
        vector<bool> vis(n);
        for (auto& e : graph) {
            g[e[0]].push_back(e[1]);
        }
        queue<int> q{{start}};
        vis[start] = true;
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            if (i == target) {
                return true;
            }
            for (int j : g[i]) {
                if (!vis[j]) {
                    q.push(j);
                    vis[j] = true;
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func findWhetherExistsPath(n int, graph [][]int, start int, target int) bool {
	g := make([][]int, n)
	vis := make([]bool, n)
	for _, e := range graph {
		g[e[0]] = append(g[e[0]], e[1])
	}
	q := []int{start}
	vis[start] = true
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		if i == target {
			return true
		}
		for _, j := range g[i] {
			if !vis[j] {
				vis[j] = true
				q = append(q, j)
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function findWhetherExistsPath(
    n: number,
    graph: number[][],
    start: number,
    target: number,
): boolean {
    const g: number[][] = Array.from({ length: n }, () => []);
    const vis: boolean[] = Array.from({ length: n }, () => false);
    for (const [a, b] of graph) {
        g[a].push(b);
    }
    const q: number[] = [start];
    vis[start] = true;
    while (q.length > 0) {
        const i = q.pop()!;
        if (i === target) {
            return true;
        }
        for (const j of g[i]) {
            if (!vis[j]) {
                vis[j] = true;
                q.push(j);
            }
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
