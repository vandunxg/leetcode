---
comments: true
difficulty: Medium
rating: 1725
source: Weekly Contest 486 Q3
tags:
    - Tree
    - Breadth-First Search
---

<!-- problem:start -->

# [3820. Pythagorean Distance Nodes in a Tree](https://leetcode.com/problems/pythagorean-distance-nodes-in-a-tree)

[中文文档](/solution/3800-3899/3820.Pythagorean%20Distance%20Nodes%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> và một cây vô hướng gồm <code>n</code> đỉnh, được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh vô hướng nối <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>.</p>

<p>Bạn cũng được cho ba đỉnh đích <strong>phân biệt</strong> <code>x</code>, <code>y</code> và <code>z</code>.</p>

<p>Với mọi đỉnh <code>u</code> trong cây:</p>

<ul>
	<li>Gọi <code>dx</code> là khoảng cách từ <code>u</code> đến đỉnh <code>x</code></li>
	<li>Gọi <code>dy</code> là khoảng cách từ <code>u</code> đến đỉnh <code>y</code></li>
	<li>Gọi <code>dz</code> là khoảng cách từ <code>u</code> đến đỉnh <code>z</code></li>
</ul>

<p>Đỉnh <code>u</code> được gọi là <strong>đặc biệt</strong> nếu ba khoảng cách này tạo thành một <strong>bộ ba Pythagore</strong>.</p>

<p>Trả về một số nguyên biểu thị số lượng đỉnh đặc biệt trong cây.</p>

<p>Một <strong>bộ ba Pythagore</strong> gồm ba số nguyên <code>a</code>, <code>b</code> và <code>c</code> sao cho khi được sắp xếp theo thứ tự <strong>tăng dần</strong>, chúng thỏa mãn <code>a<sup>2</sup> + b<sup>2</sup> = c<sup>2</sup></code>.</p>

<p><strong>Khoảng cách</strong> giữa hai đỉnh trong cây là số cạnh trên đường đi duy nhất giữa chúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1],[0,2],[0,3]], x = 1, y = 2, z = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mỗi đỉnh, ta tính khoảng cách từ đỉnh đó đến các đỉnh <code>x = 1</code>, <code>y = 2</code> và <code>z = 3</code>.</p>

<ul>
	<li>Đỉnh 0 có các khoảng cách là 1, 1 và 1. Sau khi sắp xếp, các khoảng cách là 1, 1 và 1, không thỏa mãn điều kiện Pythagore.</li>
	<li>Đỉnh 1 có các khoảng cách là 0, 2 và 2. Sau khi sắp xếp, các khoảng cách là 0, 2 và 2. Vì <code>0<sup>2</sup> + 2<sup>2</sup> = 2<sup>2</sup></code>, đỉnh 1 là đỉnh đặc biệt.</li>
	<li>Đỉnh 2 có các khoảng cách là 2, 0 và 2. Sau khi sắp xếp, các khoảng cách là 0, 2 và 2. Vì <code>0<sup>2</sup> + 2<sup>2</sup> = 2<sup>2</sup></code>, đỉnh 2 là đỉnh đặc biệt.</li>
	<li>Đỉnh 3 có các khoảng cách là 2, 2 và 0. Sau khi sắp xếp, các khoảng cách là 0, 2 và 2. Trường hợp này cũng thỏa mãn điều kiện Pythagore.</li>
</ul>

<p>Do đó, các đỉnh 1, 2 và 3 là đỉnh đặc biệt, và đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1],[1,2],[2,3]], x = 0, y = 3, z = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mỗi đỉnh, ta tính khoảng cách từ đỉnh đó đến các đỉnh <code>x = 0</code>, <code>y = 3</code> và <code>z = 2</code>.</p>

<ul>
	<li>Đỉnh 0 có các khoảng cách là 0, 3 và 2. Sau khi sắp xếp, các khoảng cách là 0, 2 và 3, không thỏa mãn điều kiện Pythagore.</li>
	<li>Đỉnh 1 có các khoảng cách là 1, 2 và 1. Sau khi sắp xếp, các khoảng cách là 1, 1 và 2, không thỏa mãn điều kiện Pythagore.</li>
	<li>Đỉnh 2 có các khoảng cách là 2, 1 và 0. Sau khi sắp xếp, các khoảng cách là 0, 1 và 2, không thỏa mãn điều kiện Pythagore.</li>
	<li>Đỉnh 3 có các khoảng cách là 3, 0 và 1. Sau khi sắp xếp, các khoảng cách là 0, 1 và 3, không thỏa mãn điều kiện Pythagore.</li>
