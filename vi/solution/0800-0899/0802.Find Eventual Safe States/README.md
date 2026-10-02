---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Kosaraju
    - Tarjan
---

<!-- problem:start -->

# [802. Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states)

[中文文档](/solution/0800-0899/0802.Find%20Eventual%20Safe%20States/README.md)

## Mô tả

<!-- description:start -->

<p>Cho đồ thị có hướng gồm <code>n</code> node, mỗi node được đánh số từ <code>0</code> đến <code>n - 1</code>. Đồ thị được biểu diễn bằng mảng số nguyên 2 chiều <strong>đánh số từ 0</strong> <code>graph</code>, trong đó <code>graph[i]</code> là mảng các node kề với node <code>i</code>, nghĩa là có cạnh từ node <code>i</code> đến từng node trong <code>graph[i]</code>.</p>

<p>Node được gọi là <strong>node kết thúc</strong> nếu không có cạnh đi ra. Node được gọi là <strong>node an toàn</strong> nếu mọi đường đi có thể bắt đầu từ node đó đều dẫn đến một <strong>node kết thúc</strong> (hoặc một node an toàn khác).</p>

<p>Hãy trả về <em>mảng chứa tất cả <strong>node an toàn</strong> của đồ thị</em>. Kết quả phải được sắp xếp theo thứ tự <strong>tăng dần</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="Illustration of graph" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0802.Find%20Eventual%20Safe%20States/images/picture1.png" style="height: 171px; width: 600px;" />
<pre>
<strong>Đầu vào:</strong> graph = [[1,2],[2,3],[5],[0],[5],[],[]]
<strong>Đầu ra:</strong> [2,4,5,6]
<strong>Giải thích:</strong> Đồ thị đã cho được minh họa ở trên.
Node 5 và 6 là node kết thúc vì không có cạnh nào đi ra từ chúng.
Mọi đường đi bắt đầu từ node 2, 4, 5 hoặc 6 đều dẫn đến node 5 hoặc node 6.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
<strong>Đầu ra:</strong> [4]
<strong>Giải thích:</strong>
Chỉ node 4 là node kết thúc, và mọi đường đi bắt đầu từ node 4 đều dẫn đến node 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == graph.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= graph[i].length &lt;= n</code></li>
	<li><code>0 &lt;= graph[i][j] &lt;= n - 1</code></li>
	<li><code>graph[i]</code> được sắp xếp theo thứ tự tăng nghiêm ngặt.</li>
	<li>Đồ thị có thể chứa self-loop.</li>
	<li>Số cạnh trong đồ thị nằm trong khoảng <code>[1, 4 * 10<sup>4</sup>]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một node cuối cùng sẽ an toàn nếu mọi đường đi từ nó đều kết thúc tại node kết thúc và không đi vào chu trình. Tìm kiếm riêng từ từng node sẽ lặp lại việc duyệt các chuỗi node giống nhau.
>
> Đảo chiều các cạnh để out-degree ban đầu trở thành in-degree. Thuật toán Kahn lần lượt xóa các node có in-degree bằng $0$; các node bị xóa chính xác là những node không thể đi đến chu trình, tức các node an toàn. Một lượt duyệt tuyến tính là đủ với $n\le 10^4$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def eventualSafeNodes(self, graph: List[List[int]]) -> List[int]:
        rg = defaultdict(list)
        indeg = [0] * len(graph)
        for i, vs in enumerate(graph):
            for j in vs:
                rg[j].append(i)
            indeg[i] = len(vs)
        q = deque([i for i, v in enumerate(indeg) if v == 0])
        while q:
            i = q.popleft()
            for j in rg[i]:
                indeg[j] -= 1
                if indeg[j] == 0:
                    q.append(j)
        return [i for i, v in enumerate(indeg) if v == 0]
