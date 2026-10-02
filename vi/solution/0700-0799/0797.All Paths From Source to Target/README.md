---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Backtracking
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [797. All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target)

[中文文档](/solution/0700-0799/0797.All%20Paths%20From%20Source%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đồ thị có hướng không chu trình (<strong>DAG</strong>) gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Hãy tìm mọi đường đi có thể từ node <code>0</code> đến node <code>n - 1</code> và trả về theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Đồ thị được cho như sau: <code>graph[i]</code> là danh sách tất cả node có thể đi đến từ node <code>i</code> (tức là có cạnh có hướng từ node <code>i</code> đến node <code>graph[i][j]</code>).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0797.All%20Paths%20From%20Source%20to%20Target/images/all_1.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> graph = [[1,2],[3],[3],[]]
<strong>Đầu ra:</strong> [[0,1,3],[0,2,3]]
<strong>Giải thích:</strong> Có hai đường đi: 0 -&gt; 1 -&gt; 3 và 0 -&gt; 2 -&gt; 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0797.All%20Paths%20From%20Source%20to%20Target/images/all_2.jpg" style="width: 423px; height: 301px;" />
<pre>
<strong>Đầu vào:</strong> graph = [[4,3,1],[3,2,4],[3],[4],[]]
<strong>Đầu ra:</strong> [[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == graph.length</code></li>
	<li><code>2 &lt;= n &lt;= 15</code></li>
	<li><code>0 &lt;= graph[i][j] &lt; n</code></li>
	<li><code>graph[i][j] != i</code> (tức là không có self-loop).</li>
	<li>Tất cả phần tử trong <code>graph[i]</code> đều <strong>khác nhau</strong>.</li>
	<li>Đồ thị đầu vào được <strong>đảm bảo</strong> là một <strong>DAG</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê mọi đường đi từ $0$ đến $n-1$ trong DAG. $n\le 15$; cần xuất ra tất cả các đường đi.
>
> BFS lưu toàn bộ đường đi; ghi nhận đường đi khi đến đích, nếu chưa thì thêm từng node kề vào đường đi và đưa đường đi mới vào queue.

<!-- thinking:end -->

Bắt đầu từ node $0$, lưu các đường đi trong queue và ghi nhận đường đi khi đến node $n-1$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        n = len(graph)
        q = deque([[0]])
        ans = []
        while q:
            path = q.popleft()
            u = path[-1]
            if u == n - 1:
                ans.append(path)
                continue
            for v in graph[u]:
                q.append(path + [v])
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        int n = graph.length;
        Queue<List<Integer>> queue = new ArrayDeque<>();
        queue.offer(Arrays.asList(0));
        List<List<Integer>> ans = new ArrayList<>();
        while (!queue.isEmpty()) {
            List<Integer> path = queue.poll();
            int u = path.get(path.size() - 1);
            if (u == n - 1) {
                ans.add(path);
                continue;
            }
            for (int v : graph[u]) {
                List<Integer> next = new ArrayList<>(path);
                next.add(v);
                queue.offer(next);
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
    vector<vector<int>> graph;
    vector<vector<int>> ans;

    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        this->graph = graph;
        vector<int> path;
        path.push_back(0);
        dfs(0, path);
        return ans;
    }

    void dfs(int i, vector<int> path) {
        if (i == graph.size() - 1) {
            ans.push_back(path);
            return;
        }
        for (int j : graph[i]) {
            path.push_back(j);
            dfs(j, path);
            path.pop_back();
        }
    }
};
```

#### Go

```go
func allPathsSourceTarget(graph [][]int) [][]int {
	var path []int
	path = append(path, 0)
	var ans [][]int

	var dfs func(i int)
	dfs = func(i int) {
		if i == len(graph)-1 {
			ans = append(ans, append([]int(nil), path...))
			return
		}
		for _, j := range graph[i] {
			path = append(path, j)
			dfs(j)
			path = path[:len(path)-1]
		}
	}

	dfs(0)
	return ans
}
```

#### Rust

```rust
impl Solution {
    fn dfs(i: usize, path: &mut Vec<i32>, res: &mut Vec<Vec<i32>>, graph: &Vec<Vec<i32>>) {
        path.push(i as i32);
        if i == graph.len() - 1 {
            res.push(path.clone());
        }
        for j in graph[i].iter() {
            Self::dfs(*j as usize, path, res, graph);
        }
        path.pop();
    }

    pub fn all_paths_source_target(graph: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let mut res = Vec::new();
        Self::dfs(0, &mut vec![], &mut res, &graph);
        res
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} graph
 * @return {number[][]}
 */
var allPathsSourceTarget = function (graph) {
    const ans = [];
    const t = [0];

    const dfs = t => {
        const cur = t[t.length - 1];
        if (cur == graph.length - 1) {
            ans.push([...t]);
            return;
        }
        for (const v of graph[cur]) {
            t.push(v);
            dfs(t);
            t.pop();
        }
    };

    dfs(t);
    return ans;
};
```

#### TypeScript

```ts
function allPathsSourceTarget(graph: number[][]): number[][] {
    const ans: number[][] = [];

    const dfs = (path: number[]) => {
        const curr = path.at(-1)!;
        if (curr === graph.length - 1) {
            ans.push([...path]);
            return;
        }

        for (const v of graph[curr]) {
            path.push(v);
            dfs(path);
            path.pop();
        }
    };

    dfs([0]);

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: DFS

<!-- thinking:start -->

> **Tư duy**
>
> BFS sao chép mọi tiền tố đường đi. DFS thêm node vào cùng một mảng, lưu bản sao khi đến node đích rồi pop node đó ra, nhờ vậy tạo ít danh sách trung gian hơn.

<!-- thinking:end -->

Chạy DFS từ node $0$ và backtrack sau mỗi đường đi đến đích.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        def dfs(t):
            if t[-1] == len(graph) - 1:
                ans.append(t[:])
                return
            for v in graph[t[-1]]:
                t.append(v)
                dfs(t)
                t.pop()

        ans = []
        dfs([0])
        return ans
```

#### Java

```java
class Solution {
    private List<List<Integer>> ans;
    private int[][] graph;

    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        ans = new ArrayList<>();
        this.graph = graph;
        List<Integer> t = new ArrayList<>();
        t.add(0);
        dfs(t);
        return ans;
    }

    private void dfs(List<Integer> t) {
        int cur = t.get(t.size() - 1);
        if (cur == graph.length - 1) {
            ans.add(new ArrayList<>(t));
            return;
        }
        for (int v : graph[cur]) {
            t.add(v);
            dfs(t);
            t.remove(t.size() - 1);
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
