---
comments: true
difficulty: Medium
rating: 1908
source: Biweekly Contest 125 Q3
tags:
    - Tree
    - Depth-First Search
    - Array
---

<!-- problem:start -->

# [3067. Count Pairs of Connectable Servers in a Weighted Tree Network](https://leetcode.com/problems/count-pairs-of-connectable-servers-in-a-weighted-tree-network)

[中文文档](/solution/3000-3099/3067.Count%20Pairs%20of%20Connectable%20Servers%20in%20a%20Weighted%20Tree%20Network/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một cây có trọng số, không có gốc, gồm <code>n</code> đỉnh biểu diễn các server được đánh số từ <code>0</code> đến <code>n - 1</code>, một mảng <code>edges</code> trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>, weight<sub>i</sub>]</code> biểu diễn một cạnh hai chiều nối các đỉnh <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>, có trọng số <code>weight<sub>i</sub></code>. Bạn cũng được cung cấp một số nguyên <code>signalSpeed</code>.</p>

<p>Hai server <code>a</code> và <code>b</code> <strong>có thể kết nối</strong> thông qua server <code>c</code> nếu:</p>

<ul>
    <li><code>a &lt; b</code>, <code>a != c</code> và <code>b != c</code>.</li>
    <li>Khoảng cách từ <code>c</code> đến <code>a</code> chia hết cho <code>signalSpeed</code>.</li>
    <li>Khoảng cách từ <code>c</code> đến <code>b</code> chia hết cho <code>signalSpeed</code>.</li>
    <li>Đường đi từ <code>c</code> đến <code>b</code> và đường đi từ <code>c</code> đến <code>a</code> không có cạnh nào chung.</li>
</ul>

<p>Trả về <em>một mảng số nguyên</em> <code>count</code> <em>có độ dài</em> <code>n</code> <em>trong đó</em> <code>count[i]</code> <em>là <strong>số lượng</strong> cặp server <strong>có thể kết nối</strong> thông qua</em> <em>server</em> <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3067.Count%20Pairs%20of%20Connectable%20Servers%20in%20a%20Weighted%20Tree%20Network/images/example22.png" style="width: 438px; height: 243px; padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1,1],[1,2,5],[2,3,13],[3,4,9],[4,5,2]], signalSpeed = 1
<strong>Đầu ra:</strong> [0,4,6,6,4,0]
<strong>Giải thích:</strong> Vì signalSpeed bằng 1, count[c] bằng số cặp đường đi bắt đầu tại c và không có cạnh nào chung.
Trong đồ thị dạng đường đi đã cho, count[c] bằng số server ở bên trái c nhân với số server ở bên phải c.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3067.Count%20Pairs%20of%20Connectable%20Servers%20in%20a%20Weighted%20Tree%20Network/images/example11.png" style="width: 495px; height: 484px; padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,6,3],[6,5,3],[0,3,1],[3,2,7],[3,1,6],[3,4,2]], signalSpeed = 3
<strong>Đầu ra:</strong> [2,0,0,0,0,0,2]
<strong>Giải thích:</strong> Thông qua server 0, có 2 cặp server có thể kết nối: (4, 5) và (4, 6).
Thông qua server 6, có 2 cặp server có thể kết nối: (4, 5) và (0, 5).
Có thể chứng minh rằng không có cặp server nào có thể kết nối thông qua các server khác ngoài 0 và 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 1000</code></li>
    <li><code>edges.length == n - 1</code></li>
    <li><code>edges[i].length == 3</code></li>
    <li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
    <li><code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>, weight<sub>i</sub>]</code><!-- notionvc: a2623897-1bb1-4c07-84b6-917ffdcd83ec --></li>
    <li><code>1 &lt;= weight<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
    <li><code>1 &lt;= signalSpeed &lt;= 10<sup>6</sup></code></li>
    <li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp được tính tại server trung gian khi hai đường đi không chung cạnh và cả hai khoảng cách đều chia hết cho $\textit{signalSpeed}$. $n \le 1000$.
>
> Cố định server trung gian $a$, các node hợp lệ trong những cây con khác nhau có thể ghép cặp với nhau. Một DFS sẽ đếm các node có khoảng cách từ $a$ chia hết theo tốc độ tín hiệu.
>
> Với mỗi node con của $a$, DFS trả về $t$; ta cộng $s \cdot t$, sau đó cộng $t$ vào $s$.

<!-- thinking:end -->

Trước hết, ta xây dựng danh sách kề `g` dựa trên các cạnh được cung cấp trong đề bài, trong đó `g[a]` biểu diễn tất cả các node láng giềng của node `a` cùng trọng số cạnh tương ứng.

