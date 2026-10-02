---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Graph Coloring
    - Bipartite Graph
---

<!-- problem:start -->

# [785. Is Graph Bipartite](https://leetcode.com/problems/is-graph-bipartite)

[中文文档](/solution/0700-0799/0785.Is%20Graph%20Bipartite/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đồ thị <strong>vô hướng</strong> gồm <code>n</code> node, mỗi node được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho mảng 2 chiều <code>graph</code>, trong đó <code>graph[u]</code> là mảng các node kề với node <code>u</code>. Cụ thể hơn, với mỗi <code>v</code> trong <code>graph[u]</code>, tồn tại một cạnh vô hướng nối node <code>u</code> với node <code>v</code>. Đồ thị có các tính chất sau:</p>

<ul>
	<li>Không có cạnh tự nối (<code>graph[u]</code> không chứa <code>u</code>).</li>
	<li>Không có cạnh song song (<code>graph[u]</code> không chứa giá trị trùng lặp).</li>
	<li>Nếu <code>v</code> nằm trong <code>graph[u]</code>, thì <code>u</code> cũng nằm trong <code>graph[v]</code> (đồ thị là vô hướng).</li>
	<li>Đồ thị có thể không liên thông, nghĩa là có thể tồn tại hai node <code>u</code> và <code>v</code> không có đường đi nối chúng.</li>
</ul>

<p>Đồ thị được gọi là <strong>hai phía</strong> nếu có thể chia các node thành hai tập độc lập <code>A</code> và <code>B</code>, sao cho <strong>mọi</strong> cạnh trong đồ thị đều nối một node thuộc tập <code>A</code> với một node thuộc tập <code>B</code>.</p>

<p>Trả về <code>true</code><em> khi và chỉ khi đồ thị là <strong>hai phía</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0785.Is%20Graph%20Bipartite/images/bi2.jpg" style="width: 222px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể chia các node thành hai tập độc lập sao cho mỗi cạnh đều nối một node của tập này với một node của tập kia.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0785.Is%20Graph%20Bipartite/images/bi1.jpg" style="width: 222px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> graph = [[1,3],[0,2],[1,3],[0,2]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể chia các node thành hai tập: {0, 2} và {1, 3}.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>graph.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= graph[u].length &lt; n</code></li>
	<li><code>0 &lt;= graph[u][i] &lt;= n - 1</code></li>
	<li><code>graph[u]</code>&nbsp;không chứa&nbsp;<code>u</code>.</li>
	<li>Tất cả giá trị trong <code>graph[u]</code> đều <strong>khác nhau</strong>.</li>
	<li>Nếu <code>graph[u]</code> chứa <code>v</code>, thì <code>graph[v]</code> chứa <code>u</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tô màu để xác định đồ thị hai phía

<!-- thinking:start -->

> **Tư duy**
>
> Xác định đồ thị vô hướng có phải đồ thị hai phía hay không. $n\le 100$. Chạy DFS từ mỗi đỉnh chưa tô màu, dùng hai màu; nếu một đỉnh kề đã được tô cùng màu thì không thỏa mãn.
>
> Đồ thị có thể không liên thông, vì vậy cần tô màu mọi thành phần liên thông.

<!-- thinking:end -->

Duyệt qua tất cả node để tô màu. Chẳng hạn, ban đầu xem chúng là màu trắng rồi dùng DFS tô các node kề bằng màu khác. Nếu màu cần tô cho một node trùng với màu node đó đã được tô, thì đồ thị không thể là đồ thị hai phía.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        def dfs(a: int, c: int) -> bool:
            color[a] = c
            for b in graph[a]:
                if color[b] == c or (color[b] == 0 and not dfs(b, -c)):
                    return False
            return True

        n = len(graph)
        color = [0] * n
        for i in range(n):
            if color[i] == 0 and not dfs(i, 1):
                return False
        return True
```

#### Java

```java
class Solution {
    private int[] color;
    private int[][] g;

    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        color = new int[n];
        g = graph;
        for (int i = 0; i < n; ++i) {
            if (color[i] == 0 && !dfs(i, 1)) {
                return false;
            }
        }
        return true;
    }

    private boolean dfs(int a, int c) {
        color[a] = c;
        for (int b : g[a]) {
            if (color[b] == c || (color[b] == 0 && !dfs(b, -c))) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n);
        for (int i = 0; i < n; ++i)
            if (!color[i] && !dfs(i, 1, color, graph))
                return false;
        return true;
    }

    bool dfs(int u, int c, vector<int>& color, vector<vector<int>>& g) {
        color[u] = c;
        for (int& v : g[u]) {
            if (!color[v]) {
                if (!dfs(v, 3 - c, color, g)) return false;
            } else if (color[v] == c)
                return false;
        }
        return true;
    }
};
```

#### Go

```go
func isBipartite(graph [][]int) bool {
	n := len(graph)
	color := make([]int, n)
	var dfs func(int, int) bool
	dfs = func(a, c int) bool {
		color[a] = c
		for _, b := range graph[a] {
			if color[b] == c || (color[b] == 0 && !dfs(b, -c)) {
				return false
			}
		}
		return true
	}
	for i := range graph {
		if color[i] == 0 && !dfs(i, 1) {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isBipartite(graph: number[][]): boolean {
    const n = graph.length;
    const color: number[] = Array(n).fill(0);
    const dfs = (a: number, c: number): boolean => {
        color[a] = c;
        for (const b of graph[a]) {
            if (color[b] === c || (color[b] === 0 && !dfs(b, -c))) {
                return false;
            }
        }
        return true;
    };
    for (let i = 0; i < n; i++) {
        if (color[i] === 0 && !dfs(i, 1)) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_bipartite(graph: Vec<Vec<i32>>) -> bool {
        let n = graph.len();
        let mut color = vec![0; n];

        fn dfs(a: usize, c: i32, graph: &Vec<Vec<i32>>, color: &mut Vec<i32>) -> bool {
            color[a] = c;
            for &b in &graph[a] {
                if color[b as usize] == c
                    || (color[b as usize] == 0 && !dfs(b as usize, -c, graph, color))
                {
                    return false;
                }
            }
            true
        }

        for i in 0..n {
            if color[i] == 0 && !dfs(i, 1, &graph, &mut color) {
                return false;
            }
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Tô màu tương đương với union-find: mọi node kề với $a$ thuộc cùng một tập, còn $a$ thuộc tập kia.
>
> Nếu $a$ và một node kề có cùng root thì không thỏa mãn; nếu không, hợp các node kề vào cùng một tập.

<!-- thinking:end -->

Với đồ thị hai phía, mọi node kề với một đỉnh phải thuộc cùng một tập và khác tập với chính đỉnh đó. Vì vậy, ta có thể dùng union-find. Duyệt từng đỉnh trong đồ thị; nếu đỉnh hiện tại và một node kề của nó thuộc cùng tập thì đồ thị không phải hai phía. Nếu không, hợp các node kề với node hiện tại vào cùng một tập.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(len(graph)))
        for a, bs in enumerate(graph):
            for b in bs:
                pa, pb = find(a), find(b)
                if pa == pb:
                    return False
                p[pb] = find(bs[0])
        return True
```

#### Java

```java
class Solution {
    private int[] p;

    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        for (int a = 0; a < n; ++a) {
            for (int b : graph[a]) {
                int pa = find(a), pb = find(b);
                if (pa == pb) {
                    return false;
                }
                p[pb] = find(graph[a][0]);
            }
        }
        return true;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        auto find = [&](this auto&& find, int x) -> int {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        for (int a = 0; a < n; ++a) {
            for (int b : graph[a]) {
                int pa = find(a), pb = find(b);
                if (pa == pb) {
                    return false;
                }
                p[pb] = find(graph[a][0]);
            }
        }
        return true;
    }
};
```

#### Go

```go
func isBipartite(graph [][]int) bool {
	n := len(graph)
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	var find func(x int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for a, bs := range graph {
		for _, b := range bs {
			pa, pb := find(a), find(b)
			if pa == pb {
				return false
			}
			p[pb] = find(bs[0])
		}
	}
	return true
}
```

#### TypeScript

```ts
function isBipartite(graph: number[][]): boolean {
    const n = graph.length;
    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const find = (x: number): number => {
        if (x !== p[x]) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    for (let a = 0; a < n; ++a) {
        for (const b of graph[a]) {
            const [pa, pb] = [find(a), find(b)];
            if (pa === pb) {
                return false;
            }
            p[pb] = find(graph[a][0]);
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_bipartite(graph: Vec<Vec<i32>>) -> bool {
        let n = graph.len();
        let mut p: Vec<usize> = (0..n).collect();

        fn find(x: usize, p: &mut Vec<usize>) -> usize {
            if p[x] != x {
                p[x] = find(p[x], p);
            }
            p[x]
        }

        for a in 0..n {
            for &b in &graph[a] {
                let pa = find(a, &mut p);
                let pb = find(b as usize, &mut p);
                if pa == pb {
                    return false;
                }
                p[pb] = find(graph[a][0] as usize, &mut p);
            }
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