</ul>

<p>Không có đỉnh nào thỏa mãn điều kiện Pythagore. Do đó, đáp án là 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1],[1,2],[1,3]], x = 1, y = 3, z = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mỗi đỉnh, ta tính khoảng cách từ đỉnh đó đến các đỉnh <code>x = 1</code>, <code>y = 3</code> và <code>z = 0</code>.</p>

<ul>
	<li>Đỉnh 0 có các khoảng cách là 1, 2 và 0. Sau khi sắp xếp, các khoảng cách là 0, 1 và 2, không thỏa mãn điều kiện Pythagore.</li>
	<li>Đỉnh 1 có các khoảng cách là 0, 1 và 1. Sau khi sắp xếp, các khoảng cách là 0, 1 và 1. Vì <code>0<sup>2</sup> + 1<sup>2</sup> = 1<sup>2</sup></code>, đỉnh 1 là đỉnh đặc biệt.</li>
	<li>Đỉnh 2 có các khoảng cách là 1, 2 và 2. Sau khi sắp xếp, các khoảng cách là 1, 2 và 2, không thỏa mãn điều kiện Pythagore.</li>
	<li>Đỉnh 3 có các khoảng cách là 1, 0 và 2. Sau khi sắp xếp, các khoảng cách là 0, 1 và 2, không thỏa mãn điều kiện Pythagore.</li>
</ul>

<p>Do đó, đáp án là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>4 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub>, x, y, z &lt;= n - 1</code></li>
	<li><code>x</code>, <code>y</code> và <code>z</code> đôi một <strong>phân biệt</strong>.</li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi $u$, ta cần kiểm tra xem khoảng cách đến $x,y,z$ có tạo thành một bộ ba Pythagore hay không. Với $n \le 10^5$, không thể tìm kiếm từ từng $u$.
>
> Khoảng cách trên cây từ một nguồn cố định có thể được tính bằng một lần BFS. Chỉ cần ba nguồn $x,y,z$.
>
> Chạy BFS từ mỗi nguồn, sau đó tại mỗi đỉnh sắp xếp ba khoảng cách và kiểm tra $a^2+b^2=c^2$.
>
> Ba lần duyệt tuyến tính cùng một lần kiểm tra $O(n)$ là đủ.

<!-- thinking:end -->

Trước tiên, chúng ta xây dựng một adjacency list $g$ dựa trên các cạnh được cho trong đề bài, trong đó $g[u]$ lưu tất cả các đỉnh kề với đỉnh $u$.

Tiếp theo, chúng ta định nghĩa hàm $\text{bfs}(i)$ để tính khoảng cách từ đỉnh $i$ đến tất cả các đỉnh khác. Chúng ta sử dụng một queue để triển khai Breadth-First Search (BFS) và duy trì một mảng khoảng cách $\text{dist}$, trong đó $\text{dist}[j]$ biểu thị khoảng cách từ đỉnh $i$ đến đỉnh $j$. Ban đầu, $\text{dist}[i] = 0$, còn khoảng cách đến tất cả các đỉnh khác được đặt là vô cực. Trong quá trình BFS, chúng ta liên tục cập nhật mảng khoảng cách cho đến khi duyệt qua tất cả các đỉnh có thể đi tới.

Chúng ta gọi $\text{bfs}(x)$, $\text{bfs}(y)$ và $\text{bfs}(z)$ để tính khoảng cách từ các đỉnh $x$, $y$ và $z$ đến tất cả các đỉnh khác, thu được lần lượt ba mảng khoảng cách $d_1$, $d_2$ và $d_3$.