Sau đó, ta có thể liệt kê từng node `a` làm node trung gian kết nối, rồi dùng tìm kiếm theo chiều sâu để tính số node `t` bắt đầu từ node láng giềng `b` của `a` và có khoảng cách đến node `a` chia hết cho `signalSpeed`. Khi đó, số cặp node có thể kết nối của node `a` tăng thêm `s * t`, trong đó `s` là số lượng tích lũy các node bắt đầu từ node láng giềng `b` của `a` và có khoảng cách đến node `a` không chia hết cho `signalSpeed`. Sau đó, ta cập nhật `s` thành `s + t`.

Sau khi liệt kê tất cả các node `a`, ta có thể nhận được số cặp node có thể kết nối cho mọi node.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairsOfConnectableServers(
        self, edges: List[List[int]], signalSpeed: int
    ) -> List[int]:
        def dfs(a: int, fa: int, ws: int) -> int:
            cnt = 0 if ws % signalSpeed else 1
            for b, w in g[a]:
                if b != fa:
                    cnt += dfs(b, a, ws + w)
            return cnt

        n = len(edges) + 1
        g = [[] for _ in range(n)]
        for a, b, w in edges:
            g[a].append((b, w))
            g[b].append((a, w))
        ans = [0] * n
        for a in range(n):
            s = 0
            for b, w in g[a]:
                t = dfs(b, a, w)
                ans[a] += s * t
                s += t
        return ans
```

#### Java

```java
class Solution {
    private int signalSpeed;
    private List<int[]>[] g;

    public int[] countPairsOfConnectableServers(int[][] edges, int signalSpeed) {
        int n = edges.length + 1;
        g = new List[n];
        this.signalSpeed = signalSpeed;
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1], w = e[2];
            g[a].add(new int[] {b, w});
            g[b].add(new int[] {a, w});
        }
        int[] ans = new int[n];
        for (int a = 0; a < n; ++a) {
            int s = 0;
            for (var e : g[a]) {
                int b = e[0], w = e[1];
                int t = dfs(b, a, w);
                ans[a] += s * t;
                s += t;
            }
        }
        return ans;
    }

    private int dfs(int a, int fa, int ws) {
        int cnt = ws % signalSpeed == 0 ? 1 : 0;
        for (var e : g[a]) {
            int b = e[0], w = e[1];
            if (b != fa) {
                cnt += dfs(b, a, ws + w);
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countPairsOfConnectableServers(vector<vector<int>>& edges, int signalSpeed) {
        int n = edges.size() + 1;
        vector<pair<int, int>> g[n];
        for (auto& e : edges) {
            int a = e[0], b = e[1], w = e[2];
            g[a].emplace_back(b, w);
            g[b].emplace_back(a, w);
        }
        function<int(int, int, int)> dfs = [&](int a, int fa, int ws) {
            int cnt = ws % signalSpeed == 0;
            for (auto& [b, w] : g[a]) {
                if (b != fa) {
                    cnt += dfs(b, a, ws + w);
                }
            }
            return cnt;
        };
        vector<int> ans(n);
        for (int a = 0; a < n; ++a) {
            int s = 0;
            for (auto& [b, w] : g[a]) {
                int t = dfs(b, a, w);
                ans[a] += s * t;
                s += t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countPairsOfConnectableServers(edges [][]int, signalSpeed int) []int {
    n := len(edges) + 1
    type pair struct{ x, w int }
    g := make([][]pair, n)
    for _, e := range edges {
        a, b, w := e[0], e[1], e[2]
        g[a] = append(g[a], pair{b, w})
        g[b] = append(g[b], pair{a, w})
    }
    var dfs func(a, fa, ws int) int
    dfs = func(a, fa, ws int) int {
        cnt := 0
        if ws%signalSpeed == 0 {
            cnt++
        }
        for _, e := range g[a] {
            b, w := e.x, e.w
            if b != fa {
                cnt += dfs(b, a, ws+w)
            }
        }
        return cnt
    }
    ans := make([]int, n)
    for a := 0; a < n; a++ {
        s := 0
        for _, e := range g[a] {
            b, w := e.x, e.w
            t := dfs(b, a, w)
            ans[a] += s * t
            s += t
        }
    }
    return ans
}
```

#### TypeScript

```ts
function countPairsOfConnectableServers(edges: number[][], signalSpeed: number): number[] {
    const n = edges.length + 1;
    const g: [number, number][][] = Array.from({ length: n }, () => []);
    for (const [a, b, w] of edges) {
        g[a].push([b, w]);
        g[b].push([a, w]);
    }
    const dfs = (a: number, fa: number, ws: number): number => {
        let cnt = ws % signalSpeed === 0 ? 1 : 0;
        for (const [b, w] of g[a]) {
            if (b != fa) {
                cnt += dfs(b, a, ws + w);
            }
        }
        return cnt;
    };
    const ans: number[] = Array(n).fill(0);
    for (let a = 0; a < n; ++a) {
        let s = 0;
        for (const [b, w] of g[a]) {
            const t = dfs(b, a, w);
            ans[a] += s * t;
            s += t;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
