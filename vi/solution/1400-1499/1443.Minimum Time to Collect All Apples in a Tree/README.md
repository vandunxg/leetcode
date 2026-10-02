---
comments: true
difficulty: Medium
rating: 1682
source: Weekly Contest 188 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Hash Table
    - Tree DP
---

<!-- problem:start -->

# [1443. Minimum Time to Collect All Apples in a Tree](https://leetcode.com/problems/minimum-time-to-collect-all-apples-in-a-tree)

[中文文档](/solution/1400-1499/1443.Minimum%20Time%20to%20Collect%20All%20Apples%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây vô hướng gồm <code>n</code> đỉnh được đánh số từ <code>0</code> đến <code>n-1</code>, trong đó một số đỉnh có táo. Bạn mất 1 giây để đi qua một cạnh của cây. <em>Hãy trả về thời gian tối thiểu tính bằng giây cần để thu thập tất cả táo trong cây, bắt đầu tại <strong>đỉnh 0</strong> và quay lại đỉnh này.</em></p>

<p>Các cạnh của cây vô hướng được cho trong mảng <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> nghĩa là có một cạnh nối đỉnh <code>a<sub>i</sub></code> và đỉnh <code>b<sub>i</sub></code>. Ngoài ra, có một mảng boolean <code>hasApple</code>, trong đó <code>hasApple[i] = true</code> nghĩa là đỉnh <code>i</code> có táo; nếu không thì đỉnh đó không có táo.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1443.Minimum%20Time%20to%20Collect%20All%20Apples%20in%20a%20Tree/images/min_time_collect_apple_1.png" style="width: 300px; height: 212px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,true,true,false]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Hình trên biểu diễn cây đã cho, trong đó các đỉnh màu đỏ có táo. Một đường đi tối ưu để thu thập tất cả táo được biểu diễn bằng các mũi tên màu xanh lá cây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1443.Minimum%20Time%20to%20Collect%20All%20Apples%20in%20a%20Tree/images/min_time_collect_apple_2.png" style="width: 300px; height: 212px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,false,true,false]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Hình trên biểu diễn cây đã cho, trong đó các đỉnh màu đỏ có táo. Một đường đi tối ưu để thu thập tất cả táo được biểu diễn bằng các mũi tên màu xanh lá cây.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,false,false,false,false,false]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub> &lt; b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>hasApple.length == n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta bắt đầu từ $0$, thu thập mọi quả táo rồi quay về, nên mỗi cạnh được sử dụng sẽ được đi qua hai lần. Với $n\le 10^5$, một lần DFS là đủ.
>
> Có thể bỏ qua một cây con không có táo và không cần thực hiện thêm công việc nào. Ta cộng chi phí của các con (đi vào một đỉnh con tốn `2`), rồi chỉ cộng thêm cạnh đi vào nếu đỉnh hiện tại có táo hoặc một đỉnh con đóng góp chi phí dương. Chi phí đi vào root là `0`.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minTime(self, n: int, edges: List[List[int]], hasApple: List[bool]) -> int:
        def dfs(u, cost):
            if vis[u]:
                return 0
            vis[u] = True
            nxt_cost = 0
            for v in g[u]:
                nxt_cost += dfs(v, 2)
            if not hasApple[u] and nxt_cost == 0:
                return 0
            return cost + nxt_cost

        g = defaultdict(list)
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)
        vis = [False] * n
        return dfs(0, 0)
```

#### Java

```java
class Solution {
    public int minTime(int n, int[][] edges, List<Boolean> hasApple) {
        boolean[] vis = new boolean[n];
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        return dfs(0, 0, g, hasApple, vis);
    }

    private int dfs(int u, int cost, List<Integer>[] g, List<Boolean> hasApple, boolean[] vis) {
        if (vis[u]) {
            return 0;
        }
        vis[u] = true;
        int nxtCost = 0;
        for (int v : g[u]) {
            nxtCost += dfs(v, 2, g, hasApple, vis);
        }
        if (!hasApple.get(u) && nxtCost == 0) {
            return 0;
        }
        return cost + nxtCost;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minTime(int n, vector<vector<int>>& edges, vector<bool>& hasApple) {
        vector<bool> vis(n);
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        return dfs(0, 0, g, hasApple, vis);
    }

    int dfs(int u, int cost, vector<vector<int>>& g, vector<bool>& hasApple, vector<bool>& vis) {
        if (vis[u]) return 0;
        vis[u] = true;
        int nxt = 0;
        for (int& v : g[u]) nxt += dfs(v, 2, g, hasApple, vis);
        if (!hasApple[u] && !nxt) return 0;
        return cost + nxt;
    }
};
```

#### Go

```go
func minTime(n int, edges [][]int, hasApple []bool) int {
	vis := make([]bool, n)
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	var dfs func(int, int) int
	dfs = func(u, cost int) int {
		if vis[u] {
			return 0
		}
		vis[u] = true
		nxt := 0
		for _, v := range g[u] {
			nxt += dfs(v, 2)
		}
		if !hasApple[u] && nxt == 0 {
			return 0
		}
		return cost + nxt
	}
	return dfs(0, 0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