Cuối cùng, chúng ta duyệt qua tất cả các đỉnh $u$. Với mỗi đỉnh, ta lấy khoảng cách từ nó đến $x$, $y$ và $z$ lần lượt là $a = d_1[u]$, $b = d_2[u]$ và $c = d_3[u]$. Ta sắp xếp ba khoảng cách này rồi kiểm tra xem chúng có thỏa mãn định lý Pythagore hay không: $a^2 + b^2 = c^2$. Nếu thỏa mãn, ta tăng số lượng đáp án lên một.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số đỉnh của cây.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def specialNodes(
        self, n: int, edges: List[List[int]], x: int, y: int, z: int
    ) -> int:
        g = [[] for _ in range(n)]
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)

        def bfs(i: int) -> List[int]:
            q = deque([i])
            dist = [inf] * n
            dist[i] = 0
            while q:
                for _ in range(len(q)):
                    u = q.popleft()
                    for v in g[u]:
                        if dist[v] > dist[u] + 1:
                            dist[v] = dist[u] + 1
                            q.append(v)
            return dist

        d1 = bfs(x)
        d2 = bfs(y)
        d3 = bfs(z)
        ans = 0
        for a, b, c in zip(d1, d2, d3):
            s = a + b + c
            a, c = min(a, b, c), max(a, b, c)
            b = s - a - c
            if a * a + b * b == c * c:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int n;
    private final int inf = Integer.MAX_VALUE / 2;

    public int specialNodes(int n, int[][] edges, int x, int y, int z) {
        this.n = n;
        g = new ArrayList[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }

        int[] d1 = bfs(x);
        int[] d2 = bfs(y);
        int[] d3 = bfs(z);

        int ans = 0;
        for (int i = 0; i < n; i++) {
            long[] a = new long[] {d1[i], d2[i], d3[i]};
            Arrays.sort(a);
            if (a[0] * a[0] + a[1] * a[1] == a[2] * a[2]) {
                ++ans;
            }
        }
        return ans;
    }

    private int[] bfs(int i) {
        int[] dist = new int[n];
        Arrays.fill(dist, inf);
        Deque<Integer> q = new ArrayDeque<>();
        dist[i] = 0;
        q.add(i);
        while (!q.isEmpty()) {
            for (int k = q.size(); k > 0; --k) {
                int u = q.poll();
                for (int v : g[u]) {
                    if (dist[v] > dist[u] + 1) {
                        dist[v] = dist[u] + 1;
                        q.add(v);
                    }
                }
            }
        }
        return dist;
    }
}
```

#### C++

```cpp
class Solution {
private:
    vector<vector<int>> g;
    int n;
    const int inf = INT_MAX / 2;

    vector<int> bfs(int i) {
        vector<int> dist(n, inf);
        queue<int> q;
        dist[i] = 0;
        q.push(i);
        while (!q.empty()) {
            for (int k = q.size(); k > 0; --k) {
                int u = q.front();
                q.pop();
                for (int v : g[u]) {
                    if (dist[v] > dist[u] + 1) {
                        dist[v] = dist[u] + 1;
                        q.push(v);
                    }
                }
            }
        }
        return dist;
    }

public:
    int specialNodes(int n, vector<vector<int>>& edges, int x, int y, int z) {
        this->n = n;
        g.assign(n, {});
        for (auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }

        vector<int> d1 = bfs(x);
        vector<int> d2 = bfs(y);
        vector<int> d3 = bfs(z);

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            array<long long, 3> a = {
                (long long) d1[i],
                (long long) d2[i],
                (long long) d3[i]};
            sort(a.begin(), a.end());
            if (a[0] * a[0] + a[1] * a[1] == a[2] * a[2]) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func specialNodes(n int, edges [][]int, x int, y int, z int) int {
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}

	const inf = int(1e9)

	bfs := func(i int) []int {
		dist := make([]int, n)
		for k := 0; k < n; k++ {
			dist[k] = inf
		}
		q := make([]int, 0)
		dist[i] = 0
		q = append(q, i)
		for len(q) > 0 {
			sz := len(q)
			for ; sz > 0; sz-- {
				u := q[0]
				q = q[1:]
				for _, v := range g[u] {
					if dist[v] > dist[u]+1 {
						dist[v] = dist[u] + 1
						q = append(q, v)
					}
				}
			}
		}
		return dist
	}

	d1 := bfs(x)
	d2 := bfs(y)
	d3 := bfs(z)

	ans := 0
	for i := 0; i < n; i++ {
		a := []int{d1[i], d2[i], d3[i]}
		sort.Ints(a)
		x0, x1, x2 := int64(a[0]), int64(a[1]), int64(a[2])
		if x0*x0+x1*x1 == x2*x2 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function specialNodes(n: number, edges: number[][], x: number, y: number, z: number): number {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }

    const inf = 1e9;

    const bfs = (i: number): number[] => {
        const dist = Array(n).fill(inf);
        let q: number[] = [i];
        dist[i] = 0;
        while (q.length) {
            const nq = [];
            for (const u of q) {
                for (const v of g[u]) {
                    if (dist[v] > dist[u] + 1) {
                        dist[v] = dist[u] + 1;
                        nq.push(v);
                    }
                }
            }
            q = nq;
        }
        return dist;
    };

    const d1 = bfs(x);
    const d2 = bfs(y);
    const d3 = bfs(z);

    let ans = 0;
    for (let i = 0; i < n; i++) {
        const a = [d1[i], d2[i], d3[i]];
        a.sort((p, q) => p - q);
        if (a[0] * a[0] + a[1] * a[1] === a[2] * a[2]) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
