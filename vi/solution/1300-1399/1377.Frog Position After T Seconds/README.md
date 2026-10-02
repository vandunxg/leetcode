---
comments: true
difficulty: Hard
rating: 1823
source: Weekly Contest 179 Q4
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [1377. Frog Position After T Seconds](https://leetcode.com/problems/frog-position-after-t-seconds)

[中文文档](/solution/1300-1399/1377.Frog%20Position%20After%20T%20Seconds/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây vô hướng gồm <code>n</code> đỉnh được đánh số từ <code>1</code> đến <code>n</code>. Một con ếch bắt đầu nhảy từ <strong>đỉnh 1</strong>. Mỗi giây, nếu đỉnh hiện tại có cạnh nối trực tiếp đến một đỉnh <strong>chưa được thăm</strong>, ếch sẽ nhảy đến đỉnh đó. Ếch không thể quay lại đỉnh đã thăm. Nếu có nhiều đỉnh có thể nhảy đến, ếch chọn ngẫu nhiên một đỉnh với xác suất như nhau. Nếu không còn đỉnh chưa thăm nào để nhảy đến, ếch sẽ ở nguyên tại đỉnh đó mãi mãi.</p>

<p>Các cạnh của cây vô hướng được cho trong mảng <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> nghĩa là có một cạnh nối hai đỉnh <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>.</p>

<p><em>Trả về xác suất để sau <code>t</code> giây, ếch đang ở đỉnh <code>target</code>. </em>Các đáp án sai lệch không quá <code>10<sup>-5</sup></code> so với đáp án thực tế sẽ được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1377.Frog%20Position%20After%20T%20Seconds/images/frog1.jpg" style="width: 338px; height: 304px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[1,2],[1,3],[1,7],[2,4],[2,6],[3,5]], t = 2, target = 4
<strong>Đầu ra:</strong> 0.16666666666666666 
<strong>Giải thích:</strong> Hình trên minh họa đồ thị đã cho. Ếch bắt đầu ở đỉnh 1, có xác suất 1/3 nhảy đến đỉnh 2 sau <strong>giây thứ 1</strong>, rồi có xác suất 1/2 nhảy đến đỉnh 4 sau <strong>giây thứ 2</strong>. Vì vậy, xác suất để ếch ở đỉnh 4 sau 2 giây là 1/3 * 1/2 = 1/6 = 0.16666666666666666. 
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1377.Frog%20Position%20After%20T%20Seconds/images/frog2.jpg" style="width: 304px; height: 304px;" /></strong>

<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[1,2],[1,3],[1,7],[2,4],[2,6],[3,5]], t = 1, target = 7
<strong>Đầu ra:</strong> 0.3333333333333333
<strong>Giải thích: </strong>Hình trên minh họa đồ thị đã cho. Ếch bắt đầu ở đỉnh 1 và có xác suất 1/3 = 0.3333333333333333 nhảy đến đỉnh 7 sau <strong>giây thứ 1</strong>. 
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n</code></li>
	<li><code>1 &lt;= t &lt;= 50</code></li>
	<li><code>1 &lt;= target &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Ếch nhảy đến một đỉnh kề chưa thăm với xác suất như nhau; ta cần tìm xác suất ếch ở $\textit{target}$ sau $t$ giây. Trong cây chỉ có một đường đi giữa hai đỉnh, nên có thể dùng BFS theo từng giây để truyền xác suất. Nếu node lấy ra khỏi queue là đích, ta trả về xác suất tại đó khi không còn đỉnh kề chưa thăm hoặc đã hết thời gian; nếu không, ếch buộc phải rời đi và đáp án là $0$. Với các node khác, chia đều xác suất cho những đỉnh kề chưa thăm.

<!-- thinking:end -->

Trước tiên, dựa trên các cạnh của cây vô hướng trong đề bài, ta tạo adjacency list $g$, trong đó $g[u]$ chứa tất cả các đỉnh kề với đỉnh $u$.

Sau đó, ta định nghĩa các cấu trúc dữ liệu sau:

- Queue $q$ dùng để lưu các đỉnh và xác suất tương ứng trong mỗi lượt tìm kiếm. Ban đầu, $q = [(1, 1.0)]$, nghĩa là xác suất ếch ở đỉnh $1$ là $1.0$;
- Mảng $vis$ dùng để ghi nhận mỗi đỉnh đã được thăm hay chưa. Ban đầu, $vis[1] = true$, các phần tử còn lại là $false$.

Tiếp theo, ta bắt đầu BFS.

Trong mỗi lượt tìm kiếm, ta lấy phần tử đầu queue $(u, p)$, trong đó $u$ là đỉnh hiện tại và $p$ là xác suất tương ứng. Gọi $cnt$ là số đỉnh kề chưa được thăm của đỉnh $u$.

- Nếu $u = target$, nghĩa là ếch đã đến đỉnh đích. Khi đó, ta kiểm tra ếch đến đích đúng sau $t$ giây, hoặc đến sớm hơn nhưng không thể nhảy sang đỉnh khác (tức là $t=0$ hoặc $cnt=0$). Nếu đúng, trả về $p$; nếu không, trả về $0$.
- Nếu $u \neq target$, ta chia đều xác suất $p$ cho tất cả đỉnh kề chưa thăm của $u$, thêm các đỉnh đó vào queue $q$ và đánh dấu chúng đã được thăm.

