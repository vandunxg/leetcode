---
comments: true
difficulty: Hard
rating: 2161
source: Weekly Contest 426 Q4
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
---

<!-- problem:start -->

# [3373. Maximize the Number of Target Nodes After Connecting Trees II](https://leetcode.com/problems/maximize-the-number-of-target-nodes-after-connecting-trees-ii)

[中文文档](/solution/3300-3399/3373.Maximize%20the%20Number%20of%20Target%20Nodes%20After%20Connecting%20Trees%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có hai cây <strong>vô hướng</strong> gồm lần lượt <code>n</code> và <code>m</code> node, được đánh số từ <code>[0, n - 1]</code> và <code>[0, m - 1]</code>.</p>

<p>Cho hai mảng số nguyên 2 chiều <code>edges1</code> và <code>edges2</code> có độ dài lần lượt là <code>n - 1</code> và <code>m - 1</code>, trong đó <code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một cạnh nối node <code>a<sub>i</sub></code> và node <code>b<sub>i</sub></code> trong cây thứ nhất, còn <code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị có một cạnh nối node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code> trong cây thứ hai.</p>

<p>Node <code>u</code> là <strong>target</strong> đối với node <code>v</code> nếu số cạnh trên đường đi từ <code>u</code> đến <code>v</code> là số chẵn.&nbsp;<strong>Lưu ý</strong> rằng một node <em>luôn</em> là <strong>target</strong> đối với chính nó.</p>

<p>Trả về một mảng gồm <code>n</code> số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là số lượng node <strong>target</strong> <strong>lớn nhất</strong> có thể đối với node <code>i</code> của cây thứ nhất nếu nối một node của cây thứ nhất với một node khác trong cây thứ hai.</p>

<p><strong>Lưu ý</strong> rằng các truy vấn độc lập với nhau. Nghĩa là sau mỗi truy vấn, bạn sẽ xóa cạnh vừa thêm trước khi chuyển sang truy vấn tiếp theo.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges1 = [[0,1],[0,2],[2,3],[2,4]], edges2 = [[0,1],[0,2],[0,3],[2,7],[1,4],[4,5],[4,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[8,7,7,8,8]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>, nối node 0 của cây thứ nhất với node 0 của cây thứ hai.</li>
	<li>Với <code>i = 1</code>, nối node 1 của cây thứ nhất với node 4 của cây thứ hai.</li>
	<li>Với <code>i = 2</code>, nối node 2 của cây thứ nhất với node 7 của cây thứ hai.</li>
	<li>Với <code>i = 3</code>, nối node 3 của cây thứ nhất với node 0 của cây thứ hai.</li>
	<li>Với <code>i = 4</code>, nối node 4 của cây thứ nhất với node 4 của cây thứ hai.</li>
</ul>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3373.Maximize%20the%20Number%20of%20Target%20Nodes%20After%20Connecting%20Trees%20II/images/3982-1.png" style="width: 600px; height: 169px;" /></div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges1 = [[0,1],[0,2],[0,3],[0,4]], edges2 = [[0,1],[1,2],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,6,6,6,6]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mọi <code>i</code>, nối node <code>i</code> của cây thứ nhất với bất kỳ node nào của cây thứ hai.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3373.Maximize%20the%20Number%20of%20Target%20Nodes%20After%20Connecting%20Trees%20II/images/3928-2.png" style="height: 281px; width: 500px;" /></div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n, m &lt;= 10<sup>5</sup></code></li>
	<li><code>edges1.length == n - 1</code></li>
	<li><code>edges2.length == m - 1</code></li>
	<li><code>edges1[i].length == edges2[i].length == 2</code></li>
	<li><code>edges1[i] = [a<sub>i</sub>, b<sub>i</sub>]</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>edges2[i] = [u<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; m</code></li>
	<li>Đầu vào được tạo sao cho <code>edges1</code> và <code>edges2</code> biểu diễn các cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Không giống phần I, khoảng cách được thay bằng tính chẵn lẻ và $n,m \le 10^5$, nên chạy DFS cho từng node sẽ quá chậm.
>
> Các node target là những node có cùng màu với $i$, cộng với lớp màu lớn hơn của cây thứ hai. Cây là đồ thị hai phía, nên hai cách tô màu cho biết kích thước của cả hai lớp.
>
> Đáp án tại $i$ là $\textit{cnt1}[c1[i]]$ cộng với lớp lớn hơn của cây thứ hai.

<!-- thinking:end -->

Số lượng node target đối với node $i$ có thể chia thành hai phần:

- Số node trong cây thứ nhất có cùng tính chẵn lẻ của độ sâu với node $i$.
- Số node lớn nhất trong cây thứ hai có cùng tính chẵn lẻ của độ sâu.

Đầu tiên, ta dùng Depth-First Search (DFS) để tính số node trong cây thứ hai có cùng tính chẵn lẻ của độ sâu, ký hiệu là $\textit{cnt2}$. Sau đó, ta tính số node trong cây thứ nhất có cùng tính chẵn lẻ của độ sâu với node $i$, ký hiệu là $\textit{cnt1}$. Do đó, số lượng node target đối với node $i$ là $\max(\textit{cnt2}) + \textit{cnt1}$.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là số node trong cây thứ nhất và cây thứ hai.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTargetNodes(
        self, edges1: List[List[int]], edges2: List[List[int]]
    ) -> List[int]:
        def build(edges: List[List[int]]) -> List[List[int]]:
            n = len(edges) + 1
            g = [[] for _ in range(n)]
            for a, b in edges:
                g[a].append(b)
                g[b].append(a)
            return g

        def dfs(
            g: List[List[int]], a: int, fa: int, c: List[int], d: int, cnt: List[int]
        ):
            c[a] = d
            cnt[d] += 1
            for b in g[a]:
                if b != fa:
                    dfs(g, b, a, c, d ^ 1, cnt)

        g1 = build(edges1)
        g2 = build(edges2)
        n, m = len(g1), len(g2)
        c1 = [0] * n
        c2 = [0] * m
        cnt1 = [0, 0]
        cnt2 = [0, 0]
        dfs(g2, 0, -1, c2, 0, cnt2)
        dfs(g1, 0, -1, c1, 0, cnt1)
        t = max(cnt2)
        return [t + cnt1[c1[i]] for i in range(n)]
```

#### Java

```java
class Solution {
    public int[] maxTargetNodes(int[][] edges1, int[][] edges2) {
        var g1 = build(edges1);
        var g2 = build(edges2);
        int n = g1.length, m = g2.length;
        int[] c1 = new int[n];
        int[] c2 = new int[m];
        int[] cnt1 = new int[2];
        int[] cnt2 = new int[2];
        dfs(g2, 0, -1, c2, 0, cnt2);
        dfs(g1, 0, -1, c1, 0, cnt1);
        int t = Math.max(cnt2[0], cnt2[1]);
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = t + cnt1[c1[i]];
        }
        return ans;
    }

    private List<Integer>[] build(int[][] edges) {
        int n = edges.length + 1;
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        return g;
    }

    private void dfs(List<Integer>[] g, int a, int fa, int[] c, int d, int[] cnt) {
        c[a] = d;
        cnt[d]++;
        for (int b : g[a]) {
            if (b != fa) {
                dfs(g, b, a, c, d ^ 1, cnt);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maxTargetNodes(vector<vector<int>>& edges1, vector<vector<int>>& edges2) {
        auto g1 = build(edges1);
        auto g2 = build(edges2);
        int n = g1.size(), m = g2.size();
        vector<int> c1(n, 0), c2(m, 0);
        vector<int> cnt1(2, 0), cnt2(2, 0);

        dfs(g2, 0, -1, c2, 0, cnt2);
        dfs(g1, 0, -1, c1, 0, cnt1);

        int t = max(cnt2[0], cnt2[1]);
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            ans[i] = t + cnt1[c1[i]];
        }
        return ans;
    }

private:
    vector<vector<int>> build(const vector<vector<int>>& edges) {
        int n = edges.size() + 1;
        vector<vector<int>> g(n);
        for (const auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        return g;
    }

    void dfs(const vector<vector<int>>& g, int a, int fa, vector<int>& c, int d, vector<int>& cnt) {
        c[a] = d;
        cnt[d]++;
        for (int b : g[a]) {
            if (b != fa) {
                dfs(g, b, a, c, d ^ 1, cnt);
            }
        }
    }
};
```

#### Go

```go
func maxTargetNodes(edges1 [][]int, edges2 [][]int) []int {
    g1 := build(edges1)
    g2 := build(edges2)
    n, m := len(g1), len(g2)
    c1 := make([]int, n)
    c2 := make([]int, m)
    cnt1 := make([]int, 2)
    cnt2 := make([]int, 2)

    dfs(g2, 0, -1, c2, 0, cnt2)
    dfs(g1, 0, -1, c1, 0, cnt1)

    t := max(cnt2[0], cnt2[1])
    ans := make([]int, n)
    for i := 0; i < n; i++ {
        ans[i] = t + cnt1[c1[i]]
    }
    return ans
}

func build(edges [][]int) [][]int {
    n := len(edges) + 1
    g := make([][]int, n)
    for _, e := range edges {
        a, b := e[0], e[1]
        g[a] = append(g[a], b)
        g[b] = append(g[b], a)
    }
    return g
}

func dfs(g [][]int, a, fa int, c []int, d int, cnt []int) {
    c[a] = d
    cnt[d]++
    for _, b := range g[a] {
        if b != fa {
            dfs(g, b, a, c, d^1, cnt)
        }
    }
}
```

#### TypeScript

```ts
function maxTargetNodes(edges1: number[][], edges2: number[][]): number[] {
    const g1 = build(edges1);
    const g2 = build(edges2);
    const [n, m] = [g1.length, g2.length];
    const c1 = Array(n).fill(0);
    const c2 = Array(m).fill(0);
    const cnt1 = [0, 0];
    const cnt2 = [0, 0];

    dfs(g2, 0, -1, c2, 0, cnt2);
    dfs(g1, 0, -1, c1, 0, cnt1);

    const t = Math.max(...cnt2);
    const ans = Array(n);
    for (let i = 0; i < n; i++) {
        ans[i] = t + cnt1[c1[i]];
    }
    return ans;
}

function build(edges: number[][]): number[][] {
    const n = edges.length + 1;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    return g;
}

function dfs(g: number[][], a: number, fa: number, c: number[], d: number, cnt: number[]): void {
    c[a] = d;
    cnt[d]++;
    for (const b of g[a]) {
        if (b !== fa) {
            dfs(g, b, a, c, d ^ 1, cnt);
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn max_target_nodes(edges1: Vec<Vec<i32>>, edges2: Vec<Vec<i32>>) -> Vec<i32> {
        fn build(edges: &Vec<Vec<i32>>) -> Vec<Vec<i32>> {
            let n = edges.len() + 1;
            let mut g = vec![vec![]; n];
            for e in edges {
                let a = e[0] as usize;
                let b = e[1] as usize;
                g[a].push(b as i32);
                g[b].push(a as i32);
            }
            g
        }

        fn dfs(g: &Vec<Vec<i32>>, a: usize, fa: i32, c: &mut Vec<i32>, d: i32, cnt: &mut Vec<i32>) {
            c[a] = d;
            cnt[d as usize] += 1;
            for &b in &g[a] {
                if b != fa {
                    dfs(g, b as usize, a as i32, c, d ^ 1, cnt);
                }
            }
        }

        let g1 = build(&edges1);
        let g2 = build(&edges2);
        let n = g1.len();
        let m = g2.len();

        let mut c1 = vec![0; n];
        let mut c2 = vec![0; m];
        let mut cnt1 = vec![0; 2];
        let mut cnt2 = vec![0; 2];

        dfs(&g2, 0, -1, &mut c2, 0, &mut cnt2);
        dfs(&g1, 0, -1, &mut c1, 0, &mut cnt1);

        let t = cnt2[0].max(cnt2[1]);
        let mut ans = vec![0; n];
        for i in 0..n {
            ans[i] = t + cnt1[c1[i] as usize];
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int[] MaxTargetNodes(int[][] edges1, int[][] edges2) {
        var g1 = Build(edges1);
        var g2 = Build(edges2);
        int n = g1.Length, m = g2.Length;
        var c1 = new int[n];
        var c2 = new int[m];
        var cnt1 = new int[2];
        var cnt2 = new int[2];

        Dfs(g2, 0, -1, c2, 0, cnt2);
        Dfs(g1, 0, -1, c1, 0, cnt1);

        int t = Math.Max(cnt2[0], cnt2[1]);
        var ans = new int[n];
        for (int i = 0; i < n; i++) {
            ans[i] = t + cnt1[c1[i]];
        }
        return ans;
    }

    private List<int>[] Build(int[][] edges) {
        int n = edges.Length + 1;
        var g = new List<int>[n];
        for (int i = 0; i < n; i++) {
            g[i] = new List<int>();
        }
        foreach (var e in edges) {
            int a = e[0], b = e[1];
            g[a].Add(b);
            g[b].Add(a);
        }
        return g;
    }

    private void Dfs(List<int>[] g, int a, int fa, int[] c, int d, int[] cnt) {
        c[a] = d;
        cnt[d]++;
        foreach (var b in g[a]) {
            if (b != fa) {
                Dfs(g, b, a, c, d ^ 1, cnt);
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
