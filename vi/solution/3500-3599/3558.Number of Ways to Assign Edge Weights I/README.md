---
comments: true
difficulty: Medium
rating: 1845
source: Biweekly Contest 157 Q3
tags:
    - Tree
    - Depth-First Search
    - Math
---

<!-- problem:start -->

# [3558. Number of Ways to Assign Edge Weights I](https://leetcode.com/problems/number-of-ways-to-assign-edge-weights-i)

[中文文档](/solution/3500-3599/3558.Number%20of%20Ways%20to%20Assign%20Edge%20Weights%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây vô hướng gồm <code>n</code> node được đánh số từ 1 đến <code>n</code>, với node 1 là gốc. Cây được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh nối node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code>.</p>

<p>Ban đầu, tất cả các cạnh có trọng số bằng 0. Bạn phải gán cho mỗi cạnh một trọng số là <strong>1</strong> hoặc <strong>2</strong>.</p>

<p><strong>Chi phí</strong> của đường đi giữa hai node <code>u</code> và <code>v</code> là tổng trọng số của tất cả các cạnh trên đường đi nối chúng.</p>

<p>Chọn tùy ý một node <code>x</code> có độ sâu <strong>lớn nhất</strong>. Hãy trả về số cách gán trọng số cho các cạnh trên đường đi từ node 1 đến <code>x</code> sao cho tổng chi phí là <strong>lẻ</strong>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý:</strong> Bỏ qua tất cả các cạnh <strong>không</strong> nằm trên đường đi từ node 1 đến <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3558.Number%20of%20Ways%20to%20Assign%20Edge%20Weights%20I/images/screenshot-2025-03-24-at-060006.png" style="width: 200px; height: 72px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Đường đi từ Node 1 đến Node 2 gồm một cạnh (<code>1 &rarr; 2</code>).</li>
    <li>Gán trọng số 1 làm chi phí lẻ, còn 2 làm chi phí chẵn. Do đó, số cách gán hợp lệ là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3558.Number%20of%20Ways%20to%20Assign%20Edge%20Weights%20I/images/screenshot-2025-03-24-at-055820.png" style="width: 220px; height: 207px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[1,2],[1,3],[3,4],[3,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Độ sâu lớn nhất là 2, với node 4 và 5 có cùng độ sâu. Có thể chọn một trong hai node để xử lý.</li>
    <li>Ví dụ, đường đi từ Node 1 đến Node 4 gồm hai cạnh (<code>1 &rarr; 3</code> và <code>3 &rarr; 4</code>).</li>
    <li>Gán trọng số (1,2) hoặc (2,1) đều cho chi phí lẻ. Do đó, số cách gán hợp lệ là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>edges.length == n - 1</code></li>
    <li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code></li>
    <li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
    <li><code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Đường đi từ $1$ đến một lá sâu nhất có $d$ cạnh. Mỗi cạnh có trọng số $1$ hoặc $2$, và chi phí lẻ khi và chỉ khi có số lượng cạnh mang trọng số $1$ là lẻ. Các cạnh khác không bị giới hạn.
>
> Số tập con có kích thước lẻ của tập gồm $d$ phần tử là $2^{d-1}$ ($0$ khi $d=0$). DFS tìm $d$, rồi dùng lũy thừa nhanh để hoàn tất việc đếm.

<!-- thinking:end -->

Trước tiên, ta xây dựng một danh sách kề $g$ từ các cạnh, trong đó $g[u]$ chứa tất cả các node kề với node $u$.

Tiếp theo, ta dùng hàm $\textit{dfs}$ để tính độ sâu $d$ của cây. Đáp án là số cách chọn một số lượng phần tử lẻ từ $d$ phần tử. Theo một đồng nhất thức tổ hợp quen thuộc, số cách chọn một số lượng phần tử lẻ từ một tập có kích thước $d$ là $2^{d-1}$. Vì vậy, ta có thể tính đáp án bằng lũy thừa nhanh.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node trong cây.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def assignEdgeWeights(self, edges: List[List[int]]) -> int:
        def dfs(i: int, fa: int = 0) -> int:
            res = 0
            for j in g[i]:
                if j != fa:
                    res = max(res, dfs(j, i) + 1)
            return res

        n = len(edges) + 1
        g = [[] for _ in range(n + 1)]
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)
        d = dfs(1)
        return pow(2, d - 1, 10**9 + 7)
```

#### Java

```java
class Solution {
    private List<Integer>[] g;

    public int assignEdgeWeights(int[][] edges) {
        int n = edges.length + 1;
        g = new List[n + 1];
        Arrays.setAll(g, k -> new ArrayList<>());

        for (var e : edges) {
            int u = e[0];
            int v = e[1];
            g[u].add(v);
            g[v].add(u);
        }

        return (int) pow(2, dfs(1, 0) - 1, 1_000_000_007);
    }

    private int dfs(int i, int fa) {
        int res = 0;
        for (int j : g[i]) {
            if (j != fa) {
                res = Math.max(res, dfs(j, i) + 1);
            }
        }
        return res;
    }

    private long pow(long a, int n, int mod) {
        long res = 1;
        while (n > 0) {
            if ((n & 1) != 0) {
                res = res * a % mod;
            }
            a = a * a % mod;
            n >>= 1;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int assignEdgeWeights(vector<vector<int>>& edges) {
        int n = edges.size() + 1;
        vector<vector<int>> g(n + 1);

        for (auto& e : edges) {
            int u = e[0];
            int v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }

        auto dfs = [&](this auto&& dfs, int i, int fa) -> int {
            int res = 0;
            for (int j : g[i]) {
                if (j != fa) {
                    res = max(res, dfs(j, i) + 1);
                }
            }
            return res;
        };

        return pow(2, dfs(1, 0) - 1, 1000000007);
    }

private:
    long long pow(long long a, int n, int mod) {
        long long res = 1;
        while (n > 0) {
            if (n & 1) {
                res = res * a % mod;
            }
            a = a * a % mod;
            n >>= 1;
        }
        return res;
    }
};
```

#### Go

```go
func assignEdgeWeights(edges [][]int) int {
    const mod = 1_000_000_007

    n := len(edges) + 1
    g := make([][]int, n+1)

    for _, e := range edges {
        u, v := e[0], e[1]
        g[u] = append(g[u], v)
        g[v] = append(g[v], u)
    }

    var dfs func(int, int) int
    dfs = func(i, fa int) int {
        res := 0
        for _, j := range g[i] {
            if j != fa {
                res = max(res, dfs(j, i)+1)
            }
        }
        return res
    }

    return pow(2, dfs(1, 0)-1, mod)
}

func pow(a, n, mod int) int {
    res := 1
    for n > 0 {
        if n&1 > 0 {
            res = res * a % mod
        }
        a = a * a % mod
        n >>= 1
    }
    return res
}
```

#### TypeScript

```ts
function assignEdgeWeights(edges: number[][]): number {
    const mod = 1_000_000_007;
    const n = edges.length + 1;
    const g: number[][] = Array.from({ length: n + 1 }, () => []);

    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }

    const dfs = (i: number, fa: number): number => {
        let res = 0;
        for (const j of g[i]) {
            if (j !== fa) {
                res = Math.max(res, dfs(j, i) + 1);
            }
        }
        return res;
    };

    const pow = (a: number, n: number, mod: number): number => {
        let res = 1n;
        let x = BigInt(a);
        const m = BigInt(mod);

        while (n > 0) {
            if (n & 1) {
                res = (res * x) % m;
            }
            x = (x * x) % m;
            n >>= 1;
        }

        return Number(res);
    };

    return pow(2, dfs(1, 0) - 1, mod);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
