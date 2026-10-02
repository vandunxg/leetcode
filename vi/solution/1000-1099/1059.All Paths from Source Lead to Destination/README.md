---
comments: true
difficulty: Medium
tags:
    - Graph
    - Topological Sort
    - Kosaraju
    - Tarjan
---

<!-- problem:start -->

# [1059. All Paths from Source Lead to Destination 🔒](https://leetcode.com/problems/all-paths-from-source-lead-to-destination)

[中文文档](/solution/1000-1099/1059.All%20Paths%20from%20Source%20Lead%20to%20Destination/README.md)

## Mô tả

<!-- description:start -->

<p>Cho các cạnh <code>edges</code> của đồ thị có hướng, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị cạnh nối node <code>a<sub>i</sub></code> với node <code>b<sub>i</sub></code>, cùng hai node <code>source</code> và <code>destination</code>. Hãy xác định liệu mọi đường đi bắt đầu từ <code>source</code> cuối cùng có kết thúc tại <code>destination</code> hay không. Cụ thể:</p>

<ul>
	<li>Tồn tại ít nhất một đường đi từ node <code>source</code> đến node <code>destination</code>.</li>
	<li>Nếu có đường đi từ node <code>source</code> đến một node không có cạnh đi ra, thì node đó phải là <code>destination</code>.</li>
	<li>Số đường đi có thể có từ <code>source</code> đến <code>destination</code> là hữu hạn.</li>
</ul>

<p>Chỉ trả về <code>true</code> khi và chỉ khi mọi đường đi từ <code>source</code> đều dẫn đến <code>destination</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1059.All%20Paths%20from%20Source%20Lead%20to%20Destination/images/485_example_1.png" style="width: 200px; height: 208px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[0,1],[0,2]], source = 0, destination = 2
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có thể đi đến node 1 hoặc node 2 rồi bị mắc kẹt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1059.All%20Paths%20from%20Source%20Lead%20to%20Destination/images/485_example_2.png" style="width: 200px; height: 230px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[0,1],[0,3],[1,2],[2,1]], source = 0, destination = 3
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có hai khả năng: kết thúc tại node 3 hoặc lặp vô hạn giữa node 1 và node 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1059.All%20Paths%20from%20Source%20Lead%20to%20Destination/images/485_example_3.png" style="width: 200px; height: 183px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[0,1],[0,2],[1,3],[2,3]], source = 0, destination = 3
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= edges.length &lt;= 10<sup>4</sup></code></li>
	<li><code>edges.length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>0 &lt;= source &lt;= n - 1</code></li>
	<li><code>0 &lt;= destination &lt;= n - 1</code></li>
	<li>Đồ thị đã cho có thể chứa self-loop và các cạnh song song.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Mọi đường đi từ $source$ phải kết thúc tại $destination$ và không được lặp vô hạn ở nơi khác. Vì $n,m\le 10^4$, ta lưu trạng thái cho từng node để phân biệt “đang được duyệt” và “đã biết sẽ dẫn đến destination”.
>
> Nếu destination có cạnh đi ra thì không hợp lệ. Trong DFS, trạng thái $1$ biểu thị chu trình; node không có cạnh đi ra phải là destination. Với các node còn lại, đánh dấu trạng thái $1$, yêu cầu mọi node kề đều thỏa mãn, rồi đánh dấu trạng thái $2$.
>
> Kết quả của $\textit{dfs}(\textit{source})$ chính là đáp án.

<!-- thinking:end -->

Ta dùng mảng trạng thái $\textit{state}$ để lưu trạng thái của từng node, trong đó:

- Trạng thái 0 cho biết node chưa được duyệt;
- Trạng thái 1 cho biết node đang được duyệt;
- Trạng thái 2 cho biết node đã được duyệt và có thể dẫn đến destination.

Trước tiên, ta biểu diễn đồ thị bằng danh sách kề, rồi thực hiện depth-first search (DFS) bắt đầu từ node source. Trong quá trình DFS:

- Nếu trạng thái của node hiện tại là 1, nghĩa là ta đã gặp chu trình và trả về $\text{false}$ ngay;
- Nếu trạng thái của node hiện tại là 2, nghĩa là node đã được duyệt và có thể dẫn đến destination, nên trả về $\text{true}$ ngay;
- Nếu node hiện tại không có cạnh đi ra, kiểm tra xem nó có phải destination hay không. Nếu đúng, trả về $\text{true}$; nếu không, trả về $\text{false}$;
- Nếu không, đặt trạng thái của node hiện tại thành 1 rồi đệ quy duyệt tất cả node kề;
- Nếu tất cả node kề đều có thể dẫn đến destination, đặt trạng thái của node hiện tại thành 2 và trả về $\text{true}$; nếu không, trả về $\text{false}$.

Đáp án là kết quả của $\text{dfs}(\text{source})$.

Độ phức tạp thời gian là $O(n + m)$, với $n$ và $m$ lần lượt là số node và số cạnh. Độ phức tạp không gian là $O(n + m)$ để lưu danh sách kề của đồ thị và mảng trạng thái.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def leadsToDestination(
        self, n: int, edges: List[List[int]], source: int, destination: int
    ) -> bool:
        def dfs(i: int) -> bool:
            if st[i]:
                return st[i] == 2
            if not g[i]:
                return i == destination

            st[i] = 1
            for j in g[i]:
                if not dfs(j):
                    return False
            st[i] = 2
            return True

        g = [[] for _ in range(n)]
        for a, b in edges:
            g[a].append(b)
        if g[destination]:
            return False

        st = [0] * n
        return dfs(source)
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int[] st;
    private int destination;

    public boolean leadsToDestination(int n, int[][] edges, int source, int destination) {
        this.destination = destination;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            g[e[0]].add(e[1]);
        }
        if (!g[destination].isEmpty()) {
            return false;
        }
        st = new int[n];
        return dfs(source);
    }

    private boolean dfs(int i) {
        if (st[i] != 0) {
            return st[i] == 2;
        }
        if (g[i].isEmpty()) {
            return i == destination;
        }
        st[i] = 1;
        for (int j : g[i]) {
            if (!dfs(j)) {
                return false;
            }
        }
        st[i] = 2;
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> g;
    vector<int> st;
    int destination;

    bool leadsToDestination(int n, vector<vector<int>>& edges, int source, int destination) {
        this->destination = destination;
        g.assign(n, {});
        for (auto& e : edges) {
            g[e[0]].push_back(e[1]);
        }
        if (!g[destination].empty()) {
            return false;
        }
        st.assign(n, 0);
        return dfs(source);
    }

    bool dfs(int i) {
        if (st[i] != 0) {
            return st[i] == 2;
        }
        if (g[i].empty()) {
            return i == destination;
        }
        st[i] = 1;
        for (int j : g[i]) {
            if (!dfs(j)) {
                return false;
            }
        }
        st[i] = 2;
        return true;
    }
};
```

#### Go

```go
func leadsToDestination(n int, edges [][]int, source int, destination int) bool {
	g := make([][]int, n)
	for _, e := range edges {
		g[e[0]] = append(g[e[0]], e[1])
	}
	if len(g[destination]) > 0 {
		return false
	}

	st := make([]int, n)

	var dfs func(i int) bool
	dfs = func(i int) bool {
		if st[i] != 0 {
			return st[i] == 2
		}
		if len(g[i]) == 0 {
			return i == destination
		}
		st[i] = 1
		for _, j := range g[i] {
			if !dfs(j) {
				return false
			}
		}
		st[i] = 2
		return true
	}

	return dfs(source)
}
```

#### TypeScript

```ts
function leadsToDestination(
    n: number,
    edges: number[][],
    source: number,
    destination: number,
): boolean {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
    }
    if (g[destination].length > 0) {
        return false;
    }

    const st: number[] = Array(n).fill(0);

    const dfs = (i: number): boolean => {
        if (st[i] !== 0) {
            return st[i] === 2;
        }
        if (g[i].length === 0) {
            return i === destination;
        }
        st[i] = 1;
        for (const j of g[i]) {
            if (!dfs(j)) {
                return false;
            }
        }
        st[i] = 2;
        return true;
    };

    return dfs(source);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