Kết thúc mỗi lượt tìm kiếm, ta giảm $t$ đi $1$ rồi tiếp tục lượt tiếp theo cho đến khi queue rỗng hoặc $t \lt 0$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def frogPosition(
        self, n: int, edges: List[List[int]], t: int, target: int
    ) -> float:
        g = defaultdict(list)
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)
        q = deque([(1, 1.0)])
        vis = [False] * (n + 1)
        vis[1] = True
        while q and t >= 0:
            for _ in range(len(q)):
                u, p = q.popleft()
                cnt = len(g[u]) - int(u != 1)
                if u == target:
                    return p if cnt * t == 0 else 0
                for v in g[u]:
                    if not vis[v]:
                        vis[v] = True
                        q.append((v, p / cnt))
            t -= 1
        return 0
```

#### Java

```java
class Solution {
    public double frogPosition(int n, int[][] edges, int t, int target) {
        List<Integer>[] g = new List[n + 1];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        Deque<Pair<Integer, Double>> q = new ArrayDeque<>();
        q.offer(new Pair<>(1, 1.0));
        boolean[] vis = new boolean[n + 1];
        vis[1] = true;
        for (; !q.isEmpty() && t >= 0; --t) {
            for (int k = q.size(); k > 0; --k) {
                var x = q.poll();
                int u = x.getKey();
                double p = x.getValue();
                int cnt = g[u].size() - (u == 1 ? 0 : 1);
                if (u == target) {
                    return cnt * t == 0 ? p : 0;
                }
                for (int v : g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        q.offer(new Pair<>(v, p / cnt));
                    }
                }
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double frogPosition(int n, vector<vector<int>>& edges, int t, int target) {
        vector<vector<int>> g(n + 1);
        for (auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        queue<pair<int, double>> q{{{1, 1.0}}};
        bool vis[n + 1];
        memset(vis, false, sizeof(vis));
        vis[1] = true;
        for (; q.size() && t >= 0; --t) {
            for (int k = q.size(); k; --k) {
                auto [u, p] = q.front();
                q.pop();
                int cnt = g[u].size() - (u != 1);
                if (u == target) {
                    return cnt * t == 0 ? p : 0;
                }
                for (int v : g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        q.push({v, p / cnt});
                    }
                }
            }
        }
        return 0;
    }
};
```

#### Go

```go
func frogPosition(n int, edges [][]int, t int, target int) float64 {
	g := make([][]int, n+1)
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	type pair struct {
		u int
		p float64
	}
	q := []pair{{1, 1}}
	vis := make([]bool, n+1)
	vis[1] = true
	for ; len(q) > 0 && t >= 0; t-- {
		for k := len(q); k > 0; k-- {
			u, p := q[0].u, q[0].p
			q = q[1:]
			cnt := len(g[u])
			if u != 1 {
				cnt--
			}
			if u == target {
				if cnt*t == 0 {
					return p
				}
				return 0
			}
			for _, v := range g[u] {
				if !vis[v] {
					vis[v] = true
					q = append(q, pair{v, p / float64(cnt)})
				}
			}
		}
	}
	return 0
}
```

#### TypeScript

```ts
function frogPosition(n: number, edges: number[][], t: number, target: number): number {
    const g: number[][] = Array.from({ length: n + 1 }, () => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }
    const q: number[][] = [[1, 1]];
    const vis: boolean[] = Array.from({ length: n + 1 }, () => false);
    vis[1] = true;
    for (; q.length > 0 && t >= 0; --t) {
        for (let k = q.length; k > 0; --k) {
            const [u, p] = q.shift()!;
            const cnt = g[u].length - (u === 1 ? 0 : 1);
            if (u === target) {
                return cnt * t === 0 ? p : 0;
            }
            for (const v of g[u]) {
                if (!vis[v]) {
                    vis[v] = true;
                    q.push([v, p / cnt]);
                }
            }
        }
    }
    return 0;
}
```

#### C#

```cs
public class Solution {
    public double FrogPosition(int n, int[][] edges, int t, int target) {
        List<int>[] g = new List<int>[n + 1];
        for (int i = 0; i < n + 1; i++) {
            g[i] = new List<int>();
        }
        foreach (int[] e in edges) {
            int u = e[0], v = e[1];
            g[u].Add(v);
            g[v].Add(u);
        }
        Queue<Tuple<int, double>> q = new Queue<Tuple<int, double>>();
        q.Enqueue(new Tuple<int, double>(1, 1.0));
        bool[] vis = new bool[n + 1];
        vis[1] = true;
        for (; q.Count > 0 && t >= 0; --t) {
            for (int k = q.Count; k > 0; --k) {
                (var u, var p) = q.Dequeue();
                int cnt = g[u].Count - (u == 1 ? 0 : 1);
                if (u == target) {
                    return cnt * t == 0 ? p : 0;
                }
                foreach (int v in g[u]) {
                    if (!vis[v]) {
                        vis[v] = true;
                        q.Enqueue(new Tuple<int, double>(v, p / cnt));
                    }
                }
            }
        }
        return 0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
