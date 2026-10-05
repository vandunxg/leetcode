---
comments: true
difficulty: Medium
tags:
    - Breadth-First Search
    - Graph
    - Binary Search
---

<!-- problem:start -->

# [3807. Minimum Cost to Repair Edges to Traverse a Graph 🔒](https://leetcode.com/problems/minimum-cost-to-repair-edges-to-traverse-a-graph)

[中文文档](/solution/3800-3899/3807.Minimum%20Cost%20to%20Repair%20Edges%20to%20Traverse%20a%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <strong>đồ thị vô hướng</strong> gồm <code>n</code> đỉnh được đánh số từ 0 đến <code>n - 1</code>. Đồ thị gồm <code>m</code> cạnh được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> cho biết có một cạnh nối đỉnh <code>u<sub>i</sub></code> và đỉnh <code>v<sub>i</sub></code>, với chi phí sửa chữa là <code>w<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một số nguyên <code>k</code>. Ban đầu, <strong>tất cả</strong> các cạnh đều bị hỏng.</p>

<p>Bạn có thể chọn một số nguyên không âm <code>money</code> và sửa chữa <strong>tất cả</strong> các cạnh có chi phí sửa chữa <strong>nhỏ hơn hoặc bằng</strong> <code>money</code>. Các cạnh còn lại vẫn bị hỏng và không thể sử dụng.</p>

<p>Bạn muốn đi từ đỉnh 0 đến đỉnh <code>n - 1</code> bằng nhiều nhất <code>k</code> cạnh.</p>

<p>Trả về một số nguyên biểu thị <strong>số tiền nhỏ nhất</strong> cần có để thực hiện được điều này, hoặc trả về -1 nếu không thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3807.Minimum%20Cost%20to%20Repair%20Edges%20to%20Traverse%20a%20Graph/images/ex1drawio.png" style="width: 211px; height: 171px;" /></strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,10],[1,2,10],[0,2,100]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">100</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đường đi hợp lệ duy nhất sử dụng nhiều nhất <code>k = 1</code> cạnh là <code>0 -&gt; 2</code>, cần sửa chữa cạnh có chi phí 100. Vì vậy, số tiền nhỏ nhất cần có là 100.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3807.Minimum%20Cost%20to%20Repair%20Edges%20to%20Traverse%20a%20Graph/images/ex2drawio.png" style="width: 361px; height: 251px;" /></strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6, edges = [[0,2,5],[2,3,6],[3,4,7],[4,5,5],[0,1,10],[1,5,12],[0,3,9],[1,2,8],[2,4,11]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>money = 12</code>, tất cả các cạnh có chi phí sửa chữa không vượt quá 12 đều có thể sử dụng.</li>
	<li>Điều này cho phép đi theo đường <code>0 -&gt; 1 -&gt; 5</code>, sử dụng đúng 2 cạnh và đến được đỉnh 5.</li>
	<li>Nếu <code>money &lt; 12</code>, không có đường đi khả dụng nào dài không quá <code>k = 2</code> từ đỉnh 0 đến đỉnh 5.</li>
	<li>Vì vậy, số tiền nhỏ nhất cần có là 12.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3800-3899/3807.Minimum%20Cost%20to%20Repair%20Edges%20to%20Traverse%20a%20Graph/images/ex3drawio.png" style="width: 312px; height: 52px;" />​​​​​​​</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,1]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể đi từ đỉnh 0 đến đỉnh 2 với bất kỳ số tiền nào. Vì vậy, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= edges.length == m &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
	<li>Đồ thị không có self-loop hoặc cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Search + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có thể sử dụng các cạnh có chi phí sửa chữa không vượt quá $\textit{money}$, và đường đi từ $0$ đến $(n-1)$ có thể sử dụng nhiều nhất $k$ cạnh. Vì $n$ và $m$ có thể lên tới $10^5$, ta không thể thử mọi mức chi phí.
>
> Ngân sách lớn hơn sẽ mở khóa thêm nhiều cạnh, nên tính khả thi có tính đơn điệu. Chi phí nhỏ nhất có thể là trọng số của một cạnh nào đó.
>
> Sắp xếp các cạnh theo trọng số và tìm kiếm nhị phân trên chỉ số: xây dựng đồ thị từ $\textit{mid}+1$ cạnh đầu tiên rồi dùng BFS để kiểm tra xem khoảng cách có không vượt quá $k$ hay không.
>
> Sau khi tìm kiếm xong, kiểm tra lại đầu mút trái; nếu không thỏa mãn thì trả về $-1$.

<!-- thinking:end -->

Ta nhận thấy rằng chi phí sửa chữa càng lớn thì càng có nhiều cạnh được sử dụng, từ đó càng dễ thỏa mãn yêu cầu đi từ đỉnh $0$ đến đỉnh $n - 1$ bằng nhiều nhất $k$ cạnh. Hơn nữa, chi phí sửa chữa nhỏ nhất phải là một trong các chi phí của các cạnh trong $\textit{edges}$. Vì vậy, trước tiên ta sắp xếp $\textit{edges}$ theo chi phí sửa chữa, sau đó dùng tìm kiếm nhị phân để tìm chi phí sửa chữa nhỏ nhất thỏa mãn yêu cầu.

Ta thực hiện tìm kiếm nhị phân trên chỉ số của chi phí sửa chữa, đặt biên trái là $l = 0$ và biên phải là $r = |\textit{edges}| - 1$. Với vị trí giữa $mid = \lfloor (l + r) / 2 \rfloor$, ta thêm vào đồ thị tất cả các cạnh có chi phí sửa chữa nhỏ hơn hoặc bằng $\textit{edges}[mid][2]$, sau đó dùng BFS để xác định xem có thể đi từ đỉnh $0$ đến đỉnh $n - 1$ bằng nhiều nhất $k$ cạnh hay không. Nếu có thể, ta cập nhật biên phải thành $r = mid$; nếu không, ta cập nhật biên trái thành $l = mid + 1$. Sau khi tìm kiếm nhị phân kết thúc, ta cần thực hiện thêm một lần BFS để kiểm tra xem $\textit{edges}[l][2]$ có thỏa mãn yêu cầu hay không. Nếu có, ta trả về $\textit{edges}[l][2]$; nếu không, ta trả về $-1$.

Độ phức tạp thời gian là $O((m + n) \times \log m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là số đỉnh và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, n: int, edges: List[List[int]], k: int) -> int:
        def check(idx: int) -> bool:
            g = [[] for _ in range(n)]
            for u, v, _ in edges[: idx + 1]:
                g[u].append(v)
                g[v].append(u)
            q = [0]
            dist = 0
            vis = [False] * n
            vis[0] = True
            while q:
                nq = []
                for u in q:
                    if u == n - 1:
                        return dist <= k
                    for v in g[u]:
                        if not vis[v]:
                            vis[v] = True
                            nq.append(v)
                q = nq
                dist += 1
            return False

        m = len(edges)
        edges.sort(key=lambda x: x[2])
        l, r = 0, m - 1
        while l < r:
            mid = (l + r) >> 1
            if check(mid):
                r = mid
            else:
                l = mid + 1
        return edges[l][2] if check(l) else -1
```

#### Java

```java
class Solution {
    private int n;
    private int[][] edges;
    private int k;

    public int minCost(int n, int[][] edges, int k) {
        this.n = n;
        this.edges = edges;
        this.k = k;
        Arrays.sort(edges, (a, b) -> a[2] - b[2]);
        int l = 0, r = edges.length - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return check(l) ? edges[l][2] : -1;
    }

    private boolean check(int idx) {
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int i = 0; i <= idx; ++i) {
            int u = edges[i][0], v = edges[i][1];
            g[u].add(v);
            g[v].add(u);
        }
        List<Integer> q = new ArrayList<>();
        q.add(0);
        int dist = 0;
        boolean[] vis = new boolean[n];
        vis[0] = true;
        while (!q.isEmpty()) {
            List<Integer> nq = new ArrayList<>();
            for (int u : q) {
                if (u == n - 1) {
                    return dist <= k;
                }
                for (int v : g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        nq.add(v);
                    }
                }
            }
            q = nq;
            ++dist;
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(int n, vector<vector<int>>& edges, int k) {
        sort(edges.begin(), edges.end(),
            [](const vector<int>& a, const vector<int>& b) {
                return a[2] < b[2];
            });

        auto check = [&](int idx) -> bool {
            vector<vector<int>> g(n);
            for (int i = 0; i <= idx; ++i) {
                int u = edges[i][0], v = edges[i][1];
                g[u].push_back(v);
                g[v].push_back(u);
            }

            vector<int> q;
            q.push_back(0);
            vector<char> vis(n, 0);
            vis[0] = 1;

            int dist = 0;
            while (!q.empty()) {
                vector<int> nq;
                for (int u : q) {
                    if (u == n - 1) {
                        return dist <= k;
                    }
                    for (int v : g[u]) {
                        if (!vis[v]) {
                            vis[v] = 1;
                            nq.push_back(v);
                        }
                    }
                }
                q.swap(nq);
                ++dist;
            }
            return false;
        };

        int m = edges.size();
        int l = 0, r = m - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return check(l) ? edges[l][2] : -1;
    }
};
```

#### Go

```go
func minCost(n int, edges [][]int, k int) int {
	sort.Slice(edges, func(i, j int) bool {
		return edges[i][2] < edges[j][2]
	})

	check := func(idx int) bool {
		g := make([][]int, n)
		for i := 0; i <= idx; i++ {
			u, v := edges[i][0], edges[i][1]
			g[u] = append(g[u], v)
			g[v] = append(g[v], u)
		}

		q := make([]int, 0, n)
		q = append(q, 0)
		vis := make([]bool, n)
		vis[0] = true

		dist := 0
		for len(q) > 0 {
			nq := make([]int, 0)
			for _, u := range q {
				if u == n-1 {
					return dist <= k
				}
				for _, v := range g[u] {
					if !vis[v] {
						vis[v] = true
						nq = append(nq, v)
					}
				}
			}
			q = nq
			dist++
		}
		return false
	}

	m := len(edges)
	l, r := 0, m-1
	for l < r {
		mid := (l + r) >> 1
		if check(mid) {
			r = mid
		} else {
			l = mid + 1
		}
	}
	if check(l) {
		return edges[l][2]
	}
	return -1
}
```

#### TypeScript

```ts
function minCost(n: number, edges: number[][], k: number): number {
    edges.sort((a, b) => a[2] - b[2]);

    const check = (idx: number): boolean => {
        const g: number[][] = Array.from({ length: n }, () => []);
        for (let i = 0; i <= idx; i++) {
            const [u, v] = edges[i];
            g[u].push(v);
            g[v].push(u);
        }

        let q: number[] = [0];
        const vis: boolean[] = Array(n).fill(false);
        vis[0] = true;

        let dist = 0;
        while (q.length > 0) {
            const nq: number[] = [];
            for (const u of q) {
                if (u === n - 1) {
                    return dist <= k;
                }
                for (const v of g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        nq.push(v);
                    }
                }
            }
            q = nq;
            dist++;
        }
        return false;
    };

    let [l, r] = [0, edges.length - 1];
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return check(l) ? edges[l][2] : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
