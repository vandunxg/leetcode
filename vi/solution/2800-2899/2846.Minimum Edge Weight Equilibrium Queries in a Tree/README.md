---
comments: true
difficulty: Hard
rating: 2507
source: Weekly Contest 361 Q4
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2846. Minimum Edge Weight Equilibrium Queries in a Tree](https://leetcode.com/problems/minimum-edge-weight-equilibrium-queries-in-a-tree)

[中文文档](/solution/2800-2899/2846.Minimum%20Edge%20Weight%20Equilibrium%20Queries%20in%20a%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho số nguyên <code>n</code> và một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> cho biết có một cạnh nối node <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> với trọng số <code>w<sub>i</sub></code> trong cây.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code> có độ dài <code>m</code>, trong đó <code>queries[i] = [a<sub>i</sub>, b<sub>i</sub>]</code>. Với mỗi truy vấn, hãy tìm <strong>số thao tác nhỏ nhất</strong> cần thực hiện để trọng số của mọi cạnh trên đường đi từ <code>a<sub>i</sub></code> đến <code>b<sub>i</sub></code> bằng nhau. Trong một thao tác, bạn có thể chọn bất kỳ cạnh nào trong cây và đổi trọng số của nó thành một giá trị bất kỳ.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li>Các truy vấn là <strong>độc lập</strong> với nhau, nghĩa là cây trở về <strong>trạng thái ban đầu</strong> trước mỗi truy vấn mới.</li>
	<li>Đường đi từ <code>a<sub>i</sub></code> đến <code>b<sub>i</sub></code> là một dãy các node <strong>khác nhau</strong>, bắt đầu từ node <code>a<sub>i</sub></code> và kết thúc tại node <code>b<sub>i</sub></code>, sao cho mọi cặp node liền kề trong dãy đều được nối với nhau bởi một cạnh trong cây.</li>
</ul>

<p>Trả về <em>một mảng</em> <code>answer</code> <em>có độ dài</em> <code>m</code>, <em>trong đó</em> <code>answer[i]</code> <em>là đáp án của truy vấn thứ</em> <code>i<sup>th</sup></code> <em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2846.Minimum%20Edge%20Weight%20Equilibrium%20Queries%20in%20a%20Tree/images/graph-6-1.png" style="width: 339px; height: 344px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[0,1,1],[1,2,1],[2,3,1],[3,4,2],[4,5,2],[5,6,2]], queries = [[0,3],[3,6],[2,6],[0,6]]
<strong>Đầu ra:</strong> [0,0,1,3]
<strong>Giải thích:</strong> Trong truy vấn đầu tiên, tất cả các cạnh trên đường đi từ 0 đến 3 đều có trọng số 1. Do đó, đáp án là 0.
Trong truy vấn thứ hai, tất cả các cạnh trên đường đi từ 3 đến 6 đều có trọng số 2. Do đó, đáp án là 0.
Trong truy vấn thứ ba, ta đổi trọng số của cạnh [2,3] thành 2. Sau thao tác này, tất cả các cạnh trên đường đi từ 2 đến 6 đều có trọng số 2. Do đó, đáp án là 1.
Trong truy vấn thứ tư, ta đổi trọng số của các cạnh [0,1], [1,2] và [2,3] thành 2. Sau các thao tác này, tất cả các cạnh trên đường đi từ 0 đến 6 đều có trọng số 2. Do đó, đáp án là 3.
Với mỗi queries[i], có thể chứng minh rằng answer[i] là số thao tác nhỏ nhất cần thực hiện để các trọng số cạnh trên đường đi từ a<sub>i</sub> đến b<sub>i</sub> bằng nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2800-2899/2846.Minimum%20Edge%20Weight%20Equilibrium%20Queries%20in%20a%20Tree/images/graph-9-1.png" style="width: 472px; height: 370px;" />
<pre>
<strong>Đầu vào:</strong> n = 8, edges = [[1,2,6],[1,3,4],[2,4,6],[2,5,3],[3,6,6],[3,0,8],[7,0,2]], queries = [[4,6],[0,4],[6,5],[7,4]]
<strong>Đầu ra:</strong> [1,2,2,3]
<strong>Giải thích:</strong> Trong truy vấn đầu tiên, ta đổi trọng số của cạnh [1,3] thành 6. Sau thao tác này, tất cả các cạnh trên đường đi từ 4 đến 6 đều có trọng số 6. Do đó, đáp án là 1.
Trong truy vấn thứ hai, ta đổi trọng số của các cạnh [0,3] và [3,1] thành 6. Sau các thao tác này, tất cả các cạnh trên đường đi từ 0 đến 4 đều có trọng số 6. Do đó, đáp án là 2.
Trong truy vấn thứ ba, ta đổi trọng số của các cạnh [1,3] và [5,2] thành 6. Sau các thao tác này, tất cả các cạnh trên đường đi từ 6 đến 5 đều có trọng số 6. Do đó, đáp án là 2.
Trong truy vấn thứ tư, ta đổi trọng số của các cạnh [0,7], [0,3] và [1,3] thành 6. Sau các thao tác này, tất cả các cạnh trên đường đi từ 7 đến 4 đều có trọng số 6. Do đó, đáp án là 3.
Với mỗi queries[i], có thể chứng minh rằng answer[i] là số thao tác nhỏ nhất cần thực hiện để các trọng số cạnh trên đường đi từ a<sub>i</sub> đến b<sub>i</sub> bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 26</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
	<li><code>1 &lt;= queries.length == m &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Lifting để tìm LCA

<!-- thinking:start -->

> **Tư duy**
>
> Số thao tác để cân bằng trọng số trên một đường đi bằng độ dài đường đi trừ đi số lần xuất hiện của trọng số phổ biến nhất. Binary lifting lưu LCA và số lần xuất hiện của từng trọng số trên đường đi từ gốc đến mỗi node; số lần xuất hiện trên đường đi được tính từ hai đầu mút trừ đi phần của LCA, nên mỗi truy vấn được xử lý trong thời gian logarithm.

<!-- thinking:end -->

Bài toán yêu cầu số thao tác nhỏ nhất để đưa tất cả trọng số cạnh về cùng một giá trị trên đường đi giữa hai điểm bất kỳ. Về bản chất, đó là độ dài đường đi giữa hai điểm trừ đi số lần xuất hiện của cạnh có trọng số phổ biến nhất trên đường đi.

Ta có thể tìm độ dài đường đi giữa hai điểm bằng cách tìm LCA (Lowest Common Ancestor) sử dụng binary lifting. Gọi hai điểm là $u$ và $v$, LCA của chúng là $x$. Khi đó, độ dài đường đi từ $u$ đến $v$ là $depth(u) + depth(v) - 2 \times depth(x)$.

Ngoài ra, ta có thể dùng mảng $cnt[n][26]$ để ghi lại số lần xuất hiện của mỗi trọng số cạnh từ node gốc đến mỗi node. Khi đó, số lần xuất hiện của cạnh có trọng số phổ biến nhất trên đường đi từ $u$ đến $v$ là $\max_{0 \leq j < 26} cnt[u][j] + cnt[v][j] - 2 \times cnt[x][j]$, trong đó $x$ là LCA của $u$ và $v$.

Quy trình tìm LCA bằng binary lifting như sau:

Gọi độ sâu của mỗi node là $depth$, node cha của nó là $p$, và $f[i][j]$ là tổ tiên thứ $2^j$ của node $i$. Với hai điểm bất kỳ $x$ và $y$, ta có thể tìm LCA của chúng như sau:

1. Nếu $depth(x) < depth(y)$, đổi chỗ $x$ và $y$, tức là đảm bảo độ sâu của $x$ không nhỏ hơn độ sâu của $y$;
2. Tiếp theo, liên tục đưa $x$ lên trên cho đến khi độ sâu của $x$ và $y$ bằng nhau, khi đó độ sâu của $x$ và $y$ đều là $depth(x)$;
3. Sau đó, đồng thời đưa $x$ và $y$ lên trên cho đến khi cha của $x$ và $y$ giống nhau, khi đó cha của $x$ và $y$ đều là $f[x][0]$, chính là LCA của $x$ và $y$.

Cuối cùng, số thao tác nhỏ nhất từ node $u$ đến node $v$ là $depth(u) + depth(v) - 2 \times depth(x) - \max_{0 \leq j < 26} cnt[u][j] + cnt[v][j] - 2 \times cnt[x][j]$.

Độ phức tạp thời gian là $O((n + q) \times C \times \log n)$ và độ phức tạp không gian là $O(n \times C \times \log n)$. Trong đó, $C$ là giá trị lớn nhất của trọng số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperationsQueries(
        self, n: int, edges: List[List[int]], queries: List[List[int]]
    ) -> List[int]:
        m = n.bit_length()
        g = [[] for _ in range(n)]
        f = [[0] * m for _ in range(n)]
        p = [0] * n
        cnt = [None] * n
        depth = [0] * n
        for u, v, w in edges:
            g[u].append((v, w - 1))
            g[v].append((u, w - 1))
        cnt[0] = [0] * 26
        q = deque([0])
        while q:
            i = q.popleft()
            f[i][0] = p[i]
            for j in range(1, m):
                f[i][j] = f[f[i][j - 1]][j - 1]
            for j, w in g[i]:
                if j != p[i]:
                    p[j] = i
                    cnt[j] = cnt[i][:]
                    cnt[j][w] += 1
                    depth[j] = depth[i] + 1
                    q.append(j)
        ans = []
        for u, v in queries:
            x, y = u, v
            if depth[x] < depth[y]:
                x, y = y, x
            for j in reversed(range(m)):
                if depth[x] - depth[y] >= (1 << j):
                    x = f[x][j]
            for j in reversed(range(m)):
                if f[x][j] != f[y][j]:
                    x, y = f[x][j], f[y][j]
            if x != y:
                x = p[x]
            mx = max(cnt[u][j] + cnt[v][j] - 2 * cnt[x][j] for j in range(26))
            ans.append(depth[u] + depth[v] - 2 * depth[x] - mx)
        return ans
```

#### Java

```java
class Solution {
    public int[] minOperationsQueries(int n, int[][] edges, int[][] queries) {
        int m = 32 - Integer.numberOfLeadingZeros(n);
        List<int[]>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        int[][] f = new int[n][m];
        int[] p = new int[n];
        int[][] cnt = new int[n][0];
        int[] depth = new int[n];
        for (var e : edges) {
            int u = e[0], v = e[1], w = e[2] - 1;
            g[u].add(new int[] {v, w});
            g[v].add(new int[] {u, w});
        }
        cnt[0] = new int[26];
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        while (!q.isEmpty()) {
            int i = q.poll();
            f[i][0] = p[i];
            for (int j = 1; j < m; ++j) {
                f[i][j] = f[f[i][j - 1]][j - 1];
            }
            for (var nxt : g[i]) {
                int j = nxt[0], w = nxt[1];
                if (j != p[i]) {
                    p[j] = i;
                    cnt[j] = cnt[i].clone();
                    cnt[j][w]++;
                    depth[j] = depth[i] + 1;
                    q.offer(j);
                }
            }
        }
        int k = queries.length;
        int[] ans = new int[k];
        for (int i = 0; i < k; ++i) {
            int u = queries[i][0], v = queries[i][1];
            int x = u, y = v;
            if (depth[x] < depth[y]) {
                int t = x;
                x = y;
                y = t;
            }
            for (int j = m - 1; j >= 0; --j) {
                if (depth[x] - depth[y] >= (1 << j)) {
                    x = f[x][j];
                }
            }
            for (int j = m - 1; j >= 0; --j) {
                if (f[x][j] != f[y][j]) {
                    x = f[x][j];
                    y = f[y][j];
                }
            }
            if (x != y) {
                x = p[x];
            }
            int mx = 0;
            for (int j = 0; j < 26; ++j) {
                mx = Math.max(mx, cnt[u][j] + cnt[v][j] - 2 * cnt[x][j]);
            }
            ans[i] = depth[u] + depth[v] - 2 * depth[x] - mx;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minOperationsQueries(int n, vector<vector<int>>& edges, vector<vector<int>>& queries) {
        int m = 32 - __builtin_clz(n);
        vector<pair<int, int>> g[n];
        int f[n][m];
        int p[n];
        int cnt[n][26];
        int depth[n];
        memset(f, 0, sizeof(f));
        memset(cnt, 0, sizeof(cnt));
        memset(depth, 0, sizeof(depth));
        memset(p, 0, sizeof(p));
        for (auto& e : edges) {
            int u = e[0], v = e[1], w = e[2] - 1;
            g[u].emplace_back(v, w);
            g[v].emplace_back(u, w);
        }
        queue<int> q;
        q.push(0);
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            f[i][0] = p[i];
            for (int j = 1; j < m; ++j) {
                f[i][j] = f[f[i][j - 1]][j - 1];
            }
            for (auto& [j, w] : g[i]) {
                if (j != p[i]) {
                    p[j] = i;
                    memcpy(cnt[j], cnt[i], sizeof(cnt[i]));
                    cnt[j][w]++;
                    depth[j] = depth[i] + 1;
                    q.push(j);
                }
            }
        }
        vector<int> ans;
        for (auto& qq : queries) {
            int u = qq[0], v = qq[1];
            int x = u, y = v;
            if (depth[x] < depth[y]) {
                swap(x, y);
            }
            for (int j = m - 1; ~j; --j) {
                if (depth[x] - depth[y] >= (1 << j)) {
                    x = f[x][j];
                }
            }
            for (int j = m - 1; ~j; --j) {
                if (f[x][j] != f[y][j]) {
                    x = f[x][j];
                    y = f[y][j];
                }
            }
            if (x != y) {
                x = p[x];
            }
            int mx = 0;
            for (int j = 0; j < 26; ++j) {
                mx = max(mx, cnt[u][j] + cnt[v][j] - 2 * cnt[x][j]);
            }
            ans.push_back(depth[u] + depth[v] - 2 * depth[x] - mx);
        }
        return ans;
    }
};
```

#### Go

```go
func minOperationsQueries(n int, edges [][]int, queries [][]int) []int {
	m := bits.Len(uint(n))
	g := make([][][2]int, n)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, m)
	}
	p := make([]int, n)
	cnt := make([][26]int, n)
	cnt[0] = [26]int{}
	depth := make([]int, n)
	for _, e := range edges {
		u, v, w := e[0], e[1], e[2]-1
		g[u] = append(g[u], [2]int{v, w})
		g[v] = append(g[v], [2]int{u, w})
	}
	q := []int{0}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		f[i][0] = p[i]
		for j := 1; j < m; j++ {
			f[i][j] = f[f[i][j-1]][j-1]
		}
		for _, nxt := range g[i] {
			j, w := nxt[0], nxt[1]
			if j != p[i] {
				p[j] = i
				cnt[j] = [26]int{}
				for k := 0; k < 26; k++ {
					cnt[j][k] = cnt[i][k]
				}
				cnt[j][w]++
				depth[j] = depth[i] + 1
				q = append(q, j)
			}
		}
	}
	ans := make([]int, len(queries))
	for i, qq := range queries {
		u, v := qq[0], qq[1]
		x, y := u, v
		if depth[x] < depth[y] {
			x, y = y, x
		}
		for j := m - 1; j >= 0; j-- {
			if depth[x]-depth[y] >= (1 << j) {
				x = f[x][j]
			}
		}
		for j := m - 1; j >= 0; j-- {
			if f[x][j] != f[y][j] {
				x, y = f[x][j], f[y][j]
			}
		}
		if x != y {
			x = p[x]
		}
		mx := 0
		for j := 0; j < 26; j++ {
			mx = max(mx, cnt[u][j]+cnt[v][j]-2*cnt[x][j])
		}
		ans[i] = depth[u] + depth[v] - 2*depth[x] - mx
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
