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

# [886. Possible Bipartition](https://leetcode.com/problems/possible-bipartition)

[中文文档](/solution/0800-0899/0886.Possible%20Bipartition/README.md)

## Mô tả

<!-- description:start -->

<p>Ta muốn chia <code>n</code> người (được đánh số từ <code>1</code> đến <code>n</code>) thành hai nhóm có <strong>kích thước bất kỳ</strong>. Một người có thể không thích một số người khác, và những người như vậy không được ở cùng nhóm.</p>

<p>Cho số nguyên <code>n</code> và mảng <code>dislikes</code>, trong đó <code>dislikes[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết người số <code>a<sub>i</sub></code> không thích người số <code>b<sub>i</sub></code>. Trả về <code>true</code> <em>nếu có thể chia mọi người thành hai nhóm theo cách trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 4, dislikes = [[1,2],[1,3],[2,4]]
<strong>Output:</strong> true
<strong>Giải thích:</strong> Nhóm thứ nhất gồm [1,4], nhóm thứ hai gồm [2,3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 3, dislikes = [[1,2],[1,3],[2,3]]
<strong>Output:</strong> false
<strong>Giải thích:</strong> Ta cần ít nhất 3 nhóm để chia những người này, nên không thể chia thành hai nhóm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2000</code></li>
	<li><code>0 &lt;= dislikes.length &lt;= 10<sup>4</sup></code></li>
	<li><code>dislikes[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub> &lt; b<sub>i</sub> &lt;= n</code></li>
	<li>Tất cả cặp trong <code>dislikes</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Những người không thích nhau không thể ở cùng nhóm; nói cách khác, đồ thị biểu diễn quan hệ không thích phải là đồ thị hai phía. Vì $n\le 2000$, dùng DFS tô màu là đủ: các node kề nhau nhận màu đối lập.
>
> Với mỗi node chưa được tô màu, gán màu $1$ rồi đệ quy với màu $3-c$. Nếu mọi thành phần liên thông đều thỏa mãn, ta có thể chia nhóm.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def possibleBipartition(self, n: int, dislikes: List[List[int]]) -> bool:
        def dfs(i, c):
            color[i] = c
            for j in g[i]:
                if color[j] == c:
                    return False
                if color[j] == 0 and not dfs(j, 3 - c):
                    return False
            return True

        g = defaultdict(list)
        color = [0] * n
        for a, b in dislikes:
            a, b = a - 1, b - 1
            g[a].append(b)
            g[b].append(a)
        return all(c or dfs(i, 1) for i, c in enumerate(color))
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int[] color;

    public boolean possibleBipartition(int n, int[][] dislikes) {
        g = new List[n];
        color = new int[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : dislikes) {
            int a = e[0] - 1, b = e[1] - 1;
            g[a].add(b);
            g[b].add(a);
        }
        for (int i = 0; i < n; ++i) {
            if (color[i] == 0) {
                if (!dfs(i, 1)) {
                    return false;
                }
            }
        }
        return true;
    }

    private boolean dfs(int i, int c) {
        color[i] = c;
        for (int j : g[i]) {
            if (color[j] == c) {
                return false;
            }
            if (color[j] == 0 && !dfs(j, 3 - c)) {
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
    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        unordered_map<int, vector<int>> g;
        for (auto& e : dislikes) {
            int a = e[0] - 1, b = e[1] - 1;
            g[a].push_back(b);
            g[b].push_back(a);
        }
        vector<int> color(n);
        function<bool(int, int)> dfs = [&](int i, int c) -> bool {
            color[i] = c;
            for (int j : g[i]) {
                if (!color[j] && !dfs(j, 3 - c)) return false;
                if (color[j] == c) return false;
            }
            return true;
        };
        for (int i = 0; i < n; ++i) {
            if (!color[i] && !dfs(i, 1)) return false;
        }
        return true;
    }
};
```

#### Go

```go
func possibleBipartition(n int, dislikes [][]int) bool {
	g := make([][]int, n)
	for _, e := range dislikes {
		a, b := e[0]-1, e[1]-1
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	color := make([]int, n)
	var dfs func(int, int) bool
	dfs = func(i, c int) bool {
		color[i] = c
		for _, j := range g[i] {
			if color[j] == c {
				return false
			}
			if color[j] == 0 && !dfs(j, 3-c) {
				return false
			}
		}
		return true
	}
	for i, c := range color {
		if c == 0 && !dfs(i, 1) {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function possibleBipartition(n: number, dislikes: number[][]): boolean {
    const color = new Array(n + 1).fill(0);
    const g = Array.from({ length: n + 1 }, () => []);
    const dfs = (i: number, v: number) => {
        color[i] = v;
        for (const j of g[i]) {
            if (color[j] === color[i] || (color[j] === 0 && dfs(j, 3 ^ v))) {
                return true;
            }
        }
        return false;
    };
    for (const [a, b] of dislikes) {
        g[a].push(b);
        g[b].push(a);
    }
    for (let i = 1; i <= n; i++) {
        if (color[i] === 0 && dfs(i, 1)) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    fn dfs(i: usize, v: usize, color: &mut Vec<usize>, g: &Vec<Vec<usize>>) -> bool {
        color[i] = v;
        for &j in (*g[i]).iter() {
            if color[j] == color[i] || (color[j] == 0 && Self::dfs(j, v ^ 3, color, g)) {
                return true;
            }
        }
        false
    }

    pub fn possible_bipartition(n: i32, dislikes: Vec<Vec<i32>>) -> bool {
        let n = n as usize;
        let mut color = vec![0; n + 1];
        let mut g = vec![Vec::new(); n + 1];
        for d in dislikes.iter() {
            let (i, j) = (d[0] as usize, d[1] as usize);
            g[i].push(j);
            g[j].push(i);
        }
        for i in 1..=n {
            if color[i] == 0 && Self::dfs(i, 1, &mut color, &g) {
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

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách tô màu cần dùng recursion stack. Union-find biểu diễn quan hệ “phải ở cùng nhóm”: tất cả người mà $i$ không thích phải ở cùng một nhóm, và không ai trong số họ được ở cùng nhóm với $i$.
>
> Nếu $i$ đã được union với một người hàng xóm thì không thể chia hợp lệ; nếu chưa, gộp các người hàng xóm đó vào cùng một nhóm đại diện.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def possibleBipartition(self, n: int, dislikes: List[List[int]]) -> bool:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        g = defaultdict(list)
        for a, b in dislikes:
            a, b = a - 1, b - 1
            g[a].append(b)
            g[b].append(a)
        p = list(range(n))
        for i in range(n):
            for j in g[i]:
                if find(i) == find(j):
                    return False
                p[find(j)] = find(g[i][0])
        return True
```

#### Java

```java
class Solution {
    private int[] p;

    public boolean possibleBipartition(int n, int[][] dislikes) {
        p = new int[n];
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        for (var e : dislikes) {
            int a = e[0] - 1, b = e[1] - 1;
            g[a].add(b);
            g[b].add(a);
        }
        for (int i = 0; i < n; ++i) {
            for (int j : g[i]) {
                if (find(i) == find(j)) {
                    return false;
                }
                p[find(j)] = find(g[i].get(0));
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
    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        unordered_map<int, vector<int>> g;
        for (auto& e : dislikes) {
            int a = e[0] - 1, b = e[1] - 1;
            g[a].push_back(b);
            g[b].push_back(a);
        }
        function<int(int)> find = [&](int x) -> int {
            if (p[x] != x) p[x] = find(p[x]);
            return p[x];
        };
        for (int i = 0; i < n; ++i) {
            for (int j : g[i]) {
                if (find(i) == find(j)) return false;
                p[find(j)] = find(g[i][0]);
            }
        }
        return true;
    }
};
```

#### Go

```go
func possibleBipartition(n int, dislikes [][]int) bool {
	p := make([]int, n)
	g := make([][]int, n)
	for i := range p {
		p[i] = i
	}
	for _, e := range dislikes {
		a, b := e[0]-1, e[1]-1
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for i := 0; i < n; i++ {
		for _, j := range g[i] {
			if find(i) == find(j) {
				return false
			}
			p[find(j)] = find(g[i][0])
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