```

#### Java

```java
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        int[] indeg = new int[n];
        List<Integer>[] rg = new List[n];
        Arrays.setAll(rg, k -> new ArrayList<>());
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            for (int j : graph[i]) {
                rg[j].add(i);
            }
            indeg[i] = graph[i].length;
            if (indeg[i] == 0) {
                q.offer(i);
            }
        }
        while (!q.isEmpty()) {
            int i = q.pollFirst();
            for (int j : rg[i]) {
                if (--indeg[j] == 0) {
                    q.offer(j);
                }
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                ans.add(i);
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
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> indeg(n);
        vector<vector<int>> rg(n);
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            for (int j : graph[i]) rg[j].push_back(i);
            indeg[i] = graph[i].size();
            if (indeg[i] == 0) q.push(i);
        }
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            for (int j : rg[i])
                if (--indeg[j] == 0) q.push(j);
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i)
            if (indeg[i] == 0) ans.push_back(i);
        return ans;
    }
};
```

#### Go

```go
func eventualSafeNodes(graph [][]int) []int {
	n := len(graph)
	indeg := make([]int, n)
	rg := make([][]int, n)
	q := []int{}
	for i, vs := range graph {
		for _, j := range vs {
			rg[j] = append(rg[j], i)
		}
		indeg[i] = len(vs)
		if indeg[i] == 0 {
			q = append(q, i)
		}
	}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		for _, j := range rg[i] {
			indeg[j]--
			if indeg[j] == 0 {
				q = append(q, j)
			}
		}
	}
	ans := []int{}
	for i, v := range indeg {
		if v == 0 {
			ans = append(ans, i)
		}
	}
	return ans
}
```

#### JavaScript

```js
/**
 * @param {number[][]} graph
 * @return {number[]}
 */
var eventualSafeNodes = function (graph) {
    const n = graph.length;
    const rg = new Array(n).fill(0).map(() => new Array());
    const indeg = new Array(n).fill(0);
    const q = [];
    for (let i = 0; i < n; ++i) {
        for (let j of graph[i]) {
            rg[j].push(i);
        }
        indeg[i] = graph[i].length;
        if (indeg[i] == 0) {
            q.push(i);
        }
    }
    while (q.length) {
        const i = q.shift();
        for (let j of rg[i]) {
            if (--indeg[j] == 0) {
                q.push(j);
            }
        }
    }
    let ans = [];
    for (let i = 0; i < n; ++i) {
        if (indeg[i] == 0) {
            ans.push(i);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách tiếp cận topo dựng đồ thị đảo và tính in-degree. Có thể dùng DFS ba màu trên đồ thị gốc: màu xám nghĩa là đường đi hiện tại gặp chu trình; màu đen sau khi đệ quy kết thúc nghĩa là mọi đường đi từ node đó đều an toàn.
>
> Cách này không cần lưu đồ thị đảo. Đáp án gồm các node mà DFS trả về true, với độ phức tạp tuyến tính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def eventualSafeNodes(self, graph: List[List[int]]) -> List[int]:
        def dfs(i):
            if color[i]:
                return color[i] == 2
            color[i] = 1
            for j in graph[i]:
                if not dfs(j):
                    return False
            color[i] = 2
            return True

        n = len(graph)
        color = [0] * n
        return [i for i in range(n) if dfs(i)]
```

#### Java

```java
class Solution {
    private int[] color;
    private int[][] g;

    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        color = new int[n];
        g = graph;
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            if (dfs(i)) {
                ans.add(i);
            }
        }
        return ans;
    }

    private boolean dfs(int i) {
        if (color[i] > 0) {
            return color[i] == 2;
        }
        color[i] = 1;
        for (int j : g[i]) {
            if (!dfs(j)) {
                return false;
            }
        }
        color[i] = 2;
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> color;

    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n = graph.size();
        color.assign(n, 0);
        vector<int> ans;
        for (int i = 0; i < n; ++i)
            if (dfs(i, graph)) ans.push_back(i);
        return ans;
    }

    bool dfs(int i, vector<vector<int>>& g) {
        if (color[i]) return color[i] == 2;
        color[i] = 1;
        for (int j : g[i])
            if (!dfs(j, g)) return false;
        color[i] = 2;
        return true;
    }
};
```

#### Go

```go
func eventualSafeNodes(graph [][]int) []int {
	n := len(graph)
	color := make([]int, n)
	var dfs func(int) bool
	dfs = func(i int) bool {
		if color[i] > 0 {
			return color[i] == 2
		}
		color[i] = 1
		for _, j := range graph[i] {
			if !dfs(j) {
				return false
			}
		}
		color[i] = 2
		return true
	}
	ans := []int{}
	for i := range graph {
		if dfs(i) {
			ans = append(ans, i)
		}
	}
	return ans
}
```

#### JavaScript

```js
/**
 * @param {number[][]} graph
 * @return {number[]}
 */
var eventualSafeNodes = function (graph) {
    const n = graph.length;
    const color = new Array(n).fill(0);
    function dfs(i) {
        if (color[i]) {
            return color[i] == 2;
        }
        color[i] = 1;
        for (const j of graph[i]) {
            if (!dfs(j)) {
                return false;
            }
        }
        color[i] = 2;
        return true;
    }
    let ans = [];
    for (let i = 0; i < n; ++i) {
        if (dfs(i)) {
            ans.push(i);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
